---
title: Arabesque:A System for Distributed Graph Mining
categories:
    -arcticals
tags: [Graph Mining,distribute system]
date: 2017-10-30 22:35:37

---

## Arabesque: A System for Distributed Graph Mining
> ### Arabesque:分布式的图挖掘系统


#### 原文链接：[点我跳转](https://github.com/jChanJi/jchanji.github.com/blob/master/meterial/093-teixeira.pdf)

## 目录

* [Abstract](#Abstract)

* 1.[Introduction](#introduction)

<!--more-->

* 2.Graph Mining Problems

* 3.The Filter-Process Model

    * 3.1.Computational Model

    * 3.2.Alternative Paradigms: Think Like a Vertex and Think Like a Pattern

* 4.Arabesque: API, Programming, and Implementation
    * 4.1 Arabesque API

    * 4.2 Programming with Arabesque

    * 4.3 Arabesque implementation

* 5.Graph Exploration Techniques
    * 5.1 Coordination-Free Exploration Strategy

    * 5.2 Storing Embeddings Compactly

    * 5.3 Partitioning Embeddings for Load Balancing

    * 5.4 Two-Level Pattern Aggregation for Fast Pattern Canonicality Checking
* 6.Evaluation
    * 6.1 Experimental Setup

    * 6.2 Alternative Paradigms: TLV and TLP

    * 6.3 Arabesque: The TLE Paradigm

    * 6.4 Large Graphs with Arabesque

* 7.Related Work

* 8.Conclusions

* 9.Acknowledgments

* 10.References

### <span id="Abstract">Abstract</span>



#### 原文

#### Abstract
  Distributed data processing platforms such as MapReduce
and Pregel have substantially simplified the design and deployment
of certain classes of distributed graph analytics algorithms.
However, these platforms do not represent a good
match for distributed graph mining problems, as for example
finding frequent subgraphs in a graph. Given an input
graph, these problems require exploring a very large number
of subgraphs and finding patterns that match some “interestingness”
criteria desired by the user. These algorithms are
very important for areas such as social networks, semantic
web, and bioinformatics.

  In this paper, we present Arabesque, the first distributed
data processing platform for implementing graph mining
algorithms. Arabesque automates the process of exploring
a very large number of subgraphs. It defines a high-level
filter-process computational model that simplifies the development
of scalable graph mining algorithms: Arabesque explores
subgraphs and passes them to the application, which
must simply compute outputs and decide whether the subgraph
should be further extended. We use Arabesque’s API
to produce distributed solutions to three fundamental graph
mining problems: frequent subgraph mining, counting motifs,
and finding cliques. Our implementations require a
handful of lines of code, scale to trillions of subgraphs, and
represent in some cases the first available distributed solutions.

#### 翻译
  Distributed data processing platforms such as MapReduce
and Pregel have substantially simpliﬁed the design and deployment of certain classes of distributed graph analyticsal gorithms.

>分布式数据处理平台例如mapreduce和pregel实质上是简化了某些类的分布式图形化分析算法的设计和调度

 However, these platforms do not represent a good
match for distributed graph mining problems, as for example
finding frequent subgraphs in a graph.

>但是，这些平台没有表现出对分布式图形挖掘问题的匹配。就以在图表中频繁的寻找子图作为例子

Given an input graph, these problems require exploring a very large number of subgraphs and finding patterns that match some “interestingness” criteria desired by the user.

>给出一个输入图表，这些问题需要扫描（探索）一个数量很多的子图并且寻找和用户期望的一些"兴趣性"准则相匹配的模式（图案,样品）。

These algorithms are very important for areas such as social networks, semantic web, and bioinformatics.


>这些算法对例如社交网络，语义网，和分析复杂生物的学科的领域非常重要。


In this paper, we present Arabesque, the first distributed
data processing platform for implementing graph mining
algorithms.
Arabesque automates the process of exploring a very large number of subgraphs.

>在这篇文献当中，我们介绍Arabesque,第一个实现图挖掘算法的分布式数据处理平台。Arabesque自动化了探索一个很大数量的子图的流程。

It defines a high-level filter-process computational model that simplifies the development of scalable graph mining algorithms: Arabesque explores subgraphs and passes them to the application, which must simply compute outputs and decide whether the subgraph should be further extended.

>他定义了一个高级的过滤过程的计算模型，它简化了可升级的图挖掘算法的开发:Arabesque 探索子图并且将他们传递给应用程序，这个应用程序必须简单的计算输出和决定是否子图应该被进一步的被扩展。

We use Arabesque’s API to produce distributed solutions to three fundamental graph mining problems: frequent subgraph mining, counting motifs,and finding cliques.

>我们用Arabesque的API去产生三个基础的图挖掘问题的分布式解决方案:频繁的子图挖掘，计数的图案，寻找派系。

Our implementations require a handful of lines of code, scale to trillions of subgraphs, and represent in some cases the first available distributed solutions.

>我们的实现需要很少行的代码，规模数万亿的子图，和在某些情况下第一个可获得的分布式解决方案的示范


#### 段落翻译
#### 摘要
分布式数据处理平台例如mapreduce和pregel实质上是简化了某些类的分布式图形化分析算法的设计和调度.但是，这些平台没有表现出对分布式图形挖掘问题的匹配。就以在图表中频繁的寻找子图作为例子.给出一个输入图表，这些问题需要扫描（探索）一个数量很多的子图并且寻找和用户期望的一些"兴趣性"准则相匹配的模式（图案,样品）。这些算法对例如社交网络，语义网，和分析复杂生物的学科的领域非常重要。

在这篇文献当中，我们介绍Arabesque,第一个实现图挖掘算法的分布式数据处理平台。Arabesque自动化了探索一个很大数量的子图的流程。他定义了一个高级的过滤过程的计算模型，它简化了可升级的图挖掘算法的开发:Arabesque 探索子图并且将他们传递给应用程序，这个应用程序必须简单的计算输出和决定是否子图应该被进一步的被扩展.我们用Arabesque的API去产生三个基础的图挖掘问题的分布式解决方案:频繁的子图挖掘，计数的图案，寻找派系。我们的实现需要很少行的代码，规模数万亿的子图，和在某些情况下第一个可获得的分布式解决方案的示范



### <span id="Introduction">Introduction</span>

### 原文

Graph data is ubiquitous in many fields, from the Web to advertising
and biology, and the analysis of graphs is becoming
increasingly important. The development of algorithms
for graph analytics has spawned a large amount of research,
especially in recent years. However, graph analytics has traditionally
been a challenging problem tackled by expert researchers,
who can either design new specialized algorithms
for the problem at hand, or pick an appropriate and sound
solution from a very vast literature. When the input graph or
the intermediate state or computation complexity becomes
very large, scalability is an additional challenge.

The development of graph processing systems such as
Pregel [25] has changed this scenario and made it simpler to
design scalable graph analytics algorithms. Pregel offers a
simple “think like a vertex” (TLV) programming paradigm,
where each vertex of the input graph is a processing element
holding local state and communicating with its neighbors
in the graph. TLV is a perfect match for problems that
can be represented through linear algebra, where the graph
is modeled as an adjacency matrix (or some other variant
like the Laplacian matrix) and the current state of each vertex
is represented as a vector. We call this class of methods
graph computation problems. A good example is computing
PageRank [6], which is based on iterative sparse matrix
and vector multiplication operations. TLV covers several
other algorithms that require a similar computational architecture,
for example, shortest path algorithms, and over the
years many optimizations of this paradigm have been proposed
[17, 26, 36, 42].

Despite this progress, there remains an important class
of algorithms that cannot be readily formulated using the
TLV paradigm. These are graph mining algorithms used
to discover relevant patterns that comprise both structurebased
and label-based properties of the graph. Graph mining
is widely used for several applications, for example, discovering
3D motifs in protein structures or chemical compounds,
extracting network motifs or significant subgraphs
from protein-protein or gene interaction networks, mining
attributed patterns over semantic data (e.g., in Resource
Description Framework or RDF format), finding structurecontent
relationships in social media data, dense subgraph mining for community and link spam detection in web data,among others. Graph mining algorithms typically take a labeled and immutable graph as input, and mine patterns
that have some algorithm-specific property (e.g., frequency
above some threshold) by finding all instances of these patterns
in the input graph. Some algorithms also compute aggregated
metrics based on these subgraphs


![图1](./img/1.PNG)

Figure 1: Exponential growth of the intermediate state in
graph mining problems (motifs counting, clique finding,
FSM: Frequent subgraph mining) on different datasets.


Designing graph mining algorithms is a challenging and
active area of research. In particular, scaling graph mining
algorithms to even moderately large graphs is hard. The set
of possible patterns and their subgraphs in a graph can be
exponential in the size of the original graph, resulting in an
explosion of the computation and intermediate state. Figure
1 shows the exponential growth of the number of “interesting”
subgraphs of different sizes in some of the graph
mining problems and datasets we will evaluate in this paper.
Even graphs with few thousands of edges can quickly generate
hundreds of millions of interesting subgraphs. The need
for enumerating a large number of subgraphs characterizes
graph mining problems and distinguishes them from graph
computation problems. Despite this state explosion problem,
most graph mining algorithms are centralized because of the
complexity of distributed solutions.


In this paper, we propose automatic subgraph exploration
as a generic building block for solving graph mining
problems, and introduce Arabesque, the first embedding exploration
system specifically designed for distributed graph
mining. Conceptually, we move from TLV to “think like an
embedding” (TLE), where by embedding we denote a subgraph
representing a particular instance of a more general
template subgraph called a pattern (see Figure 2).


Arabesque defines a high-level filter-process computational
model. Given an input graph, the system takes care
of automatically and systematically visiting all the embeddings
that need to be explored by the user-defined algorithm,
performing this exploration in a distributed manner. The system
passes all the embeddings it explores to the application,
which consists primarily of two functions: filter, which indicates whether an embedding should be processed, and process,
which examines an embedding and may produce some
output. For example, in the case of finding cliques the filter
function prunes embeddings that are not cliques, since none
of their extensions can be cliques, and the process function
outputs all explored embeddings, which are cliques by construction.
Arabesque also supports the pruning of the exploration
space based on user-defined metrics aggregated across
multiple embeddings.

![图2](./img/2.PNG)


Figure 2: Graph mining concepts: an input graph, an example
pattern, and the embeddings of the pattern. Colors represent
labels. Numbers denote vertex ids. Patterns and embeddings
are two types of subgraphs. However, a pattern is
a template, whereas an embedding is an instance. In this example,
the two embeddings are automorphic.

The Arabesque API simplifies and thus democratizes the
design of graph mining algorithms, and automates their execution
in a distributed setting. We used Arabesque to implement
and evaluate scalable solutions to three fundamental
and diverse graph mining problems: frequent subgraph mining,
counting motifs, and finding cliques. These problems
are defined precisely in Section 2. Some of these algorithms
are the first distributed solutions available in the literature,
which shows the simplicity and generality of Arabesque.

Arabesque’s embedding-centered API facilitates a highly
scalable implementation. The system scales by spreading
embeddings uniformly across workers, thus avoiding
hotspots. By making it explicit that embeddings are the fundamental
unit of exploration, Arabesque is able to use fast
coordination-free techniques, based on the notion of embedding
canonicality, to avoid redundant work and minimize
communication costs. It also enables us to store embeddings
efficiently using a new data structure called Overapproximating
Directed Acyclic Graph (ODAG), and to devise a
new two-level optimization for pattern-based aggregation,
which is a common operation in graph mining algorithms.


Arabesque is implemented as a layer on top of Apache
Giraph [3], a Pregel-inspired graph computation system,
thus allowing both graph computation and graph mining
algorithms to run on top of the same infrastructure. The
implementation does not use a TLV approach: it considers
Giraph just as a regular data parallel system implementing
the Bulk Synchronous Processing model.
To summarize, we make the following contributions:

• We propose embedding exploration, or “think like an embedding”,
as an effective basic building block for graph
mining. We introduce the filter-process computational
model (Section 3), design an API that enables embedding
exploration to be expressed effectively and succinctly,
and present three example graph mining applications
that can be elegantly expressed using the Arabesque
API (Section 4).

• We introduce techniques to make distributed embedding
exploration scalable: coordination-free work sharing, ef-
ficient storage of embeddings, and an important optimization
for pattern-based aggregation (Section 5).

• We demonstrate the scalability of Arabesque on various
graphs. We show that Arabesque scales to hundreds of
cores over a cluster, obtaining orders of magnitude reduction
of running time over the centralized baselines (Section
6), and can analyze trillions of embeddings on large
graphs.

The Arabesque system, together with all applications
used for this paper, is publicly available at the project’s website:
www.arabesque.io.

#### 翻译

Graph data is ubiquitous in many fields, from the Web to advertising and biology, and the analysis of graphs is becoming increasingly important.

>图形数据在许多领域普遍存在，从网站到广告业和生物学，并且分析图形正在变得越来越重要。

The development of algorithms for graph analytics has spawned a large amount of research, especially in recent years.

>图形分析算法的发展催生了大量的研究，尤其是在近些年来。

However, graph analytics has traditionally been a challenging problem tackled by expert researchers, who can either design new specialized algorithms for the problem at hand, or pick an appropriate and sound solution from a very vast literature.

>但是，图形分析历年来是具有挑战性的，由那些能够为了手上的问题设计新的专门的算法或者从非常庞大的文献中选择一个适当并且健全的解决方案的专家去解决。

When the input graph or the intermediate state or computation complexity becomes very large, scalability is an additional challenge.

>当输入图形或者中间状态或者计算复杂度非常大的时候，可测量性是一额外的挑战。

The development of graph processing systems such as Pregel [25] has changed this scenario and made it simpler to design scalable graph analytics algorithms.

>图形处理系统的发展例如pregel改变了这种方案，并且使设计可升级的图形分析算法更加的简单。

Pregel offers a simple “think like a vertex” (TLV) programming paradigm, where each vertex of the input graph is a processing element holding local state and communicating with its neighbors in the graph.

>Pregel 提供了一个简单的"像顶点一样思考"的编程范例，每一个输入图的顶点是一个保持局部状态的处理单元并且在图形中和它的邻点进行通讯。

TLV is a perfect match for problems that can be represented through linear algebra, where the graph is modeled as an adjacency matrix (or some other variant like the Laplacian matrix) and the current state of each vertex is represented as a vector.

>T L V 对那些通过线性代数表示的问题能够完美的匹配，在那些图形建模为邻接矩阵（或者一些其他变形像路普拉斯矩阵）和每一个顶点的当前状态被表示为一个向量的问题中。

We call this class of methods graph computation problems.

>我们称这一类的方法叫做图计算问题

A good example is computing PageRank [6], which is based on iterative sparse matrix and vector multiplication operations.

>一个好的例子就是计算PageRank, 它是基于迭代稀疏矩阵和向量乘法运算。

TLV covers several other algorithms that require a similar computational architecture, for example, shortest path algorithms, and over the years many optimizations of this paradigm have been proposed [17, 26, 36, 42].

>TLV 涉及了一些其他的算法，它需要相似的计算结构，例如，最短路径算法，多年来，这种模式的许多优化已被提出来。

Despite this progress, there remains an important class of algorithms that cannot be readily formulated using the TLV paradigm.

>尽管这些进展，这里依然有一类重要的算法不可以使用TLV范例制定。

These are graph mining algorithms used to discover relevant patterns that comprise both structurebased and label-based properties of the graph.

>这些就是用于发现相关模式的基于结构和基于表的图的性质的图挖掘算法。


 Graph mining is widely used for several applications, for example, discovering 3D motifs in protein structures or chemical compounds, extracting network motifs or significant subgraphs from protein-protein or gene interaction networks, mining attributed patterns over semantic data (e.g., in Resource Description Framework or RDF format), finding structure content relationships in social media data, dense subgraph mining for community and link spam detection in web data,among others.

>图挖掘广泛的用于一些应用，例如发现蛋白质结构中或者化学物质中的3D图案，从蛋白质或者基因交互网络中提取网络图案或者重要的子图，(例如在资源描述框架或者R D F 格式中)，正在使用发音在社会媒体数据，密集的子图挖掘社区和链接的垃圾邮件检测在Web数据中发现结构内容的关系，等等。

 Graph mining algorithms typically take a labeled and immutable graph as input, and mine patterns that have some algorithm-specific property (e.g., frequency above some threshold) by finding all instances of these patterns
in the input graph.


>图形挖掘算法通常采用一个标记和不可变的图形作为输入,和具有一些算法特性的挖掘模式（例如频率高于某个阈值），通过在输入图中的样式的所有实例。

Some algorithms also compute aggregated metrics based on these subgraphs。
一些算法也计算基于这些子图的综合指标。


Figure 1: Exponential growth of the intermediate state in
graph mining problems (motifs counting, clique finding,
FSM: Frequent subgraph mining) on different datasets.

>图一：在不同的数据集中图挖掘问题中的中间状态的指数增长（图案计数，派系的发现，频繁子图挖掘）

Designing graph mining algorithms is a challenging and
active area of research.

>设计图挖掘算法在研究中是一个具有挑战性和活跃的领域


In particular, scaling graph mining
algorithms to even moderately large graphs is hard.

>尤其是，将图挖掘算法应用于中等大小的图是困难的

The set of possible patterns and their subgraphs in a graph can be
exponential in the size of the original graph, resulting in an
explosion of the computation and intermediate state.

>图表中的可能的模式集和他们的子图有可能是原始图大小的指数倍，导致了爆炸性的计算和中间状态。

Figure 1 shows the exponential growth of the number of “interesting”
subgraphs of different sizes in some of the graph
mining problems and datasets we will evaluate in this paper.

>图一展示在一些图挖掘问题中不同大小的“intersting”子图的数量的爆炸性增长并且我们将评估文本的数据集。

Even graphs with few thousands of edges can quickly generate
hundreds of millions of interesting subgraphs.

>即使是数千个边的图也能生成数亿的"intersting"子图。

The need for enumerating a large number of subgraphs characterizes
graph mining problems and distinguishes them from graph
computation problems.

>图挖掘问题以需要枚举很大数量的子图为特征并且将其于图计算问题区别开来。

 Despite this state explosion problem,
most graph mining algorithms are centralized because of the
complexity of distributed solutions.

>尽管这个状态是爆炸性的问题，但是大多数图形挖掘算法是集中式的，因为
分布式解决方案的复杂性。


In this paper, we propose automatic subgraph exploration
as a generic building block for solving graph mining
problems, and introduce Arabesque, the first embedding exploration
system specifically designed for distributed graph
mining.

>在这篇文章中，我们把自动子图搜索看作是一个解决图挖掘问题的通用构建，并且介绍阿拉伯图案
，它是第一个为分布式图挖掘设计的嵌入式的探索系统

Conceptually, we move from TLV to “think like an
embedding” (TLE), where by embedding we denote a subgraph
representing a particular instance of a more general
template subgraph called a pattern (see Figure 2).

>从概念上讲，我们从TLV移动到了“像嵌入一样思考”（TLE）,通过嵌入，我们表示一个子图
通过表示一个称之为模式的更一般的模板的特别的实例（看图二）

Arabesque defines a high-level filter-process computational
model.

>阿拉伯图案定义了一个高层次的过滤过程计算模型。

Given an input graph, the system takes care
of automatically and systematically visiting all the embeddings
that need to be explored by the user-defined algorithm,
performing this exploration in a distributed manner.

>给出一个输入图，系统会自动的，系统性的关注访问所有的那些需要通过通过分布式的方式进行的自定义算法探索的嵌入部分。

The system passes all the embeddings it explores to the application,
which consists primarily of two functions: filter, which indicates whether an embedding should be processed, and process,which examines an embedding and may produce some output.

>系统通过所有的嵌入部分并暴露给应用，应用主要由两个函数组成：过滤器，指示是否嵌入部分应该被处理。处理，审查一个嵌入部分和有可能处理一些输出。

For example, in the case of finding cliques the filter function prunes embeddings that are not cliques, since none of their extensions can be cliques, and the process function outputs all explored embeddings, which are cliques by construction.

>例如，在发现子图派系的案例中过滤器的功能用于修剪不是派系的嵌入部分，因为他们的拓展没有可能是派系，并且处理函数输出所有探索的嵌入部分，那些嵌入的部分通过建设而形成派系。

Arabesque also supports the pruning of the exploration
space based on user-defined metrics aggregated across multiple embeddings.

>Arabesque还支持修剪基于用户定义的度量标准聚合的空间多次嵌入的探测。

Figure 2: Graph mining concepts: an input graph, an example
pattern, and the embeddings of the pattern. Colors represent
labels. Numbers denote vertex ids. Patterns and embeddings
are two types of subgraphs. However, a pattern is
a template, whereas an embedding is an instance. In this example,
the two embeddings are automorphic.

>图二：

The Arabesque API simplifies and thus democratizes the
design of graph mining algorithms, and automates their execution
in a distributed setting. We used Arabesque to implement
and evaluate scalable solutions to three fundamental
and diverse graph mining problems: frequent subgraph mining,
counting motifs, and finding cliques. These problems
are defined precisely in Section 2. Some of these algorithms
are the first distributed solutions available in the literature,
which shows the simplicity and generality of Arabesque.

Arabesque’s embedding-centered API facilitates a highly
scalable implementation. The system scales by spreading
embeddings uniformly across workers, thus avoiding
hotspots. By making it explicit that embeddings are the fundamental
unit of exploration, Arabesque is able to use fast
coordination-free techniques, based on the notion of embedding
canonicality, to avoid redundant work and minimize
communication costs. It also enables us to store embeddings
efficiently using a new data structure called Overapproximating
Directed Acyclic Graph (ODAG), and to devise a
new two-level optimization for pattern-based aggregation,
which is a common operation in graph mining algorithms.


Arabesque is implemented as a layer on top of Apache
Giraph [3], a Pregel-inspired graph computation system,
thus allowing both graph computation and graph mining
algorithms to run on top of the same infrastructure. The
implementation does not use a TLV approach: it considers
Giraph just as a regular data parallel system implementing
the Bulk Synchronous Processing model.
To summarize, we make the following contributions:

• We propose embedding exploration, or “think like an embedding”,
as an effective basic building block for graph
mining. We introduce the filter-process computational
model (Section 3), design an API that enables embedding
exploration to be expressed effectively and succinctly,
and present three example graph mining applications
that can be elegantly expressed using the Arabesque
API (Section 4).

• We introduce techniques to make distributed embedding
exploration scalable: coordination-free work sharing, ef-
ficient storage of embeddings, and an important optimization
for pattern-based aggregation (Section 5).

• We demonstrate the scalability of Arabesque on various
graphs. We show that Arabesque scales to hundreds of
cores over a cluster, obtaining orders of magnitude reduction
of running time over the centralized baselines (Section
6), and can analyze trillions of embeddings on large
graphs.

The Arabesque system, together with all applications
used for this paper, is publicly available at the project’s website:
www.arabesque.io.


#### 段落翻译

图形数据在许多领域普遍存在，从网站到广告业和生物学，并且分析图形正在变得越来越重要。
图形分析算法的发展催生了大量的研究，尤其是在近些年来。但是，图形分析历年来是具有挑战性的，由那些能够为了手上的问题设计新的专门的算法或者从非常庞大的文献中选择一个适当并且健全的解决方案的专家去解决。当输入图形或者中间状态或者计算复杂度非常大的时候，可测量性是一额外的挑战。


图形处理系统的发展例如pregel 改变了这种方案，并且使设计可升级的图形分析算法更加的简单。
Pregel 提供了一个简单的"像顶点一样思考"的编程范例，每一个输入图的顶点是一个保持局部状态的处理单元并且在图形中和它的邻点进行通讯。T L V 对那些通过线性代数表示的问题能够完美的匹配，在那些图形建模为邻接矩阵（或者一些其他变形像路普拉斯矩阵）和每一个顶点的当前状态被表示为一个向量的问题中。我们称这一类的方法叫做图计算问题一个好的例子就是计算PageRank, 它是基于迭代稀疏矩阵和向量乘法运算。TLV 涉及了一些其他的算法，它需要相似的计算结构，例如，最短路径算法，多年来，这种模式的许多优化已被提出来。

尽管这些进展，这里依然有一类重要的算法不可以使用TLV范例制定。这些就是用于发现相关模式的基于结构和基于表的图的性质的图挖掘算法.图挖掘广泛的用于一些应用，例如发现蛋白质结构中或者化学物质中的3D图案，从蛋白质或者基因交互网络中提取网络图案或者重要的子图，(例如在资源描述框架或者R D F 格式中)，正在使用发音在社会媒体数据，密集的子图挖掘社区和链接的垃圾邮件检测在Web数据中发现结构内容的关系，等等。图形挖掘算法通常采用一个标记和不可变的图形作为输入,和具有一些算法特性的挖掘模式（例如频率高于某个阈值），通过在输入图中的样式的所有实例。一些算法也计算基于这些子图的综合指标。

![图1](./img/1.PNG)

图一：在不同的数据集中图挖掘问题中的中间状态的指数增长（图案计数，派系的发现，频繁子图挖掘）

设计图挖掘算法在研究中是一个具有挑战性和活跃的领域。尤其是，将图挖掘算法应用于中等大小的图是很困难的。图表中的可能的模式集和他们的子图有可能是原始图大小的指数倍，导致了爆炸性的计算和中间状态。图一展示在一些图挖掘问题中不同大小的“intersting”子图的数量的爆炸性增长并且我们将评估文本的数据集。即使是数千个边的图也能生成数亿的"intersting"子图。图挖掘问题以需要枚举很大数量的子图为特征并且将其于图计算问题区别开来。尽管这个状态是爆炸性的问题，但是大多数图形挖掘算法是集中式的，因为分布式解决方案的复杂性。

在这篇文章中，我们把自动子图搜索看作是一个解决图挖掘问题的通用构建，并且介绍阿拉伯图案
，它是第一个为分布式图挖掘设计的嵌入式的探索系统。从概念上讲，我们从TLV移动到了“像嵌入一样思考”（TLE）,通过嵌入，我们表示一个子图通过表示一个称之为模式的更一般的模板的特别的实例（看图二）。阿拉伯图案定义了一个高层次的过滤过程计算模型。给出一个输入图，系统会自动的，系统性的关注访问所有的那些需要通过通过分布式的方式进行的自定义算法探索的嵌入部分。系统通过所有的嵌入部分并暴露给应用，应用主要由两个函数组成：过滤器，指示是否嵌入部分应该被处理。处理，审查一个嵌入部分和有可能处理一些输出。例如，在发现子图派系的案例中过滤器的功能用于修剪不是派系的嵌入部分，因为他们的拓展没有可能是派系，并且处理函数输出所有探索的嵌入部分，那些嵌入的部分通过建设而形成派系。

![图2](./img/2.PNG)

















[1]: https://jchanji.github.io "主页"
