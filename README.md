# Tamarin-pairing-based-models
Here I provide models of protocols based on bilinear pairings: IBE, SPKE, blind signatures, HIDE, simple CP-ABE. They are result of my Master's thesis and are meant to be building blocks as part of protocols. 

### notes on models
#### abstract and algebraic
The models are built in 2 fashions. We call them algebraic and abstract. First One consists in building model by directly rewriting protocol's algebra into model (uses builtins diffie-hellman and bilinear-pairings). Second one is based on functions which represent algorithms in protocol without algebr.

#### oracle technique
Work on models resulted in interesting abstract modeling technique. Namely oracle technique. It is possible to simulate parties behaviour (both regular parties and adversary), by external rules. If you are interested in this, check out HIDE models, one of them uses oracle technique. 

#### table of models
| Protocol              | Algebraic | Abstract | With Oracle |
|-----------------------|-----------|----------|-------------|
| IBE                   | YES       | YES      | NO          |
| PEKS                  | YES       | YES      | NO          |
| Blind Digital Signatures | NO        | YES      | NO          |
| HIDE                  | NO        | YES      | YES         |
| CP-ABE                | NO        | YES      | NO          |

