[Link](https://www.youtube.com/watch?v=1YHsSFWn5OA&list=PLypiXJdtIca5sxV7aE3-PS9fYX3vUdIOX&index=18)
## Intro:
* Drug discvoery is a time consuming process (~ 10 years). A challenging search problem with > $$10^{60}$$ drug-like molecules.
* Functional and Chemical spaces: two search spaces that have numerical scores

### 3 Possible Methods:
* Found via simulation (MD, force field ..etc)
* Virtual Screening with filtering stages
* De novo drug design (most ambitious) (Optimization, Evolutionary strategies, generative models (VAEs, GAN, RL).


##### Virtual Screening Limitations:
* It loses coverage (at best, we can screen $$10^9$$ compounds)
* Traditional techniques are not accurate (based on hand-crafted features)

##### De-novo Drug design:
* Hard because we want structure.
* More efficient coverage.
* Limitation:
* 1. We need to snythesize compounds
* 2. Traditionally hard to make them work

##### Resources:
* Possible numerical evaluations: PK scores (Protein Affinity).
* ZINC library (comercially available compounds)


### Deep Learning:
* Deep Learning has achieved human-level accuracy due to its feature-extraction ability
* Deep Generative Models can generate media with desired properties.

### Discussed technique:
* Graph Neural Networks (GNNs) are useful  becuase molecules can be modelled as graphs.
* Paper: atoms are represented as nodes, bonds as edges. [__A Deep Learning Approach to Antibiotic Discovery__](https://www.cell.com/cell/fulltext/S0092-8674(20)30102-1?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS0092867420301021%3Fshowall%3Dtrue)

### Lecture Main 3 Parts:

1. Graph Neural Networks for AntiBiotic Discovery
2. Incorporate Biologicsl Knowledge in GNNs
3. Generative models for Denovo Drug Design __(People can generate images, text, but not realyl graphs)__


## Part 1: Graph Neural Networks for AntiBiotic Discovery
* Many resistant bacteria. We need to find new antibiotics.
* On his collbaoration with Broad Institute:
  1. Collected Data of __only 2500__ molecules, with measured growth inhibition against E.Coli
  2. GNN predictive model to predict a specific molecule's inhibition
  3. Typically: 
    * Earlier hand-engineered features: Molecular fingerprint .Sophisticated feaures: Morgan Fingerprint. Very high dimensional feature (~2048) different structures merged by hash function.
  4. Then SVM or any classifier
* Problem: hand-engineered features can miss some of the antibacterial patterns.
* GNNs learn representations directly. Graph convolution to compress into low dimensional space. Followed by Feed forward.
* Experiment: Ranking 10K compounds and experimentally test top-candidates.
* Discovered Halicin (low similarity to existing antibiotics). Was known and produced but not used as an antibiotic before (__Important: previously present molecules so that synthesizing is easy!__)
* Strong in vivo inhibition of pan-resistant A.baumannii bacteria

## 2. Incorporate Biologicsl Knowledge in GNNs
* Finding Covid-19 drug combinations
* Two drugs are synergistic if effect(a,b) >> effect(a) + effect(b)
* Goal: Train a model to predict whether a drug combination is synergistic
*  We want use biology bec. we have very few data. eg: Covid-19 uses specific receptors, can we inhibit this?
*  How: __ChEMBL and NCATS__ datasets tell us about interactions. We train a GNN first, to predict these interactions, and it thus learns useful features. Then, using representations for each two drugs from the same network, learn to combine them into a single vector representation, and predict the combination antiviral activity. We compare tha antiviral activity of both combined vs sum of both, and thus predict the synergy. (Learned from __200 combinations only__ !)
*  Thus, predicted and NCATS Vero E6 cell-assay verified that, remdesivir+reserine are synergetic, and (remdesivir + IQ-1S) -- most effective

##  3. Generative models for Denovo Drug Design 
* More ambitious goal
* How to generate molecular graphs ?
  * earlier: __Sequence based__. RNNs to generate moleule into SMILES string (domain specific molecule description language), but this string representation is quite brittle. Two almost identical graphs have quite different strings (that's why it performs poorly)
  * earlier: __node-by-node__ generation: straightforward. Problem: Molecules are very sparse, but this required $$N^2$$ complexity since we predict N edges for each edge. They fail miserable for larger molecules (as small as 40 atoms).  
 * We need to leverage inductive biases. Molecules typically have low tree width, this inspired the model: Junction tree variational autoencoder (inspired by Junction Tree Algorithm). We basically use smaller vocabulary with longer words. We divide graphs into motifs (250K graphs produced ~638 motifs, for 99.9% coverage). Now, use a Hierarchiarl VAE. __motif-by-motif generation__.
* Method does not handle molecues with cycles.
* In an experiment, with QED score as a success metric, Sequence methods achieved 58.5% success rate, node by node achieved 73.6%, motif-by-motif achieved 76.9%



## Walid's final notes:
* GNNs can learn from few examples (2500)
* Good for structural representaiton
* Generation is still hard
* Good for previously synthesized molecules __(eg: ZINC library) (interactions: ChEMBL and NCATS)__
* Eg: Compare functionality to proteins already made in our insects? (general approach: rank already present molecules)

