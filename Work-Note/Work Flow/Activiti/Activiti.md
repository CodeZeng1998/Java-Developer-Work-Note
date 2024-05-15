# Activiti 

[TOC]





## 会签

### （1）多用户审批，其中一人审批即可进入下一节点

相关设置：

* 多实例类型：Parallel
* 集合（多实例）：userCodeList
* 元素变量（多实例）：userCode
* 完成条件：${nrOfCompletedInstances>=1}
* 代理人：${userCode}



调用类RuntimeService里的startProcessInstanceByKey即可获取依据模型图生成的实例。

${nrOfCompletedInstances/nrOfInstances > 0}

