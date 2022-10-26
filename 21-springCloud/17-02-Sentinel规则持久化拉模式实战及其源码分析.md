## Sentinel规则持久化拉模式实战及其源码分析

课程内容：

1. Sentinel规则持久化三种模式分析
2. 控制台是如何推送规则到客户端内存中的
3. 原始模式下规则推送的源码分析
4. Sentinel规则持久化扩展点分析
5. 动态规则扩展之读写数据源的实现
6. 客户端拉模式规则持久化实战
7. 拉模式改造之如何整合Spring Cloud



### 以流控规则为例

校验slot:   FlowSlot

**规则实体： FlowRule**

缓存规则： FlowRuleManager.getFlowRuleMap()

```
Map<String, List<FlowRule>> flowRules = new HashMap<>()
```

加载规则： FlowRuleManager.loadRules(rules)



### 控制台是如何推送规则到客户端内存中的

```
FlowControllerV1： /v1/flow/rule
```

**规则实体： FlowRuleEntity**

缓存规则： 基于内存模式  allRules   machineRules  appRules

接口： RuleRepository       规则持久化的扩展点（考虑）

发布缓存： 推送到微服务端内存中

- 重要：  FlowRuleEntity ——》FlowRule
- 发起请求：  http://192.168.3.1:8720/setRules



微服务端：

ModifyRulesCommandHandler#handle

加载规则: FlowRuleManager.loadRules(flowRules)

接口： WritableDataSource    规则持久化的扩展点



### 拉模式改造之如何整合Spring Cloud

扩展点：

spi

spring:

- beanPostProcessor  beanFactoryPostProcessor 
- SmartInitializingSingleton
- ApplicationListener
- FactoryBean     getObject

springboot 

- ApplicationRunner



