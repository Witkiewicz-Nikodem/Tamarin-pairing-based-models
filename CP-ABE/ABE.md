# simple ABE
We propose a method for building your own ABE model. The method considers policy operators OR and AND. Further development is planned. First we will attempt to develop more policy operators. 

Here we provide 2 links to understand how ABE works, particullary we use difinitions as in survey but our model is based on first paper:
1. [first CP-ABE paper](https://web.cs.ucla.edu/~sahai/work/web/2007%20Publications/SSP2007.pdf)
2. [ABE survey](https://www.oaepublish.com/articles/jsss.2023.30)

## Short dictionary
### parties
* __DO__ - data owner - party who encrypts data;
* __DU__ - data user - party who decrypt data;
* __TA__ - trusted athority - party who generates keys.
### types of ABE
We consider KP-ABE and CP-ABE. An ABE taxonomy can be more precise and complex, as in aforemantioned survey. Nevertheles, this work is a first step, so wee keep it simple.

* __KP-ABE__ - Generally The acces policy is encoded into the secret keys of the ciphertexts are created using a set of attributes. A ciphertext can only successfully be decrypted by a user if the attributes of the encrypted data fulfill the access policy embedded into the user's secret key.
* __CP-ABE__ - In contrast to KP-ABE schemes, in CP-ABE, the secret keys of users are associated with a set of attributes instead of an access policy. The access policty is encoded into cyphertext.

## CP-ABE Model description
Given attributes 1, 2, and 3, we specify an example policy in the following diagrams.

![alt text](ABE_policy_1.png) 

![alt text](ABE_policy_2.png)

### functions and equations
## functions
* __pkk__ - function that evaluates master public key;
* __enc__ - function that encrypts message;
* __dec__ - function that decrypts message;
* __skk__ - function that creates user secret key. 

## equations

### operators OR and AND
We model operators OR and AND in our model. operator att1 AND att2 means that party needs key associatet with both att1 and att2 attributes. att1 OR att2 means that party needs key with one of these attributes. 

To build proper equations, one can firstly draw policy diagram. For instance lets have a look at first diagram in this file. It might be described as 1 OR (2 AND 3). then we might say: 

* key associated with att1 is enough to decrypt message.
* key associated with att2 and att3 is enough to decrypt message.

But what if somebody has key with attributes att1 and att2. This key is also relevant and can decrypt message encrypted under aforementioned policy.

How to model that? We need to look for keys associated with a minimal set of attributes requiered to decrypt a message, encrypted with a policy. Those two points above represent such a set. We know that keys might be associated with different number of attributes (look at sk1 and sk2). Additionally functions takes list (not set) of attributes, so we always use attributes in the same order: 1,2,3. 

one defined:
1. the minial sets of attributes;
2. maximum amount of attributes associated with key - (later we will call it key length);
3. order of attributes;

is ready to construct equations. Each set is analised according to all possible key lenght.

We always use all attributes in encryption function, as we always use all attributes to create policy. Then in function evaluating DU secret key we use attributes which corresponds to minimal set of attributes requiered to decrypt a message. all other atributes in key may be random. This way in equation we define which keys can decrypt messages and which don't.

Here provide equations for two policies: 
* policy 1: a1 or (a2 and a3) 
* policy 2: a1 or a2

Note that message encrypted by both policy 1 and policy 2 might be decrypted by message with a1 attribute.

#### policy 1
* we have two minimal sets of attributes:
    1. a1
    2. a2, a3
* key length is 3.
* we order attributes ascending: a1, a2, a3

for each minimal set of attributes we produce relevant equation:
1. $dec(pkk(msk), enc(pkk(msk), m, a_1, a_2, a_n), skk(msk, a_1, x_1, x_2)) = m$
2. $dec(pkk(msk), enc(pkk(msk), m, a_1, a_2, a_n), skk(msk, x, a_2, a_3)) = m$

#### policy 2
* we have two minimal sets of attributes:
    1. a1
    2. a2
* key length is 3 - thats becouse we want to have 1 skk function for both policies and previous one needs 3 attributes.
* we order attributes ascending: a1, a2, a3

for each minimal set of attributes we produce relevant equation:
1. $dec(pkk(msk), enc(pkk(msk), m, a_1, a_2, a_3), skk(msk, a_1, x_1, x_2)) = m$
2. $dec(pkk(msk), enc(pkk(msk), m, a_1, a_2, a_3), skk(msk, x_1, a_2, x_3)) = m$


#### NOTES
* Note that x stands for any value, as these values are irrelevant for decryption but mandatory in the equations to ensure that every key length case is considered and keep only one function for key evaulation.

* In realistic scenario we could make these policies simpler, as they use the same attributes. We can see that here we have one minimal set duplication and one minimal set is actually subset of minimal set in second policy. Nevertheless it is only example.


## Model description
### key_gen
key generation is done once. We create Fresh master secret key (msk). This key is used to produce all possible DUs keys. For each key we produce party and action to use it later in lemmas. We also create fact MSK for secrecy definition and we output to public channel public key so advesrary knows it.

### encrypt
We just evaluate function enc with public key and fresh value m and list of attributes.

### Decryption
Some party just decrypt message with public key cyphertext and its Secret key. 

### Reveal
We define secrecy as in Tamarin book is defined secrecy for symmetric and asymmetric encryption. If key is not reveled then adversary do not know encrypted message. For this reason we added rules that reveal keys.

### lemmas 
* __P1_all_dec_ex__ - covers proper decryption for policy 1, with all possible proper keys .
* __P2_all_dec_ex__ - covers proper decryption for policy 2, with all possible proper keys. 
* __secrecy__ - prove that secrecy holds If any key is not reveled.

### restriction
* __TA_onlyOnce__ - to make model simple we restricted it to only one trusted authority.