# Learning-Based Performance Specialization of Configurable Systems (submitted at ICSE'17)


A large scale configurable system typically offers thousands of options or parameters to let the engineers customize it for specific needs. Among the resulting many billions possible configurations, relating option and parameter values to desired performance is then a daunting task relying on a deep know how of the internals of the configurable system. We propose a staged configuration process to narrow the space of possible configurations to a good approximation of those satisfying the wanted high level customer requirements. Based on an oracle (e.g. a runtime test) that tells us whether a given configuration meets the non-functional requirements (e.g. speed or memory footprint), we leverage machine learning to retrofit the acquired knowledge into a variability model of the system that can be used to automatically specialize the configurable system. 

**SPLC'16**: In the repository https://github.com/learningconstraints/SPLC-16,  you will find all the required artefacts to replicate the experiments included in our paper submitted to the 20th International Systems and Software Product Line Conference. The goal was mostly to avoid *faulty* configurations and correct a video generator developed in the industry. The oracle was simply a yes/no procedure. 

In **ICSE'17**, we have considered performance qualities. The oracle is parametrized with an ***objective value*** that users of our specialization method can control. Hence our proposal can be seen as a generalization of the idea originally proposed in SPLC'16. Moreover our empirical results cover more systems and classification metrics while taking training sizes and objective values into account.

**Objective of this page**: Our ICSE'17 results show that, for many different kinds of objectives and performance qualities, the approach has interesting accuracy, precision and recall after a learning stage based on a relative small number of random samples. This page describes how we gather and process input data, how we compute classification metrics. Furthermore, all plots, heatmaps, and figures can be visualized. We aim here to supplement our original material (it is impossible to include all relevant figures we have generated!) and also support an independent verification and replication of our study. 

 
## Empirical Study

#### Subject systems
We performed our experiments on a publicly-available dataset used in previous works. The dataset includes sets of configurations of real-world configurable systems that have been executed and measured over different performance metrics (more details hereafter). Configurable systems come from various domains and are implemented in different languages (C, C++ and Java). The number of options varies from as little as ten, up to about sixty; options can take Boolean and numerical values. The two last columns show, first, the total number of different possible configurations existing for the system and, last, the number of configurations that have been generated and measured in previous works ("All" meaning all different configurations have been measured).
It should be noted that most benchmarks measure a single performance value, except for x264 where benchmarks evaluate 7 performance metrics.

#### Experimental setup 
For each subject system, we have randomly selected a certain number of configurations from the ones available in the public dataset: our ML method operates over a ***training sample*** to learn a separating function. All remaining configurations form the ***test sample***, i.e., it is the whole configuration set minus the training set. They are used to verify whether the classification of our ML is correct. For instance, the public dataset contains 69,000 different configurations for x264. If we used 500 of them to learn constraints, then we evaluate the classification quality of ML on the remaining 68,500 configurations.

For each configurable system, we make ***vary the sample size*** one by one, from 1 to the total number of configurations. We make vary the *objective value* between the lower and upper bounds of the performance measured in each dataset. We notice that threshold values have direct influence on the
***percentage of acceptable configurations***. For instance, for x264, an execution time (speed) threshold less than 100s leads to only 21.4% of acceptable configurations. We thus vary the objective value from 0% to 100% of acceptable configuration with steps of 5%.

#### Reproducibility 

For the sake of verifiability and reproducibility of the experiments we provide:
 - All heatmaps, plots, and figures. We can only present a few figures
   in a typical academic paper (because of the page number limit) and we
   aim to supplement it.   
  - A git repository containing all data and the
   scripts used to: i) execute the procedures over different
   configurable systems; ii) generate all the different graphs,
   heatmaps, etc.  
  - Details on how we have collected and prepared data

## Details of the content 

### Heatmaps 

X-axis: size of the training set
Y-axis: percentage of acceptable configurations (as influenced by the objective treshold value)
Classification metric: the darker, the higher is the classification metric (usually from 0 to 1)

### Code 

 1. learning phase  
 2. computation of classification scores
 3. depicting heatmaps or plots
(repeated 10 times)

### Input data 
 
Performance measurements of subject systems 



