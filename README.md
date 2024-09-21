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
          - 使用sp正则化器，引入example weight vi和age参数λ（控制学习速度的超参数）
          * 当一个example的训练损失小于阈值λ时，则作为当前模型的一个简单数据
          * 训练过程中调整阈值λ
          * soft sp正则化器：线性、对数、混合、多项式等
          * 预嵌入式SPL：引入损失的先验知识
      * ##### Transfer Teacher
          * 首先在训练数据集或外部数据集上预训练teacher模型，然后转移其知识来计算样本难度
          * 基于损失：以teacher模型计算出的实例损失作为example难度，假设损失越低，example越容易
      * ##### RL Teacher
          * AutoCL：将一组学习进展定义为奖励，包括损失驱动和复杂性驱动的测量；如果在第i个任务训练后观察到student模型损失减少或复杂性增加，则该任务有助于student模型取得较大进展，应该分配较大的抽样概率
          * TSCL：将奖励设置为一个特定任务的学习曲线斜率的绝对值(两个连续时期的表现分数之间的绝对差异)，当斜率为大正值时，意味着学生在任务上取得进展；当斜率是一个大的负值时，就意味着学生忘记了这个任务。这两种情况都应该导致更大的抽样概率，以实现更快和更一般             化的学生训练。
          * L2T (Learning to Teach)：采用强化算法
      * ##### Other Automatic CL
          * MentorNet：具有参数的teacher模型
          * APL：通过生成对抗学习预测SPL中的损失权重v
          * meta-learning角度:
  * #### summary
     * SPL和Transfer Teacher只自动进行难度测量，仍使用预定义的Training Scheduler；而RL Teacher实现了全自动的CL设计。
### 如何选择合适CL
  * CL更适合噪声标签或极端值更多的场景以提高模型的鲁棒性和收敛速度；而HEM更有利于更cleaner的数据集，导致更快更稳定的SGD。如果目标任务很困难，CL将比HEM更可取，因为CL能够通过更容易/更平滑的版本产生更有效的训练过程。
  * 联合采用不同的CL方法使它们相互补充。
  * 许多全自动CL方法在弱监督的任务上明显比SPL方法更有效，而在这些论文中经常选择MentorNet作为基线。
  * 如果有足够的专家领域知识可用，那么预定义的CL方法更适合设计一个专门适合确切场景的知识驱动课程；如果对数据没有预先的假设，那么自动CL方法更适合学习自适应底层数据集和任务目标的数据驱动课程。
     
