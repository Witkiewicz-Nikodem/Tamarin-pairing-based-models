# HIDE
Model is based on basic HIDE from this paper: https://eprint.iacr.org/2002/056.pdf. 

Abstract model is impossible to cuntruct, becouse HIDE needs to perform point addition, what is not possible to model. Therefore We focused on abstract modeling. We provide 2 models. First one models HIDE in standard way and second one introduces modeling with oracle. 

Probably there is a way to improve model using private functions for key chain verification.
## notes on modeling BASIC HIDE
* public keys do not depend on corresponding private keys but on <ID, ancestors_ID>.

* private keys are composed by ancestors secrets and public keys. The most important property is that key $S_t$, is chained with all ancestors $S_i$ and $s_i$. this together with encryption and decryption algorithms ensure that only a party who's key was evalueated using secrets corresponding with ID's used to produce public key is able to decrypr message. In other words, if somebody is able to decrypt message then:
    * he posses a proper key.
    * The key was constructed by his parrent.
    * the parent's key was constructed by grand parent and so on and so forth until we reach root.
    * looking from leaf perspective, they are all chained with root.

* protocol forms tree structure like PKI
* in real world scerario new entity needs to authenticate itself in a PKG entity then it gets a private key.

## some naming notes
* __private key__ corresponds to $S_i$.
* __secret__ corresponds to $s_i$.

## Abstract standard model
The model is quite easy but do not represent protocol in 100%.

We omit values $Q_i$ as we do not use algebra here and security of protocol don't rely on them (or at least should not rely on them). 

### tree structure
HIDE creates tree structure. Party which is part of tree is called Entity. There are two ways of creating one. first one is by rule  *Register_Entity_Root*. It requires in premise presence of Root private key. second one is by rule *Register_Entity_Entity*. It requires in premise presence of Entity private key.


### IBE based encryption model
__public keys__ are generated using ancestors' identities 

__private Keys__ are generated like in IBE scheme and they do not encompass knowledge about ancestors' secrets, except root ltk. However they are genetrated using publick key, thereby using ancestors' identities.

__encryption__ needs ID and root's public key
__decryption__ needs secret key corresponding to public key

### key reveal
Security of symmetric or asymmetric encryption in "Tamarin book" is described this way: "if ltk is not compromised then adversary is not able to posses encrypted message". We define secrecy in HIDE similarly. Although reveleaing particular secrets may cause possibility of deduction another secrets. Thereby compromising a message. We provide below the equations depicting the deduction of secrets according to HIDE description.

* $s_n = \left( S_{n+1} - S_n\right) \cdot P^{-1}_{n+1}$
* $S_n = S_{n+1} - s_nP_{n+1}$
* $S_n = S_{n-1} + s_{n-1}P_n$

This protocol's behavior requires us to model the relevant mechanism. We achieved this by using a set of rules that reveal secrets according to the above equations.

### lemmas
We provide lemmas under one restriction: we consider that there is only one root.

We added a tree depth counter to the entity structer and use it only in lemmas.

## executable
We provide 2 lemmas to check decryption correctness. There are two rules which create entities. Thereby there are two categories of entity, so We provde lemmas for both od them.
* __proper_decryption_entity1__ — There exists trace, in which decryption is reached.  (first category — root is entities parent)

* __proper_decryption_entity2__ — There exists trace, in which decryption is reached.  (second category — entity is entities parent)

## key reveal
Here we cover two lemma examples in which secret or key deduction is not possible. We also provide one example where key is deducable.
* __skey_not_deducable__   — in some conditions it is possible to deduce key.
* __secret_not_deducable__ — in some conditions it is possible to deduce secret.
* __Deduce_skey__ — in some conditions it is not possible to deduce key.

## secrecy
* __message_secrecy__ — If a party's ltk and the Root ltk weren't compromised, then an adversary is not able to posses a message encrypted with the party's identity.


## Abstract oracle model
