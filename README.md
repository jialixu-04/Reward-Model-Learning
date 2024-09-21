# Table of Content
* [Curriculum learning](#curriculum-learning)



## Curriculum learning
### CL应用场景
  1. guide
  2. denoise
### CL框架
  * **Difficulty measurer + Training scheduler**
### CL分类
  * #### Predefined CL
      * ##### Common Types of Predefined Difficulty Measurer
          * 具有更高复杂性的example有更高的维度，难度越高
          * 高多样性会导致高难度training
          * 噪声水平高的数据更困难
      * ##### Common Types of Predefined Training Scheduler
          * ###### 离散调度器
              * Baby Step
                1. 将排序后的数据分成从简单到困难的buckets，然后开始用最简单的bucket训练
                2. 经过固定数量的training epochs或者convergence后，下一个bucket被合并到训练子集中
              * One-Pass
                1. 将排序后的数据分成从简单到困难的buckets，然后开始用最简单的bucket训练
                2. 在更新时，丢弃当前的bucket，并切换到下一个更难得bucket进行训练
          * ###### 连续调度器
              * 看做是一个函数λ(t)将训练epoch数映射到标量λ ∈ (0,1]，λ(t)单调且非递减
              * root函数
  * #### Automatic CL
      * ##### Self-Paced Learning (SPL)
      * ##### Transfer Teacher
      * ##### RL Teacher
      * ##### Other Automatic CL
