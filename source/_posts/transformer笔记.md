---
title: transformer笔记
date: 2022-11-03 19:58:46
categories: 资料
math: true
tags:
---

<!-- TOC -->

- [Transformer笔记](#transformer笔记)
    - [transformer的提出](#transformer的提出)
    - [自注意力机制](#自注意力机制)
    - [多头注意力](#多头注意力)
    - [位置编码](#位置编码)
        - [Token Embedding](#token-embedding)
        - [Positional Embedding](#positional-embedding)
    - [Transformer网络结构](#transformer网络结构)
        - [Encoder](#encoder)
        - [Decoder](#decoder)
        - [注意力掩码机制](#注意力掩码机制)
    - [基于transformer的分类模型](#基于transformer的分类模型)

<!-- /TOC -->
# Transformer笔记
在写本篇博客时，主要参考了这篇博客（[博客链接](https://zhuanlan.zhihu.com/p/420820453)），博主介绍的非常细致，强烈推荐。
## transformer的提出

该模型的提出，来源于2017年的一篇论文：Attention is all you need

在论文中作者提到，当前主流的序列模型都是基于复杂的RNN或者CNN构造的Encoder-Decoder模型,这种模型使得下一个时刻的计算过程依赖于上一个时刻的输出，因此RNN在计算效率上受到很大限制。于是作者提出了transformer架构，他抛弃了RNN结构，引入了注意力机制来计算模型输入输出间的隐含表示。

## 自注意力机制

注意力机制可以描述为将query和一系列的key-value对映射到某个输出的过程，而这个输出的向量就是根据query和key计算得到的权重作用于value上的权重和。

![](https://pic2.zhimg.com/80/v2-21ebe709cd12dda0a9c9da7559d3e045_720w.webp)
可以看出，自注意力的计算过程就是通过Query和Key计算出权重，然后与Value相乘得到输出。

$$
Attention\left(Q,K,V\right)=softmax\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

其中Q,K,V分别是三个矩阵，而除以$\sqrt{d_k}$就是上图中的scale过程。作者发现某些情况下$QK^T$会得到很大的值，这会导致softmax后产生很小的梯度，不利于训练，所以加入scale的过程。

下面举例说明Q,K,V是怎么来的。

![](https://pic4.zhimg.com/80/v2-28903ee6a9c01d4895af7836b1e5997f_720w.webp)

从上图可以看出，Q，K，V其实就是输入X乘以三个不同的矩阵计算而来，即：
$$
Q=XW^q\\
K=XW^k\\
V=XW^v
$$
得到Q,K,V之后，就可以进一步计算得到权重向量。

![](https://pic4.zhimg.com/80/v2-3173490f0b8fb89b22a22b65d2851c7f_720w.webp)
假如Q和K的计算结果如上图，对于权重矩阵的第1行来说，0.7表示的就是“我”与“我”的注意力值；0.2表示的就是“我”与”是”的注意力值；0.1表示的就是“我”与“谁”的注意力值。换句话说，在对序列中的“我“进行编码时，应该将0.7的注意力放在“我”上，0.2的注意力放在“是”上，将0.1的注意力放在谁上。

不过，上面的规律也反映了一个小问题：**模型在对当前位置的信息进行编码时，会过度的将注意力集中于自身的位置而可能忽略了其它位置。**

得到权重矩阵之后，与V相乘：

![](https://pic3.zhimg.com/80/v2-a574d12396e1e2006716eb58f9fa5806_720w.webp)

对于“是”而言，它的编码向量其实就是“我，是，谁”三个字的加权和，如下图所示：

![](https://pic2.zhimg.com/80/v2-3de3645627bd94e4258a89c4376227c1_720w.webp)

这种自注意力机制,确实解决了作者在论文中所提出的“传统序列模型在编码过程中都需顺序进行的弊端”的问题，有了自注意力机制后，仅仅只需要对原始输入进行几次矩阵变换便能够得到最终包含有不同位置注意力信息的编码向量。

## 多头注意力

之前提到，自注意力在对当前位置的信息进行编码时，会过度的将注意力集中于自身的位置，所以提出了Multi-head Attention来进行改进。

![](https://pic4.zhimg.com/80/v2-849a1a08e54a4ab3b2bfadfaceddfeab_720w.webp)

多头注意力机制其实就是将原始的输入序列进行多组的自注意力处理过程；然后再将每一组自注意力的结果拼接起来进行一次线性变换得到最终的输出结果。具体的，其计算公式为：
$$
MultiHead(Q,K,V)=Concat(head_1,...,head_h)W^O\\\\
Where head_i=Attention(QW_i^Q,KW^K_i,VW_i^V)
$$
作者使用了个并行的自注意力模块（8个头）来构建一个注意力层，论文中所使用的多头注意力机制其实就是将一个大的高维单头拆分成了h个多头。

![](https://pic4.zhimg.com/80/v2-382a68f2a5543f00b7a4a1fd84e29b83_720w.webp)

当进行进行注意力权重矩阵计算时，h越大那么Q,K,V就会被切分得越小，进而得到的注意力权重分配方式越多，如图所示。

![](https://pic1.zhimg.com/80/v2-8837a813f9028886bb8529a59c6cd9c8_720w.webp)

因而多头这一做法也恰好用于克服模型在对当前位置的信息进行编码时，会过度的将注意力集中于自身的位置的问题，使得权重分配更合理。

## 位置编码

### Token Embedding

在对文本相关的数据进行建模时首先要做的便是对其进行向量化(Embedding),在深度学习中，更常见的做法便是将各个词（或者字）通过一个Embedding层映射到低维稠密的向量空间。因此，在Transformer模型中，首先第一步要做的同样是将文本以这样的方式进行向量化表示，并且将其称之为Token Embedding，也就是深度学习中常说的词嵌入（Word Embedding）

![](https://pic4.zhimg.com/80/v2-9e4cb60bd2ed3e1aca137928bf34a537_720w.webp)

如果是换做之前的网络模型，例如CNN或者RNN，那么对于文本向量化的步骤就到此结束了，因为这些网络结构本身已经具备了捕捉时序特征的能力,但是这对仅仅只有自注意力机制的网络结构来说却不行。自注意力机制在实际运算过程中不过就是几个矩阵来回相乘进行**线性变换**而已。因此，这就导致即使是打乱各个词的顺序，那么最终计算得到的结果本质上却没有发生任何变换，换句话说仅仅只使用自注意力机制会丢失文本原有的序列信息（~~换个顺序学习不到新的东西~~）。

下面举例说明原因：

![](https://pic4.zhimg.com/80/v2-a61fbde8b2b7958eef3dd134f722c69b_720w.webp)

经过词嵌入表示后，序列“我 在 看 书”经过了一次线性变换。现在，我们将序列变成“书 在 看 我”，然后同样以中间这个权重矩阵来进行线性变换。

![](https://pic4.zhimg.com/80/v2-9aede5435fe5c6f010457c5818ec3ce3_720w.webp)

所以，序列在交换位置前和交换位置后计算得到的结果在本质上并没有任何区别，仅仅只是交换了对应的位置。因此，基于这样的原因，Transformer在原始输入文本进行Embedding后，又额外的加入了一个Positional Embedding来刻画数据在时序上的特征。
### Positional Embedding

先来通过一幅图直观看看经过Positional Embedding处理后到底产生了什么样的变化。

![](https://pic3.zhimg.com/80/v2-8110b24565ffbfb028866a19050940fa_720w.webp)

>如图所示，横坐标表示输入序列中的每一个Token，每一条曲线或者直线表示对应Token在每个维度上对应的位置信息。在左图中，每个维度所对应的位置信息都是一个不变的常数；而在右图中，每个维度所对应的位置信息都是基于某种公式变换所得到。换句话说就是，左图中任意两个Token上的向量都可以进行位置交换而模型却不能捕捉到这一差异，但是加入右图这样的位置信息模型却能够感知到。例如位置20这一处的向量，在左图中无论你将它换到哪个位置，都和原来一模一样；但在右图中，你却再也找不到与位置20处位置信息相同的位置。

下面再看一个例子

![](https://pic1.zhimg.com/80/v2-9a4be9596e49f523e90c2c9eb7b37864_720w.webp)

原始输入在经过Token Embedding后，又加入了一个常数位置信息的的Positional Embedding。在经过一次线性变换后便得到了图2-5左右边所示的结果。接下来，我们再交换序列的位置，并同时进行Positional Embedding观察其结果。

![](https://pic3.zhimg.com/80/v2-d98e246f487827ee1ad6411cc00928e6_720w.webp)

可以看到，交换位置后得到的权重矩阵只是发生了普通的线性变换，说明上述类型的PE是无效的。

在Transformer中，作者采用了以下公式所示的规则来生成各个维度的位置信息。
$$
PE_{pos,2i}=sin\left(\frac{pos}{10000^{\frac{2i}{d_{model}}}}\right)\\
PE_{pos,2i+1}=cos\left(\frac{pos}{10000^{\frac{2i}{d_{model}}}}\right)
$$

上式中，$PE$就是embedding的矩阵，$pos$代表某一个位置,$i$代表某一个维度，在融入这种非常数的Positional Embedding位置信息后，便可以得到下图结果。

![](https://pic3.zhimg.com/80/v2-0c62651daf9f5c0c1de77e9137b058b6_720w.webp)

这就说明通过Positional Embedding可以弥补自注意力机制不能捕捉序列时序信息的缺陷(~~感觉类似于激活层的作用~~)。

## Transformer网络结构

整体结构如下：

![](https://pic1.zhimg.com/80/v2-6da4c9720ec88860295bd63e19344820_720w.webp)

### Encoder

![](https://pic3.zhimg.com/80/v2-5da93887d468045677e9983f14b35db6_720w.webp)

主要由多头注意力机制和两层前馈神经网络构成，并且都加入了残差连接，并进行了层归一化。其中前馈神经网络主要用于变换维度，增强模型表达能力。

### Decoder

![](https://pic1.zhimg.com/80/v2-8ab2c37e224137c05332419d525c88cc_720w.webp)

不同于Encoder部分，在Decoder中一共包含有3个部分的网络结构。最上面的和最下面的部分（暂时忽略Mask）与Encoder相同，只是多了中间这个与Encoder输出（Memory）进行交互的部分，作者称之为“Encoder-Decoder attention”。**对于这部分的输入，Q来自于下面多头注意力机制的输出，K和V均是Encoder部分的输出（Memory）经过线性变换后得到。**

具体解码过程如下：

首先，待解码向量和Memory分别各自乘上一个矩阵后得到Q、K、V。

![](https://pic2.zhimg.com/80/v2-22669eedb0931017b8ed1d56dcd28089_720w.webp)



![](https://pic4.zhimg.com/80/v2-56ab4338fc9da4ca495fe070904846cf_720w.webp)

在解码第1个时刻时，首先Q通过与K进行交互得到权重向量，此时可以看做是**Q（待解码向量）在K（本质上也就是Memory）中查询Memory中各个位置与Q有关的信息**；然后将权重向量与V进行运算得到解码向量，此时这个解码向量可以看作是**考虑了Memory中各个位置编码信息的输出向量**，也就是说它包含了在解码当前时刻时应该将注意力放在Memory中哪些位置上的信息。

在得到这个解码向量并经过图2-10中最上面的两层全连接层后，便将其输入到分类层中进行分类得到当前时刻的解码输出值。

### 注意力掩码机制

模型在实际的预测过程中只是将当前时刻之前（包括当前时刻）的所有时刻作为输入来预测下一个时刻，也就是说模型在预测时是看不到当前时刻之后的信息。因此，Transformer中的Decoder通过加入注意力掩码机制（Masked Multi-Head Attention）来解决了这一问题。

![](https://pic4.zhimg.com/80/v2-ef714b246dfc08c912db18aaec0542cb_720w.webp)

如图所示，左边依旧是通过Q和K计算得到了注意力权重矩阵（此时还未进行softmax操作），而中间的就是所谓的注意力掩码矩阵，两者在相加之后再乘上矩阵V便得到了整个自注意力机制的输出。（因为attention mask的加入，使得softmax之后，后面时刻的权重变为0，正好屏蔽了当前时刻之后的输入。）

## 基于transformer的分类模型

![](https://moonhotel.oss-cn-shanghai.aliyuncs.com/images/2107052058585050890.jpg)

上图便是一个基于Transformer结构的文本分类模型。不过准确的说应该只是一个基于Transformer中Encoder的文本分类模型。这是因为在文本分类任务中并没有解码这一过程，所以我们只需要将Encoder编码得到的向量输入到分类器中进行分类即可。