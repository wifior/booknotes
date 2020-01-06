# Quartz任务调度

## 一、概念

Quartz是OpenSymphony开源组织在Job scheduling领域又一个开源项目 ，

简而言之，它是java实现的一个任务调度框架。

## 二、运行环境

Quartz可以运行嵌入在一个独立式应用程序，也可在应用服务内。

## 三、设计模式

Builder模式，工厂模式，组件模式，链式编程

## 四、核心概念

- 任务Job

Job就是你要实现的任务。

- 触发器Trigger

执行的条件，有两种一种是SimpleTrigger和CronTrigger两种。

- 调度器Scheduler

  它将任务Job及触发器Trigger整合起来，负责基于Trigger设定的时间来执行Job

## 五、常用api

Scheduler用于与调度程序交互的主程序接口。

Job我们预先定义的希望在未来时间能被 调度执行的任务类。

JobDetail使用JobDetail来定时任务的实例。通过JobBuilder类创建 

JobDataMap可以包含不限量的数据对象。

Trigger触发器，来触发执行Job

JobBuilder用于声明一个任务的实例。

TriggerBuilder触发器创建器，用于创建触发器Trigger实例。













