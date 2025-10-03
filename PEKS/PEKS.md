# PEKS
model is based on this paper: https://crypto.stanford.edu/~dabo/pubs/papers/encsearch.pdf.

We provide algebraic and abstract models in the files named PEKS-algebraic.spthy and PEKS-abstract.spthy, respectively.

### lemmas
for bothe models we prove following lemmas: 

* __proper_execution__ — this indicates the possibility of correct protocol exectuion. Specifically, it means that it is possible to positively check;

* __word_secrecy__ — If receiver's private key is not compromised then word remains secret;

* __Tw_compromise__ — If receiver's key is compromised then adversary can construct it.