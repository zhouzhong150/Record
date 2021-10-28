[toc]

# **GRL**

## **chapter 4**

### Node embedding

- #### Shallow embedding

- #### Inner-product



## **chapter 5 The Graph Neural Network Model**

*Permutation invariance and equivariance*

## 5.1 Neural Message Passing

### 5.1.1 Overview of the Message Passing Framwork

![image-20210826124057384](/Users/fakerbaby/Library/Application Support/typora-user-images/image-20210826124057384.png)

*Figure 5.1 Notice that the computation graph of the GNN froms a tree structure by unfolding the neighborhood around the target node.*







### 5.1.2 Motivations and Intuitions

The basic intuition behind the GNN message-passing framework is straightforward:

at each iteration, every node aggregates information from its local neighborhood, and as these iterations progress each node embedding contains more and more information from further reaches of the graph.

**But what kind of "information" do these node embeddings actually encode?**

- Structurl information about the graph:  degrees of the graph
- Feature-based 



### 5.1.3 The Basic GNN

we have discussed the GNN framework in a relatively abstract fashion as a series of message-passing iterations using UPDATE and AGGREGATE functions

$$
h_u^{(k)} = \sigma(W_{self}^{k}h_{u}^{(k-1)} + W_{neigh}^{(k)}\sum_{v\in{\cal N}(u)}h_v^{(k-1)} + b^{(k)}) \tag{5.7}
$$


### 5.1.4 Message Passing with self-loops





## 5.2 Generalized Neighborhood Aggregation

### 5.2.1 Neighborhood Normalization



##### Graph convolutional networks (GCNs)

One of the most popular baseline graph neural network models the ***graph convolutional network*** (GCN) employs the symmetric-normalized aggregation as well as the self-loop update approach. The GCN model thus denes the message passing function as

$$
h_u^{(k)} = \sigma(W^{(k)}\sum_{v\in{\cal N}(u)\cup{\set u}}\frac{h_v}{\sqrt{|{\cal N}(u)||{\cal N}(v)|}} ) \tag {5.16}
$$

##### *To normalize or not to normalize?*

Proper normalization can be essential to achieve stable and strong performance when using a GNN. It is important to note, however, that normalization can also lead to a loss of information.



### 5.2.2  Set Aggregators

there is no natural ordering of a nodes' neighbors, and any aggregation function we dene must thus be permutation invariant.

*a universal set function approximator:*
$$
m_{{\cal N}(u)} = MLP_{\theta}(\sum_{v\in N(u)}MLP_{\phi}(h_v))
$$

##### Janossy pooling 

we apply a **permutation-sensitive function** and **average the result** over many possible permutations
$$
m_{\cal N(u)} = MLP_{\theta}(\frac {1}{|\Pi|}\sum_{\pi\in\Pi}\rho_\phi(h_{v_1},h_{v_2},...,h_{v_{|\cal N|(u)}})_{\pi_i})\tag {5.18}
$$
$ \Pi$ denotes a set of permutations  and $\rho_\phi$ Is a permutation-sensitive function,e.g., a neural network that operates on sequences.

summing over all possible

permutations is generally intractable. Thus, in practice, Janossy pooling employs one of two approaches:

1. Sample a random subset of possible permutations during each application of the aggregator, and only sum over that random subset.
2. Employ a canonical ordering of the nodes in the neighborhood set; e.g., order the nodes in descending order according to their degree, with ties broken randomly.





### 5.2.3 Neighborhood Attention

The basic idea is to assign an attention weight or importance to each neighbor, which is used to weigh this neighbor's influence during the aggregation step.
$$
m_{{\cal N}(u)} = \sum_{v\in {\cal N}(u)} {\alpha_{u,v}h_v} \tag {5.19}
$$
The orignal GAT paper, the attention weights are defined as 
$$
\alpha_{u,v}=\frac{exp(\rm{[a^{T}Wh_h \oplus Wh_v]})}{\sum_{v'\in\cal{N}(u)}{exp(\rm{a^{T}[Wh_u\oplus Wh_{v'}}])}}
$$


Adding attention is a useful strategy for increasing the representational capcity of a GNN model, especially in cases where you have prior knowledge to indicate that some neighbors might be more informative than others.





## 5.3 Generalized Update Methods

reading paper: *GraphSAGE* 

GNN message passing involves two key steps: **aggregation** and **updating**, and in many ways the UPDATE operator plays an equally important role in defying the power and inductive bias of GNN model.



##### Over-smoothing and neighborhood influence

Over-smoothing: Generalized update methods can help to address--is known as over-smoothing.The essential idea of over-smoothing is that after several iterations of GNN message passing, the represetations for all the nodes in the graph can become very similar to one another. This tendency is especially common in basic GNN models and models that employ the self-loop update approach.



Over-smoothing is problematic because it makes it impossible to build deeper GNN models-- which leverage longer-term dependencies in the graph--since these deep GNN models tend to just generate over-smoothed embeddings.



The issue can be formalized by defining the influence of each node's input feature $ h_u^{(0)} = x_u$ On the final layer embedding of all the other nodes in the graph, i.e, $h_v^{(K)} $, $\forall v\in \cal V$  





### 5.3.1 Concatenation and Skip-Connections

One of the simplest skip connection updates employs a concatenation to preserve more node-level information during message passing:
$$
\rm{UPDATE_{concat}}(h_u,m_{\cal N}(u))=[UPDATE_{base}(h_u,m_{\cal N(u)})\oplus h_u] \tag {5.28}
$$
The concatenation-based skip connection was proposed in the GraphSAGE framework, which was one of the first works to highlight the possible benefits of these kinds of modifications to the update function.

However, in addition to concatenation, we can also employ other forms of skip-connections, such as the linear interpolation method proposed by Pham et al.
$$
\rm{UPDATEinterpolate(hu,mN(u))} = \alpha_1 \circ UPDATE_{base}(h_u,m_{\cal{N}(u)}) +\alpha_2\odot h_u, (5.29)
$$
where $\alpha_1,\alpha_2\in[0,1]^{d}$ are gating vector with $\alpha_2 = 1-\alpha_2$ and $\circ$ denotes elementwise multiplication.



In general, these concatenation and residual connections are simple strategies that can help to alleviate the over-smoothing issue in GNNs, while also improving the numerical stability of optimization.

Indeed, similar to the utility of residual connections in convolutional neural networks (CNNs) [He et al., 2016], applying these approaches to GNNs can facilitate the training of much deeper models. In practice these techniques tend to be most useful for node classification tasks with moderately deep GNNs (*e.g.*, 2-5 layers), and they excel on tasks that exhibit homophily, *i.e.*, where the prediction for each node is strongly related to the features of its local neighborhood.



### 5.3.2 Gated Updates

In this view, we can directly apply methods used to update the hidden state of RNN architectures based on observations. For instance, one of the earliest GNN architectures [Li et al., 2015] defines the
update function as
$$
h_u^{k} = GRU(h_u^{(k-1)},m_{{\cal N}(u)}^{(k)}),\tag{5.30}
$$

### 5.3.3 Jumping Knowledge Connections

In the preceding sections, we have been implicitly assuming that we are using the output of the final layer of the GNN. In other words, we have been assuming that the node representations $z_u$ that we use in a downstream task are equal to final layer node embeddings in the GNN:
$$
z_u=h_u^{(K)},\forall u\in{\cal V}\tag{5.31}
$$
This assumption is made by many GNN approaches, and the limitations of this strategy motivated much of the need for residual and gated updates to limit over-smoothing.



However, a complimentary strategy to improve the quality of the final node representations is to simply leverage the representations at each layer of message passing, rather than only using the final layer output. In this approach we define the final node representation $z_u$ as
$$
z_u = f_{JK}(h_u^{(0)}\oplus h_u^{(1)}\oplus...\oplus h_u^{(K)}),\tag{5.32}
$$


## 5.4 Edge Features and Multi-relational GNNs

there are many applications where the graphs in question are multi-relational or otherwise heterogenous (e.g., knowledge graphs). In this section, we review some of the most popular strategies
that have been developed to accommodate such data.

### 5.4.1 Relational Graph Neural Networks

The first approach proposed to address this problem is commonly known as the *Relational Graph Convolutional Network (RGCN)* approach. In this approach we augment the aggregation function to accommodate multiple relation types by specifying a separate transformation matrix per relation type:
$$
m_{\cal N(u)} = \sum_{\tau \in {\cal R}} \sum_{v\in {\cal N(u)}} \frac{W_\tau h_v}{f_n({\cal N}(u),{\cal N}(v))}\tag {5.33}
$$
where $f_n$ is a normalization function that can depend on both the neighborhood of the node $u$ as well as neighbor $v$ being aggregated over.

Overall, the multi-relational aggregation in RGCN is thus analogous to the basic a GNN approach with normalization, but we separately aggregate information across different edge types.



##### Parameter sharing

One drawback of the naive RGCN approach is the drastic increase in the number of parameters, as now we have one trainable matrix per relation type.

*Schlichtkrull et al.*[2017] propose a scheme to combat this issue by parameter sharing with basis matrices, where
$$
W_{\tau} = \sum_{i=1}^b\alpha_{i,\tau}\rm B_i.\tag{5.34}
$$

##### Extensions and variations

The RGCN architecture can be extended in many ways, and in general, we refer to approaches that define seperate aggregation matrices pre relation as relation graph neural networks.



### 5.4.2 Attention and Feature Concatenation

The relational GNN approach, where we define a separate aggregation parameter per relation, is applicable for multi-relational graphs and cases where we have discrete edge features. To accommodate cases where we have more general forms of edge features, we can leverage these features in attention or by concatenating this information with the neighbor embeddings during message passing.



## 5.5 Graph Pooling

The neural message passing approach produces a set of node embeddings, but what if we want to make predictions at the graph level? In other words, we have been assuming that the goal is to learn node representations.

but what if we to learn an embedding for the entire graph G? This task is often referred to as graph pooling, since our goal is to pool together the node embeddings in order to learn an embedding of the entire graph.

##### Set pooling approaches

there are two approaches that are most commonly applied for learning graph-level embeddings via set pooling. 

The first approach is simply to take a sum (or mean) of the node embeddings.
$$
z_{\cal G} = \frac{\sum_{v\in{\cal V}}\rm z_u}{f_n(|{\cal V}|)},\tag{5.37}
$$
where $f_n$ is some normalizing function.While quite simple, pooling based on the sum or mean of the node embeddings is often sufficient for applications involving small graphs.



The second popular set-based approach uses a combination of LSTMs and attention to pool the node embeddings
$$
\rm q_t = LSTM(o_{t-1},q_{t-1}),\tag{5.38}
$$

$$
e_{v,t} = f_a(\rm z_v,q_t),\forall v\in {\cal V},\tag{5.39}
$$

$$
a_{v,t} = \frac{exp(e_{v,i})}{\sum_{u\in{\cal V}}a_{v,t}z_v},\tag{5.40}
$$

$$
\rm o_t=\sum_{v\in{\cal V}}a_{v,t}\rm z_v,\tag{5.41}
$$

In the above equations, the **$\rm q_t$** vector represents a ***query vector*** for the attention at each iteration ***t***. In Equation **(5.39)**, the query vector is used to compute an attention score over each node using an attention function **$f_a : \R^d × \R^d → \R$** (e.g., a dot product), and this attention score is then normalized in Equation **(5.40)**. Finally, in Equation **(5.41)** a weighted sum of the node embeddings is computed based on the attention weights, and this weighted sum is used to update the query vector using an LSTM update **(Equation 5.38)**. Generally the q0 and o0 vectors are initialized with all-zero values, and after iterating Equations **(5.38)-(5.41)** for T iterations, an embedding for the full graph is
computed as
$$
z_{\cal G}=\rm o_1 \oplus o_2 \oplus ...\oplus o_T.\tag{5.42}
$$
This approach represents a sophisticated architecture for attention-based pooling over a set, and it has become a popular pooling method in many graph-level classification tasks.



#### Graph coarsening approaches

One popular strategy to accomplish this is to perform graph clustering or coarsening as a means to pool the node representations.

Regardless of the approach used to generate the cluster assignment matrix S, the key idea of graph coarsening approaches is that we then use this matrix to coarsen the graph. In particular, we use the assignment matrix S to compute a new coarsened adjacency matrix


$$
\rm A^{new} = S^TAS\in \R^{+c\times c},\tag{5.44}
$$
and a new set of node features
$$
\rm X^{new} = S^TX\in\R^{c\times d}.\tag{5.45}
$$


## 5.6 ==Generalized Message Passing==

$$
\rm h_{(u,v)}^{(k)} = \rm UPDATE_{edge}(h_{(u,v)}^{(k-1)},h_u^{(k-1)},h_v^{(k-1},h_{\cal G}^{(k-1)})\tag{5.46}
$$

$$
\rm m_{\cal N(u)} = AGGREGATE_{node}({h_{(u,v)}^{(k)} \forall v\in{\cal N}(u)})\tag{5.47}
$$

$$
\rm h_u^{(k)} = UPDATE_{node}(h_u^{k-1},m_{\cal N(u)},h_{\cal G}^{(k-1)})\tag{5.48}
$$

$$
\rm h_{\cal G}^{(k)}=UPDATE_{graph}(h_{\cal G}^{(k-1)},\{h_u^{(k)},\forall u \in {\cal V} \},\{h_{(u,v)}^{(k)} \forall(u,v) \in \varepsilon \}).\tag{5.49}
$$

This allows the message passing model to easily integrate edge and graph-level features, and recent work has also shown this generalized message passing approach to have benefits compared to a standard GNN in terms of logical expressiveness [Barcel´o et al., 2020]. Generating embeddings for edges and the entire graph during message passing also makes it trivial to define loss functions based on
graph or edge-level classification tasks.

In terms of the message-passing operations in this generalized message passing framework, we first update the edge embeddings based on the embeddings of their incident nodes (Equation 5.46). Next, we update the node embeddings by aggregating the edge embeddings for all their incident edges (Equations 5.47 and 5.48). The graph embedding is used in the update equation for both node and edge representations, and the graph-level embedding itself is updated by aggregating over all the node and edge embeddings at the end of each iteration (Equation 5.49). All of the individual update and aggregation operations in such a generalized message-passing framework can be implemented using the techniques discussed in this chapter (e.g., using a pooling method to compute the graph-level update).









# Chapter 7 Theoretical Motivations



## 7.1 GNNs and Graph Convolutions

Euclidean convolutions to the non-Euclidean graph domain.
