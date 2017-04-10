---
layout: page
title: Learning constraints!
tagline: Supporting tagline
---
# Learning-Based Performance Specialization of Configurable Systems 


A large scale configurable system typically offers thousands of options or parameters to let the engineers customize it for specific needs. Among the resulting many billions possible configurations, relating option and parameter values to desired performance is then a daunting task relying on a deep know how of the internals of the configurable system. We propose a staged configuration process to narrow the space of possible configurations to a good approximation of those satisfying the wanted high level customer requirements. Based on an oracle (e.g. a runtime test) that tells us whether a given configuration meets the non-functional requirements (e.g. speed or memory footprint), we leverage machine learning to retrofit the acquired knowledge into a variability model of the system that can be used to automatically specialize the configurable system. 

**SPLC'16**: In the repository https://github.com/learningconstraints/SPLC-16,  you will find all the required artefacts to replicate the experiments included in our paper submitted to the 20th International Systems and Software Product Line Conference. The goal was mostly to avoid *faulty* configurations and correct a video generator developed in the industry. The oracle was simply a yes/no procedure. 

In **RR'17**, we have considered performance qualities. The oracle is parametrized with an ***objective value*** that users of our specialization method can control. Hence our proposal can be seen as a generalization of the idea originally proposed in SPLC'16. Moreover our empirical results cover more systems and classification metrics while taking training sizes and objective values into account https://github.com/learningconstraints/ICSE-17.

**Objective of this page**: Our RR'17 results show that, for many different kinds of objectives and performance qualities, the approach has interesting accuracy, precision and recall after a learning stage based on a relative small number of random samples. This page describes how we gather and process input data, how we compute classification metrics. Furthermore, all plots, heatmaps, and figures can be visualized. We aim here to supplement our original material (it is impossible to include all relevant figures we have generated!) and also support an independent verification and replication of our study. 
We also aim to provide a practical reading grid of classification metrics for users of our method. 
	 
A research report is also freely available online https://hal.archives-ouvertes.fr/hal-01467299: 
`
+Paul Temple, Mathieu Acher, Jean-Marc A Jézéquel, Léo A Noel-Baron, and José A Galindo. "Learning-Based Performance Specialization of Configurable Systems" (2017), Research report, University of Rennes 1 (IRISA/Inria).
`
  		  
 ## Empirical Study		 ## Empirical Study
 		 


 
## Empirical Study

#### Subject systems
We performed our experiments on a publicly-available dataset used in previous works. The dataset includes sets of configurations of real-world configurable systems that have been executed and measured over different performance metrics (more details hereafter). Configurable systems come from various domains and are implemented in different languages (C, C++ and Java). The number of options varies from as little as ten, up to about sixty; options can take Boolean and numerical values. The two last columns show, first, the total number of different possible configurations existing for the system and, last, the number of configurations that have been generated and measured in previous works ("All" meaning all different configurations have been measured).
It should be noted that most benchmarks measure a single performance value, except for x264 where benchmarks evaluate 7 performance metrics.

#### Experimental setup 
For each subject system, we have randomly selected a certain number of configurations from the ones available in the public dataset: our ML method operates over a ***training sample*** to learn a separating function. All remaining configurations form the ***test sample***, i.e., it is the whole configuration set minus the training set. They are used to verify whether the classification of our ML is correct. For instance, the public dataset contains 69,000 different configurations for x264. If we used 500 of them to learn constraints, then we evaluate the classification quality of ML on the remaining 68,500 configurations.

For each configurable system, we make ***vary the sample size*** one by one, from 1 to the total number of configurations. We make vary the *objective value* between the lower and upper bounds of the performance measured in each dataset. We notice that threshold values have direct influence on the
***percentage of acceptable configurations***. For instance, for x264, an execution time (speed) threshold less than 100s leads to only 21.4% of acceptable configurations. We thus vary the objective value from 0% to 100% of acceptable configuration with steps of 5%.

### Classification measurements 

We consider that configurations are ***acceptable*** (resp. ***non-acceptable***) w.r.t a performance objective.

Our learning-based technique can introduce errors in the following cases:
* a configuration is valid (and thus classified as acceptable) despite being non-acceptable â€“ the system is under-constrained and unsafe;
* a configuration is not valid (and thus classified as non-acceptable) despite being acceptable (the system is over-constrained and lacks flexibility).

We expect to observe such cases:
* a configuration is valid and correctly classified as acceptable; 
* a configuration is non-valid and correctly classified as non-acceptable.

In our case, positive class (also called class-1) corresponds to non-acceptable configurations that we want to discard. The negative class (also called class-0) corresponds to acceptable
configurations that we want to keep. There are four possible situations represented in a confusion matrix (see Figure):
* True Positives (TP) and True Negatives (TN) are on the main diagonal and represent the set of correctly classified configurations;
* False Positive (FP) and False Negatives (FN), on the contrary, represent classification errors
  
For obtaining TP, TN, FP, and FN, we confront the classifications made by ML in the training set with the oracle decisions available in the testing set. Based on a confusion matrix, we can compute several classification metrics. 
  ![Figure]({{ site.url }}/assets/figure.png)


We rely on standard metrics used for binary classification problems. 
We used 5 metrics since they give a different and complementary perspective on the "quality" of our learning-based method. 

### Classification metrics

* **Accuracy** measures the portion of configurations accurately classified as acceptable and non-acceptable. It gives a global *classification* "performance" of the learning phase.  

However accuracy alone can have limited interest since it hinders important phenomena. 
In the example (see Figure),  you can have a high accuracy despite the clear limits of the learning phase of identifying non-acceptable configurations (precision is 29% and recall is 40%). 
*So why is there a high accuracy and a low precision and recall?* 
Because the class of "acceptable configurations" is much more represented (800+100=900 out of 1000). It is too easy to have a high accuracy with basically no specialization.  
On the one hand, accuracy gives a global classification measurement of our method. 
On the other hand, accuracy is unable to show the weaknesses of a naÃ¯ve classification strategy (e.g., "keeping everything as acceptable")

Intuitively, in our specific problem, we also need to measure the precision and recall
of our classification. It can give better insights on our ability to (1) discard non-acceptable configurations and (2) to keep acceptable configurations.

* **Precision**: measure our ability to correctly classify non-acceptable configurations. 
 In our example, precision is 40 / (100 + 40) = 40 / 140 = 29%. The learning wrongly classifies a large portion of non-acceptable configurations. 

* **Recall** (also called true positive rate): measure our ability to comprehensively identify non-acceptable configurations. In our example, recall is 40 / (40 + 60) = 40%. 
Here, the learning misses a large portion of non-acceptable configurations. 

A low recall may be problematic, since numerous non-acceptable configurations have not been classified as such: Users may still choose ***unsafe*** configurations. 
A low precision may be problematic as well since numerous non-acceptable configurations
are actually acceptable: Users may lose some ***flexibility***.

Precision and recall measure the ability of our system to **classify** non-acceptable configurations. However it says nothing on how safe and flexible is our resulting
specialized configurable system:
* How many valid configurations of our resulting specialized configurable system are truly acceptable? A low proportion and users will perform numerous configuration errors since the system is not enough safe. 
* How many configurations have been unnecessarily removed whereas they should still be reachable? A low proportion and users will have little configuration choices at their disposal.  

We thus need two additional metrics. 
**Specificity** (also called true negative rate) quantifies how flexible is
the specialized system. In the example, (800 / 800 + 100) =  89% of configurations will be reachable in the new specialized system.

**NPV** (also called negative predictive value) quantifies how safe is the specialized system. In In the example, (800 / 800 + 60) = 93% of configurations will be safe in the new specialized system. 

### How does a learning-based method compete with a non-learning based method?   

The example shows that, despite a low precision (29%) and recall (40%), the resulting specialized configurable system is quite safe (93%) and flexible (89%). 
Our learning phase misses some opportunities and introduces some errors, but the overall process may still be of interest since we have a *safer* system than originally and the flexibility is arguably reasonable.

Without a specialization, all configurations are classified as acceptable, and the metrics are as follows in our specific example:
* accuracy is 90%
* precision is ~0
* recall is  0  
* specificity is always 100% (the system is highly flexible) 
* NPV is 90% (the system is quite safe), but it is less than 93%

It shows an improvement of 3% for the safety of the system. Obviously, it is just an example. The potential of our technique mostly resides in cases the performance objective leads to numerous non-acceptable configurations: We can learn and identify them, leading to safer configurable systems. 

In general, a non-learning based method has always a precision is equal to 0, recall is equal to 0, specificity is equal to 100%. 
Hence we cannot beat a non-learning in terms of flexibility, since it keeps everything as acceptable. 

However the safety of the resulting system may be very low. It is equal to:
NPV_nonlearning = TN / (TN + TP) = number of acceptable configurations / number of configurations in total. We can thus compute a new metric, called **negNPV**, that is equal to NPV - NPV_nonlearning. The higher negNPV, the safer is our specialized system comparatively to a non-specialized system. Our empirical results confirm that we are safer in the vast majority of cases, even for a small training set. 

Similarly, we can compute negAccuracy (difference of accuracy between a non-specialized method and our learning-based method). 

### Reading grid of classification metrics

To sum up:
* there are three classification metrics that measure our ability to *classify* (accuracy, precision, recall). 
* accuracy gives a global classification "performance" of the learning phase 
* accuracy alone has limitations since it does not quantify the classification of non-acceptable configurations; precision and recall should be used in complement 
* high accuracy does not imply high precision or high recall: it is a theoretical statement (see formulae) and empirical results confirm there is no correlation between accuracy, precision and recall 
* NPV and specificity characterize how safe and flexible is the resulting specialized system (precision and recall cannot) 
* a high (resp. low) specifity means a high (resp. low) flexibility of the resulting system
* a high (resp. low) NPV means a high (resp. low) safety of the resulting system
* a low precision or low recall does not imply a low NPV or specificity. For instance, the classification metric can be low and, yet, leads to a highly safe and flexible system

A "specializer" (i.e., user of our specialization method) can employ:
* precision and recall for measuring the quality of the classification and perhaps identifying some "missing" opportunities. Overall it can drive the learning strategy: for example, with more (non-acceptable) configurations in the training set, precision and recall can perhaps be improved. 
* NPV and specificity for measuring the safety and flexibility of the resulting system. 
 In practice, there is (sometimes) a trade-off to find between flexibility and safety, depending on the specificities of the configurable system and its usage. Let us take two examples. 
 On the one hand, critical configurable systems have to be highly safe: Leaving a large portion of non-acceptable (and thus unsafe) configurations is not suited. On the other hand, there are configurable systems for which flexibility is the key since it attracts lots of customers. 
The extent to which safety and flexibility can be degraded is the decision of a "specializer": NPV and specificity help in such a task.

An important point is that some **performance objectives** (w.r.t. execution time, energy consumption, etc.) can lead to a high (resp. low) percentage of non-acceptable configurations. It can complicate the learning phase and thus both influence accuracy, precision, and recall. 
The definition of performance objectives can also influence NPV and specificity. For example, it is hard to be completely safe when numerous non-acceptable configurations have to be removed. You can miss a small portion of them and, yet, it is a lot of unsafe configurations that remain within the specialized system. 
Another factor is the **size of the training set**. It may influence both accuracy, precision, recall, NPV, and specificity. 

With our empirical results, we aim to further understand the effectiveness of our learning-based method w.r.t any performance objective and training set.
For each configurable system, performance objective (defining the percentage of acceptable configurations in the population), and size of the training set, we have computed accuracy, precision, recall, NPV, specificity, negNPV and negAccuracy. 

 
#### Reproducibility 

For the sake of verifiability and reproducibility of the experiments we provide:
* All heatmaps, plots, and figures. We can only present a few figures
   in a typical academic paper (because of the page number limit) and we
   aim to supplement it.  
* A git repository containing all data and the
   scripts used to: i) execute the procedures over different
   configurable systems; ii) generate all the different graphs,
   heatmaps, etc.  
* Details on how we have collected and prepared data

## Details of the content 

The repository is available here: https://github.com/learningconstraints/ICSE-17

### Heatmaps 

We have generated numerous [heatmaps](https://github.com/learningconstraints/ICSE-17/tree/master/1.heatmaps/results) for each configurable system. 

X-axis: size of the training set
Y-axis: percentage of acceptable configurations (as influenced by the objective treshold value)
Classification metric: the darker, the higher is the classification metric (usually from 0 to 1)
It includes accuracy, precision, recall, NPV, specificity, negNPV and negAccuracy. 
It is a total of 7 heatmaps per system, with variations of training sets and performance objectives. 

The tables (CSV files) out of which we have produced heatmaps are also available. 

### Graphs/tables

In addition we have fixed the number of acceptable configurations (in the population and as defined by an objective value) to 20%, 50%, and 80% and we have ploted the evolution of classification metric (accuracy, precision, recall, etc.) with regards to the size of the training set.
* 50% means that there are as non-acceptable configurations as there are acceptable ones. 
* 20% means that 80% of configurations are non-acceptable: It is a challenge to discard all of them. 
* 80% means that only 20% are non-acceptable: It can seen as challenging to learn from few non-acceptable configurations 

Such graphs help to visualize the effect of the size of the training set. 
The general trend is that the more configurations we have in the training set, more effective our technique is.
https://github.com/learningconstraints/ICSE-17/tree/master/2.metric_view/results


### Code 

 1. Learning phase  
 2. Computation of classification scores (see https://github.com/learningconstraints/ICSE-17/tree/master/2.metric_view and https://github.com/learningconstraints/ICSE-17/tree/master/3.synthetic_view)
 3. Depicting heatmaps or plots (https://github.com/learningconstraints/ICSE-17/tree/master/1.heatmaps) 


### Input data 

Performance measurements of subject systems. 
It is available here: https://github.com/learningconstraints/ICSE-17/tree/master/datasets



