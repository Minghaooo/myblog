---
title: UVM-01-Basics
date: 2020-08-10 15:11:21
tags:
---

拖延症患者开始看UVM了。
<!--more-->
### UVM Factory
#### Registration
一个component 或者 object 必须包含 factory 的 registration code 
* uvm_component_registry wrapper 并且 typedef type_id
* static function to get type_id
* a function to get type name
  
#### construction default
uvm_component and uvm_object 中的constructor 是virtual 类型的，在使用之前必须被implement.   

### Phase
UVM 的执行顺序分成3个phase， 分别是build，run 和 clean up phase.     

#### Starting UVM Phase Execution
run_test() method is called from the static part of the test bench.

##### Build Phase
build start phase are executed at the start of the UVM test bench simulation. 

###### build 
once the root node component node is constructed, the build phase starts to execture. Each component of each component is deferred so that each layer can be configured by the level above. during the build phase, uvm_components are indirectly constructed using UVM factory. 
##### connect 
make TLM connections 

