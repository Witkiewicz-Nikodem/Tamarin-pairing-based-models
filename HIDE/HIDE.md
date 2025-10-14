# HIDE
Model is based on basic HIDE from this paper: https://eprint.iacr.org/2002/056.pdf. 

Algebraic model is impossible to cuntruct, becouse HIDE needs to perform point addition, what is not possible in Tamarin. Therefore We focused on abstract modeling. We provide 2 models. First one models HIDE in standard way and second one introduces modeling with oracle. 

Probably there is a way to improve model using private functions for key chain verification.
## notes on modeling BASIC HIDE
* public keys do not depend on corresponding private keys but on chain ID: <ID, ancestors_ID>.

* private keys are composed by ancestors secrets and public keys. The most important property is that key $S_t$, is chained with all ancestors $S_i$ and $s_i$. this together with encryption and decryption algorithms ensure that only a party who's key was evalueated using secrets corresponding with ID's used to produce public key is able to decrypt message. In other words, if somebody is able to decrypt message then:
    * he posses a proper private key.
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

#### executable
We provide 2 lemmas to check decryption correctness. There are two rules which create entities. Thereby there are two categories of entity, so We provde lemmas for both od them.
* __proper_decryption_entity1__ — There exists trace, in which decryption is reached.  (first category — root is entities parent)

* __proper_decryption_entity2__ — There exists trace, in which decryption is reached.  (second category — entity is entities parent)

#### key reveal
Here we cover two lemma examples in which secret or key deduction is not possible. We also provide one example where key is deducable.
* __skey_not_deducable__   — in some conditions it is possible to deduce key.
* __secret_not_deducable__ — in some conditions it is possible to deduce secret.
* __Deduce_child_skey__ —  it is possible to reach *deduce_child_skeys* rule;
* __Deduce_parent_skey__ — it is possible to reach *deduce_parent_secret* rule;
* __Deduce_secret__ — it is possible to reach *deduce_parent_skey* rule;

#### secrecy
* __message_secrecy__ — If a party's ltk and the Root ltk weren't compromised, then an adversary is not able to posses a message encrypted with the party's identity.


## Abstract oracle model
Model using orcale is quite similar to previous one. It construct tree structer in the same fashion, excluding private keys generation. Moreover key reveal mechanism is exactly the same. However Encrypion and decryption are modeled in different way. Unlike first model we attempted to model composed private key - private key is generated using private keys of it's ancestors. Therefore, the decryption process may check whether the private key is composed of the private keys coresponding to public keys used to decrypt message.

Before using this model, please read note about message_compromise lemma limitation. You will find the note at the end of this file.

### tree structure key generation
Private key is, actually list of private keys corresponding to Identyficators used to construct publick key.
$Pk = <Pk_1, Pk_2, ... , Pk_n>$
$Sk = <Sk_0, Sk_1, ..., Sk_n>$, where 0 is index of Root.

### Encryption and Decryption

#### Oracle in HIDE
The main idea is to use a oracle to model decryption. We use the name oracle to clearly distinguish that it is a black box from parties perspective. Oracle rules models decryption by honest parties, as well as adversary. Honest parties and adversary communicate with Oracle using channel (In, Out).
* Honest parties comunicate with Oracle using asymmetric encryption, so adversary is not able to poses any secret or private key. 
* Adversary communicate with Oracle without any encryption, just plain text.
Therefore, we provide relevant rules for both scenarios, so that honest parties and the adversary are able to decrypt message under circumstances suitable for their respective roles.

#### Iterative Decryption
Decryption must be applicable to parties at any level of tree depth. For this reason, decryption rules iteratively checks if subsequent $Pk_i$ and $Sk_i$ comes from the same party. If so then decryption is possible, otherwise decryption trace break.

### lemmas
We used --auto-sources to prove following lemmas. 

#### executable
We provide 2 lemmas to check decryption correctness. There are two rules which create entities. Thereby there are two categories of entity, so We provde lemmas for both od them.
* __proper_decryption_entity1__ — There exists trace, in which decryption is reached.  (first category — root is entities parent)

* __proper_decryption_entity2__ — There exists trace, in which decryption is reached.  (second category — entity is entities parent)

#### key reveal
Here we cover two lemma examples in which secret or key deduction is not possible. We also provide three examples (for each equation) where key is deducable.

* __skey_not_deducable__   — in some conditions (there are lot, please read lemma in code) it is not possible to deduce key;
* __secret_not_deducable__ — in some conditions (there are lot, please read lemma in code) it is not possible to deduce secret;
* __Deduce_child_skey__ —  it is possible to reach *deduce_child_skeys* rule;
* __Deduce_parent_skey__ — it is possible to reach *deduce_parent_secret* rule;
* __Deduce_secret__ — it is possible to reach *deduce_parent_skey* rule;


#### message secrecy
* __message_secrecy__ if receiver private key wasn't revealed or deduced then message is secret;
* __message_compromise__ if there are no restrcitions on key revealing then message will be available in unsecure channel. Note that in second part of the sentence we don't say "adversary will posses message". It is caused by a performance issues. Namely if we added to lemma *& (Ex #l. K(m)@l)*, then Tamarin couldn't find a prove. We lost patience after a few hours of waiting. In our model both cases are equal from logic perspective but in protocols where HIDE is component it would probably be usfull to be able to prove *& (Ex #l. K(m)@l)* statement.

## note on tree depth
We provided model for unbounded tree depth. If your protocol has a limit for tree depth, you should include this in your model to improve proving efficency.