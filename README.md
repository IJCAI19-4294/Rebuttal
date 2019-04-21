
# Rebuttal to Review #213830:
Thank you for your review.
1. To clearly express the differences between attention mechanism and rejection mechanism, we take the chain graph (i.e., node0->node1->node2->node3) as an example (the same with the example showed in Figure 2 in our paper).
For the attention mechanism, because node1 contains only one neighbor (i.e., node0), no matter what the features of node0 and node1 are, the attention weight of node0 to node1 is always 1. This means when the messages with attentions propagation layer by layer, the influence of node0 to node3 is always weighted by 1 (i.e., 1*1*1=1), and the distant node0 cannot be punished. Attention mechanism is a node-wise operation, and the message passing of distant node (e.g., node0) is mainly depended on the adjacency node (e.g., node1) rather than the embedding node (e.g., node3).  Thus, the feature of distant node can be more likely to pass to the embedding node. Actually, attention mechanism can be taken as a bias based random walk, which will also coverage to a stationary distribution and has over-smoothing problem [1].
While, compared with attention mechanism, the rejection mechanism is a layer-wise operation and the message of distant node (i.e., node0) must be punished. When the messages with rejection mechanism propagate layer by layer, the influence of node0 to node3 is always weighted by c^(1)c^(2)c^(3) (a small value), which can well solve the over-smoothing problem.  


2. It is true that, overall, Citeseer contains 3327 nodes. But, among 3327 nodes, there are 15 isolated nodes (seeing Line 61-62 in [2]), and these isolated nodes have no label information (seeing Line 67-69 in [2], where the label vector of isolated node in “ty_extended” is initialized as a zero vector). (代码)
Although GAT paper reports there has 3327 nodes in Citeseer. Actually, the nodes used in GAT paper is 1620 (i.e., #train 120+#val 500+#test 1000). Furthermore, none of them (i.e., 1620 nodes) is isolated node (seeing Line 56-57 and Line 78-80 in [2], where “test_idx_range”, “range(len(y))” and “range(len(y), len(y)+500)” do not contain the ids of isolated nodes).
Thus, we also ignore the isolated nodes, and use the left 3312 nodes (which are reported in our paper) as train, val and test nodes. Because the ignored nodes are isolated, the number of edges reported in our paper is still 4,732 (the same with GAT paper).  

3.The main reason for the difference of accuracy of GAT on Cora between our paper and GAT paper is the different experimental setting. For example, the embedding dimension (16 dimension in our paper, and 64 dimension in GAT paper), and the way of data split strategy (3312 nodes used in our paper and 1620 nodes used in GAT paper) influence the accuracy. Actually, there are many works that adopt a different setting with GAT paper, and report different results with GAT paper (e.g., the paper [1]). We think a more important thing is keeping the experiments setting the same among all methods (including baselines and the proposed method) used in one paper rather than across papers.

4. As for the accuracy on Citeseer reported in our paper, we are sincere to say sorry for our mistakes. Specifically, as mentioned before, we try to report the results of the non-isolated nodes in Citeseer, and we modify the “load_data” function in [2]. But we mismatch the node feature and the corresponding node labels after deleting the isolated nodes. Thus, there has a huge gap in results. For other datasets, there have no isolated nodes (without deleting nodes and re-matching). Thus, the results are corrected and similar with the results reported in other papers.
Some evidences can be helpful to verify what we say. 
1)	The node number (i.e., 3312) of Citeseer reported in our paper is the number referred to the non-isolated nodes.
2)	Due to no isolated nodes in other datasets, there has no addition operation (e.g., deleting and re-matching) in these datasets. Thus, the results of other datasets are similar with the results published in other papers.
Again, we are sincere to say sorry for our mistakes. Here, we provide the results of Citeseer on Table 2 and Table 3 in our paper, which are obtained from the corrected code after rerunning.

------------------
Table 2  Citeseer
GraphSGAE 71.00 
GCN		  71.90  
GAT		  70.85 
JK		  71.53 
GraphDRej  72.58 
------------------
Table 3  Citeseer
GraphSGAE 71.00 
GCN		  71.90  
GAT		  70.92  
JK		  72.20 
GraphDRej  72.58 
--------------------


# Rebuttal to Review #252757:
First of all, thank you for your review.
1.	Thank you for your advice about the improvement of our paper, 1) The motivation of Section 3 “Limitation of Current GNNs” introduced in our paper is to emphasize the limitation of GNNs, and give a friendly understanding of this limitation and the motivation of our paper. There may exist a better way to put this section in a more suitable position. We will polish this content and give a deeper insight to GNNs. 2) The details about experimental setting can be found in the following answering about your four questions, and we will cite these four papers you mentioned in rebuttal.

Here are four answers about your four questions.
1.For the first question, the accuracy on Citeseer reported in our paper, we are sincere to say sorry for our mistakes. Specifically, for the datasets of Citeseer, there are 12 isolated nodes (seeing Line 57-66 in [1]), and the labels of all the isolated nodes are not provided (seeing Line 64-66 in [1], where the label vector of isolated node in “ty_extended” is initialized as a zero vector). In GCN paper or GAT paper, both of them only use 1620 nodes, and ignore the isolated nodes. Thus, we also try to report the results of the non-isolated nodes in Citeseer (total number is 3312=3327-15), and we modify the “load_data” function in [1]. But we mismatch the node feature and the corresponding node labels after deleting the isolated nodes. Thus, there has a huge gap in results.  For other datasets, there has no isolated nodes, and the results are corrected and similar with the results reported in other papers.	
Some evidences can be helpful to verify what we say. 
1)The node number (i.e., 3312) of Citeseer reported in our paper is the number referred to the non-isolated nodes.
2)Due to no isolated nodes in other datasets, there has no addition operation (e.g., deleting and re-matching) in these datasets. Thus, the results of other datasets are similar with the results published in other papers.
Again, we are sincere to say sorry for our mistakes. Here, we provide the results of Citeseer on Table 2 and Table 3 in our paper, which are obtained from the corrected code after rerunning.
------------------
Table 2  Citeseer
GraphSGAE 71.00 
GCN		  71.90  
GAT		  70.85 
JK		  71.53 
GraphDRej  72.58 
------------------
Table 3  Citeseer
GraphSGAE 71.00 
GCN		  71.90  
GAT		  70.92  
JK		  72.20 
GraphDRej  72.58 
--------------------

2.	For the second question, we randomly select 20% nodes from the rest non-isolated nodes (i.e., 3312 nodes) as training nodes. Due to the randomly sampling, the labeling rate per class is also the same (i.e., 20% per class).  We only use 20% nodes for training. We are sorry we cannot catch the meaning of the question “Do all nodes including unlabeled nodes used for training?”. Here are some explanations which may be helpful. For the unlabeled nodes, on the one hand, as we delete the isolated nodes (i.e., unlabeled nodes), there are no unlabeled nodes left in training set. On the other hand, due to randomly sampling, there may exist a case where there may be no training samples for one particular class, and it may be ok to ignore this problem as we report the mean results through several different random seeds.

3.	For the third question, it is true that the setting is different from other papers, such as split strategy and the embedding dimension. We also notice that the experimental setting may be different in different papers. For example, the paper [3] splits nodes in each graph into 60%, 20% and 20% for training,
validation and testing, and the paper [4] adopts several different training rates in experiments.
IMHO, if the experiment setting among all models (including the proposed model and baselines) used in one paper is keeping the same, we can fairly evaluate this work with other baselines. No matter the setting is same from other published paper or not. 

  
4.For the forth question, on the one hand, it is true that different variants of JK achieve best results in different datasets. But according to the results reported on Citeseer and Cora (similar datasets used in our paper), JK-Concat and JK-Maxpool outperform other baselines in accuracy, and JK-Maxpool has a low standard deviation than JK-Concat. Hence, JK-Maxpool is a more competitive baseline.
On the other hand, according to the Section 4.1 in [3], compared with other aggregation function, maxpool is more likely to overcome the over-smoothing problem. Thus, considering the property and the performance of maxpool, and the over-smoothing problem analyzed in our paper, we choose maxpool for JK.


5. For your opinion of over-smoothing about GCN, GAT and so on, it is nice to give the “phenomenon” view about over-smoothing. But we may have to admit that, this “problem” or “phenomenon” hurts the performance of GNN when too many layers added. If we can deal with it, a more powerful GNN with deep layers may be obtained.  
6. The motivation of Section 3 “Limitation of Current GNNs” introduced in our paper is to emphasize the limitation of GNNs, and give a friendly understanding of this limitation and the motivation of our paper. There may exist a better way to put this section in a more suitable position. We will polish this content and give a deeper insight to GNNs
7. Thank you for your advice on writing, we will polish it in the later version.

[1] https://github.com/tkipf/gcn/blob/master/gcn/utils.py  (the official code of GCN)
[2] https://github.com/tkipf/gcn/tree/master/gcn/data (the official code of GCN)
[3] K. Xu, C. Li, Y. Tian, T. Sonobe, K. Kawarabayashi, and S. Jegelka, Representation Learning on Graphs with Jumping Knowledge Networks, in ICML 2018.
[4] Q. Li, Z. Han, X.-M. Wu, Deeper Insights into Graph Convolutional Networks for Semi-Supervised Learning, in AAAI 2018.


# Rebuttal to Review #255415
First of all, thank you for your review.
1.	For the first question, first of all, we totally agree with you that replacing the single constant by a vector may be more adaptive. Here are some reasons that why we use a single constant:
1)Weighting a vector by a single constant may be a common idea used in deep learning. Taking the attention mechanism as an example, each vector is also weighted by a single constant.
2）A single constant, compared with a vector, is simpler. Considering that a single constant can also well solve the over-smoothing problem, there may be no need to extend to a vector. 
3) Introducing a vector rather a single constant may be more likely to have over-fitting problem. 
4)	A single constant has a better interpretability. In other words, the single constant can be taking as a layer-wise punishment strategy where each node is taken as the smallest unit. While, for vector-based methods, the smallest unit is the feature value in each dimension, which mixes the node feature passing in a GNN and losses the interpretability, i.e., the layer-wise punishment.
2.	For the second question, first of all, we are sorry to make you confused about our model explanation. The red arrows in Figure 1 refer to the short cut strategy or COMBIN used in GraphDRej or other GNNs, i.e., the Equation (2) or (10) where we need to feed COMBIN with the vectors directly from the previous layer.

3.	For the third question, we are sorry to give you some confusion about GraphDRej in mathematics, and thank you for advices to add more theoretical analysis about graph dilated convolution. Here we give a general mathematics view about it.  As we know, the original graph convolution operator is defined based on graph fourier transform [1]. The paper [2] extends the graph fourier transform to a Chebyshev polynomial which is a truncated expansion and well-approximated to graph fourier transform. From the mathematics perspective, our dilated convolution can be regarded as a modified Chebyshev polynomial introduced in [2], where dilated convolution separately aggregates multiple hops and Chebyshev polynomial iteratively aggregates multiple hops.

4.	Yes. For k-th layer, t-th hop, there has been a penalty parameter, i.e., c^(k,t). At each layer, there may have different kinds of kernels which represent different speeds of massage passing. Hence, different penalty parameters in each layer for different hops are needed.

[1] M. Henaff, J. Bruna, and Y. Lecun. Deep convolutional networks on graph-structured data. CoRR, abs/1506.05163, 2015.
[2] M. Defferrard, X. Bresson, and P. Vandergheynst. Convolutional neural networks on graphs with fast localized spectral filtering. In NIPS, 2016. 


# Rebuttal to Review #261709
Thank you for your review. We are pleasant you like our work which will motivate us to further improve our work to giving a higher quality.
We also thank you for your advice on some formulations. We will put more effort to polish our paper, and give a more clarity expression for readers.

