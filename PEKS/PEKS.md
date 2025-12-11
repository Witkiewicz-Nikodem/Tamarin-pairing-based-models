# PEKS
model is based on this paper: https://crypto.stanford.edu/~dabo/pubs/papers/encsearch.pdf.

We provide algebraic and abstract models in the files named PEKS-algebraic.spthy and PEKS-abstract.spthy, respectively. Note that they are slightly different in *Test* stage.

We also provide proof for abstract model that adversary can't distinguish two PEKS function evaluations despite he knows what messages were used to evaluate them. The proof is in PEKS-abs-obs-eq.spthy file. We tried to do the same with algebraic model but it's proof is computationally too complex.

### lemmas
for both models we prove following lemmas : 

* __proper_execution__ — this indicates the possibility of correct protocol exectuion. Specifically, it means that it is possible to positively check;

* __word_secrecy__ — If receiver's private key is not compromised then word remains secret;

* __Tw_compromise__ — If receiver's key is compromised then adversary can construct it.