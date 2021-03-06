# Dos and Don'ts of Machine Learning in Computer Security——阅读报告

在尝试解决某些网络安全问题时，例如恶意样本识别，网络恶意流量识别，风控安全、渗透测试、二进制代码分析等，机器学习的表现有时不是那么的尽如人意。

对其原因，这篇文章分析了机器学习研究网络安全问题出现的普遍性的问题，其可能导致一系列的性能和解释性问题，进而影响我们对于安全问题的理解。文章首先将机器学习解决安全问题的流程划分为六大步骤，

1. Security problem
2. Data collection
3. System design and learning
4. Performance evaluation
5. Deployment and operation
6. Security solution

其中，主要的问题出现在中间的四步中：

1. Data collection and labeling，数据收集与标注
2. System design and learning，系统设计与学习过程
3. Performance evaluation，表现评估
4. Deployment and operation，实际部署与操作

文章接着分别探讨了其中的常见问题

## 一、Data collection and labeling，数据收集与标注

Sampling bias，样本偏差这个问题在于，如果收集的数据不足代表底层安全问题的真实数据分布，如果没有有效地表示输入空间甚至遵循相似分布，则无法从训练数据中汲取任何有意义的结论。安全问题的一大难点在于数据的获取特别具有挑战性，而且通常需要使用不同质量的多个来源，因此会导致严重的样本偏差问题。

Label Inaccuracy，不准确的标签分类任务需要的人工标注标签是不准确的，不稳定的，或错误的，严重影响了基于学习的系统的整体性能。

## 二、System design and learning，系统设计与学习过程

一旦收集到足够的数据，就可以训练基于学习的安全系统。 这个过程包括从数据预处理到提取有意义的特征和构建有效的学习模型。 在这些步骤中的每一步都可能引入缺陷或偏差。

Data Snooping，数据泄露机器学习通常的做法是在生成学习模型之前，将收集的数据拆分为单独的训练和测试集。尽管拆分数据看起来很简单，但测试数据（或其它通常不可用的背景信息)会以许多微妙的方式影响训练过程，从而导致数据泄露。

False Causality，错误的因果推理安全任务时常会使用具有不可解释的特征空间来构建复杂学习模型，从而导致产生了错误的因果关系。

Biased Parameter Selection，参数选择的偏差机器学习方法的最终参数在训练时并不完全固定，而是间接依赖于测试集。例如，使用未校准的评价指标/标准或使用测试集指导参数设定可能会产生误导性的结果，进而导致在实际环境下的差异性，这被称为是参数选择的偏差。

## 三、Performance evaluation，表现评估

Inappropriate Baseline，不适当的基线方法评估是在不使用或使用有限的基线方法的情况下进行的，因此，无法证明对现有技术和其他安全机制的改进。在机器学习中，没有一种通用的算法可以超越所有其它的方法。其次，过于复杂的学习方法不仅会增加过拟合的可能性，还会增加运行时开销、攻击面以及部署时间和成本。

Inappropriate Performance Measures，不适当的评估指标性能测量不考虑应用场景的约束，在实际场景的部署中，恶意样本的极少量，导致了严重的数据不平衡问题，使得模型实际检测性能大大降低。

Base Rate Fallacy，基准比例错误在解释性能度量时忽略了大的类数量的不平衡，从而导致对性能的过高估计。

## 四、Deployment and operation，实际部署与操作

Lab-Only Evaluation，仅在实验室做出评估以学习为基础的系统仅在实验室环境中进行评估，而不讨论其实际局限性。

Inappropriate Threat Model，不恰当的威胁模型没有考虑机器学习的安全性，使系统暴露于各种攻击，如中毒和逃避攻击。

## 五、建议与改进

一、Data collection and labeling，数据收集与标注尽量以真实环境中的比例来收集样本，如果实在无法贴合真实环境情况，应对真实的数据分布情况做出多种假设，并分别进行实验使用迁移学习方法弥补在一个领域内难以采集足够数据的问题谨慎地对待公开的数据集，应将其主要用于和过去方法的对比尽可能地验证标签的真实性不能简单地去除标签不确定的数据，应想办法进行处理；预防数据随时间变化而带来的标签偏移

二、System design and learning，系统设计与学习过程使用可解释的机器学习技术以检查模型是否依赖于错误的因果关系从最开始就严格区分训练集、验证集、测试集，防止测试集在任何阶段干预到系统的构建

三、Performance evaluation，表现评估选择的评价指标应有助于从业人员在部署期间评估安全系统的性能安全问题通常是围绕检测罕见事件展开，可以选择不受类不平衡影响的指标，如精确召回曲线在选择基线对比方法时，需要考虑简单的机器学习模型（如线性分类器、朴素贝叶斯等）和非机器学习模型

四、Deployment and operation，实际部署与操作应将系统部署于真实环境下观察在实验室环境下观察不到的问题，分析真实数据的复杂性与多样性以对系统进行调整应假设基于机器学习的系统将面临对抗性的考验，考虑机器学习各个搭建步骤中可能存在漏洞；假设遭到白盒攻击这种最差的情况来做预案，搭建系统时遵守Kerckhoff原则（密码体制应该对外公开，仅需对密钥进行保密；如果一个密码系统需要保密的越多，可能的弱点也越多)

## 六、总结

该文章就机器学习在解决网络安全问题面临的困境做出了深入的剖析，并提出了自己的解决方案，为后续使用机器学习方法解决网络安全问题的研究指明了方向。