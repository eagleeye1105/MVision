# Attention Net Attention Model（注意力模型）

[台大李宏毅老师 Machine Learning, Deep Learning and Structured Learning 包含RNN Attention模块](http://speech.ee.ntu.edu.tw/~tlkagk/courses_MLSD15_2.html)

[浅谈Attention-based Model【原理篇】 上述课程 部分笔记](https://blog.csdn.net/u010159842/article/details/80473462)

[完全图解RNN、RNN变体、Seq2Seq、Attention机制-知乎](https://zhuanlan.zhihu.com/p/28054589)

Attention即为注意力，人脑在对于的不同部分的注意力是不同的。需要attention的原因是非常直观的，比如，我们期末考试的时候，我们需要老师划重点，划重点的目的就是为了尽量将我们的attention放在这部分的内容上，以期用最少的付出获取尽可能高的分数；再比如我们到一个新的班级，吸引我们attention的是不是颜值比较高的人？普通的模型可以看成所有部分的attention都是一样的，而这里的attention-based model对于不同的部分，重要的程度则不同。

Attention-based Model其实就是一个相似性的度量，当前的输入与目标状态越相似，那么在当前的输入的权重就会越大，说明当前的输出越依赖于当前的输入。严格来说，Attention并算不上是一种新的model，而仅仅是在以往的模型中加入attention的思想，所以Attention-based Model或者Attention Mechanism是比较合理的叫法，而非Attention Model。

> 从Attention的作用角度出发，Attention分为两类： 

* **1.空间注意力 Spatial Attention，同一时期不同部分的关联**
* **2.时间注意力 Temporal Attention，不同时期内容的关联**

这样的分类更多的是从应用层面上，而从 Attention的作用方法上，可以将其分为 Soft Attention 和 Hard Attention，这既我们所说的， Attention输出的向量分布是一种one-hot的独热分布还是soft的软分布，这直接影响对于上下文信息的选择作用。

深度学习里的Attention model其实模拟的是人脑的注意力模型，举个例子来说，当我们观赏一幅画时，虽然我们可以看到整幅画的全貌，但是在我们深入仔细地观察时，其实眼睛聚焦的就只有很小的一块，这个时候人的大脑主要关注在这一小块图案上，也就是说这个时候人脑对整幅图的关注并不是均衡的，是有一定的权重区分的。这就是深度学习里的Attention Model的核心思想。

Attention模型最初应用于图像识别，模仿人看图像时，目光的焦点在不同的物体上移动。当神经网络对图像或语言进行识别时，每次集中于部分特征上，识别更加准确。如何衡量特征的重要性呢？最直观的方法就是权重，因此，Attention模型的结果就是在每次识别时，首先计算每个特征的权值，然后对特征进行加权求和，权值越大，该特征对当前识别的贡献就大。 

[RAM： Recurrent Models of Visual Attention 学习笔记](https://blog.csdn.net/c602273091/article/details/79059445)

RAM model讲得是视觉的注意力机制，说人识别一个东西的时候，如果比较大的话，是由局部构造出整体的概念。人的视觉注意力在选择局部区域的时候，是有一种很好的机制的，会往需要更少的步数和更能判断这个事物的方向进行的，我们把这个过程叫做Attention。由此，我们把这个机制引入AI领域。使用RNN这种可以进行sequential decision的模型引入，然后因为在选择action部分不可导，因为找到目标函数无法进行求导，只能进采样模拟期望，所以引入了reinforcment leanrning来得到policy进而选择action。

首先输入时一副完整的图片，一开始是没有action的，所以随机挑选一个patch，然后送入了RNN网络中，由RNN产生的输出作为action，这个action可以是hard attention，就是根据概率a~P(a|X)进行采样，或者是直接由概率最大的P(a|X)执行。有了action以后就可以从图片中选择某个位置的sub image送到RNN中作为input，另外一方面的input来自于上一个的hidden layer的输出。通过同样的网络经过T step之后，就进行classification，这里得到了最终的reward，（把calssification是否判断正确作为reward）就可以进行BPTT，同时也可以根据policy gradient的方法更新policy function。可以发现这个网络算是比较简单，也只有一个hidden layer，我觉得应该是加入了RL之后比较难训练。


将卷积神经网络应用于大型图像的计算量很大，因为计算量与图像像素数成线性关系。我们提出了一种新颖的循环神经网络模型，可以从图像或视频中提取信息，方法是自适应地选择一系列区域或位置，并仅以高分辨率处理选定区域。与卷积神经网络一样，所提出的模型具有内置的平移不变性程度，但其执行的计算量可以独立于输入图像大小进行控制。虽然模型是不可区分的，但可以使用强化学习方法来学习，以学习特定于任务的策略。

人类感知的一个重要特性是不倾向于一次处理整个场景。 相反，人类有选择地将注意力集中在视觉空间的某些部分上，以获取需要的信息，并随时间将不同视角的信息相结合，以建立场景的内部表示，指导未来的眼球运动和决策制定。 由于需要处理更少的“像素”，因此将场景中的部分计算资源集中在一起可节省“带宽”。 但它也大大降低了任务的复杂性，因为感兴趣的对象可以放置在固定的中心，固定区域之外的视觉环境（“杂乱”）的不相关特征自然被忽略。

该模型是一个循环神经网络（RNN），它按顺序处理输入，一次一个地处理图像（或视频帧）内的不同位置，并递增地组合来自这些注视的信息以建立场景的动态内部表示，或环境。基于过去的信息和任务的需求，模型不是一次处理整个图像甚至是边界框，而是在每一步中选择下一个要注意的位置。我们的模型中的参数数量和它执行的计算量可以独立于输入图像的大小来控制，而卷积网络的计算需与图像像素的数量线性地成比例。我们描述了一个端到端的优化程序，该程序允许模型直接针对给定的任务进行训练，并最大限度地提高可能取决于模型做出的整个决策序列的性能测量。该过程使用反向传播来训练神经网络组件和策略梯度以解决由于控制问题导致的非差异性。

我们表明，我们的模型可以有效的学习特定于任务的策略，如多图像分类任务以及动态视觉控制问题。 我们的结果还表明，基于关注的模型可能比卷积神经网络更好地处理杂波和大输入图像。

对于对象检测，已经做了很多工作来降低广泛的滑动窗口范例的成本，主要着眼于减少评估完整分类器的窗口的数量.

循环注意力模型 Attention rnn 

> 单个神经元
![](https://github.com/Ewenwan/MVision/blob/master/CNN/AttentionNet/img/single_cnn.jpg)

