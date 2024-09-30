
## L02 引擎架构分层

5+1

Tool Layer

Function Layer

Resources Layer

Core Layer

Platform Layer

3rd Party Libraries

1. **Resources Layer - How to Access My Data**

   (1) import，将来自各种软件产出的资源文件的格式转化为高效的引擎文件格式，访问速度上升

   resources --import--> assets

   (2) build a composite asset file to refer all resources 表示几个ast之间的关系（references）

   (3) GUID ast 的全局编号

   (4) Runtime Asset Manager

   ​	a virtual file system to load/unload ast by path reference

   ​	manage ast lifespan and reference by handle system

2. **Function Layer - How to Make the World Alive**

   (1) tickMain 逻辑帧&渲染帧，要严格区分

   ​	tickLogic

   ​	tickRender

   (2) 未来趋势一定是多核架构

3. **Core Layer - Math Library/ Data Structure& Containers**

   (1) 数学计算效率优先

   (2) 数据结构也要重新实现，优化且可控，访问效率更高

   (3) 内存管理

4. **Platform Layer - Target on Different Platform**

5. **Tool - Allow Anyone to Create Game**

## L04  游戏引擎中的渲染实践

**SIMD**

单指令流多数据流（英语：Single Instruction Multiple Data，[缩写](https://baike.baidu.com/item/缩写/0?fromModule=lemma_inlink)：SIMD）是一种采用一个[控制器](https://baike.baidu.com/item/控制器/0?fromModule=lemma_inlink)来控制多个[处理器](https://baike.baidu.com/item/处理器/0?fromModule=lemma_inlink)，同时对一组数据（又称“数据向量”）中的每一个分别执行相同的操作从而实现空间上的[并行](https://baike.baidu.com/item/并行/0?fromModule=lemma_inlink)性的技术。

GPU拥有强大的并发处理能力和可编程流水线，面对单指令流多数据流时，运算能力远超传统CPU。

通常CPU的数据单向传输给GPU，尽量不从GPU中读数据。

Cache缓存

-> 需要学一下显卡的运行原理

**SIMT**

