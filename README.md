- 图像分类的核心图像分类的核心是对输入的图像进行检测，从给定的数据集中寻找最合适的标签.
#####  传统的图像分类模型
- 词袋模型的物体分类方法；后来引入了深度学习模型分类方法。1.词袋模型通常包括底层特征提取、特征编码、空间约束、分类器设计、模型融合等几个阶段。2.深度学习模型的基本思想是通过有无监督的方式学习层次化的特征表达，来对物体进行从底层到高层的描述，深度学习模型包括自动编码器、受限玻尔兹曼机、深度信念网络、卷积神经网络、生物启发式模型。后来自2012年CNN席卷图像分类领域，比如Lenet、vggnet、resnet等网络模型使图像分类问题得到了基本解决。
##### 基于Transformer的新突破
- 由于近年来transformer模型在NLP领域大放异彩的缘故，研究人员决定在CV领域也尝试进行相关的研究。20年Google的一篇论文《 An Image is Worth 16*16 Words: Transformers for Image Recognition at Scale》提出了ViT模型，该模型使用了纯transformer模型，同样得到了足以比肩以resnet为代表的CNN模型。可同样，其缺点也很明显，对数据的依赖是他的短板之一。在数据集不够充足时，他的训练结果比resnet低了几个百分点。
##### Transformer模型构造
- 一个encoder与一个decoder模块构成了transformer，多个相同的编/解码器共同构成了一个完整的网络。transformer的基本组成是self-attention，每个编码器都有一个自注意力层和一个前馈神经网络；而每个解码器有一个自注意力层。在自注意力层，分为scaled dot-product attention和multi-head attention.进行scaled dot-product attention时,先将输入向量首先被三个投影矩阵转换成三个不同的向量：q向量、k向量、v向量。之后将q向量与k向量做点积,并将结果归一化,再利用softmax函数求分布概率,再乘以V向量,之后求向量评分.multi-head attention的工作原理其实是求h个不同的scaled dot-product attention结果的集合.第一步是将数据输入到h个不同的scaled dot-product attention中,得到h个不同的加权特征矩阵,并拼接成一个特征矩阵,之后经过一层全连接并输出.
- ![transformer_self-attention](https://user-images.githubusercontent.com/66305624/113574985-c49ae380-964f-11eb-8ccc-17df11f9f4ec.jpg)
##### 模型实现:
- 现阶段展现初步成效的有ViT、iGPT、Deit  Swin Transformer等.其中ViT模型是一种基于纯Transformer的结构，并没有使用卷积，并且以transformer为核心的ViT模型对全局信息较为关注,实现长距离的依赖,,相比之下,CNN则更为注重局部信息.它的性能同样达到甚至超越了ResNet等CNN模型在图像分类上的成果。以ViT为例，该模型需要先将图片进行分块，由于transformer最开始是在NLP领域中应用的，也因此要二维的图片进行处理需要先将图片分为一个个块（patch），之后再经由transformer机制处理，最后得到结果。ViT是将输入的图片拆分为16*16个patches,每个patch做一次线性变化降维时嵌入位置信息,然后送入transformer.ViT在transformer输入序列前增加一个额外科学系的类标记位,以该位置的编译器输出为图像特征.ViT舍弃了CNN的归纳偏好问题,使得其在充足数据训练下具有良好的性能.甚至在某些测试下拥有不输于SOTA的表现.
