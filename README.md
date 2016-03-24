# Learning Constraints
Variability intensive systems may include several thousand features allowing for an enormous number of possible configurations, including wrong ones (e.g. the derived product does not compile). For years, engineers have been using  constraints to a priori restrict the space of possible configurations, i.e. to exclude configurations that would violate these constraints. The challenge is to find the set of constraints that would be both precise (allow all correct configurations) and complete (never allow a wrong configuration with respect to some oracle). In this repository, you will find the implementation of a machine learning approach to infer such product-line constraints from an oracle that is able to assess whether a given product is correct. 

1. We generate products from the product line, keeping for each of them its resolution model. 
2. We classify these products according to the oracle, and use their resolution models to infer cross-tree constraints over the product-line. 
3. We offer the experimentation data to validate our approach on a product-line video generator, using a simple computer vision algorithm as an oracle. 

SPLC'16 In the XXX repository, you will find all the required artefacts to replicate the experiments included in our paper submitted to the 20th International Systems and Software Product Line Conference



