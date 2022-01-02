# RFC

### 团队成员

[qihonggang](https://github.com/qihonggang)，就职于小鹏汽车云原生团队，监控告警方向，对时序数据库感兴趣

[RichieChan](https://github.com/RichieChan)，就职于小鹏汽车云原生团队，链路可观测性方向，对数据库开发感兴趣

[slsjnp](https://github.com/slsjnp)，就职于奥卡云数据科技，分布式文件系统方向，分布式存储发烧友

[wfnuser](https://github.com/wfnuser)，花名微扰理论，EMQ X 工程师，开源爱好者 

### 项目介绍

TiDB 指标离群值异常检测，摆脱监控过程中难以配置阈值的烦恼。

### 背景&动机

- Prometheus 告警的典型做法是通过配置阈值来触发告警，但在一些场景中很难配置阈值来告警。比如物联网设备 MQTT 连接数正常情况是呈周期性变化，当突然下降或上升，很难精准确定异常阈值并及时准确触发告警。
- TiDB 中一定也有类似上述场景，难以配置阈值的周期性指标，本项目旨在对这些指标进行准确且及时的告警。

### 项目设计

![image](https://user-images.githubusercontent.com/9431724/147870212-a59ebdd7-fed1-4dd4-ac4d-115bfbf0fd22.png)

- 以 TiDB Metrics 时序数据为数据集，通过机器学习算法（傅里叶、时间序列预测等）预测动态阈值范围，并将结果保存在新的指标中。
- 比较指标实时数据是否在预测的动态阈值范围内，从而判断是否是异常值，如果是异常值，利用 AlertManger 触发告警。
- 利用 Chaos Mesh 演练故障，以便训练和测试预测模型的准确性。
- 参考开源项目 https://github.com/AICoE/prometheus-anomaly-detector ，应用到 TiDB 异常检测场景中。
