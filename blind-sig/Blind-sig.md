# Blind signatures
the protocol model is based on scheme here under this link:
https://eprint.iacr.org/2004/064.pdf

# notes
Using standard modeling (using functions that directly represnt those in protocol) causes one inconvenience. Namely signature without blinding don't exists in model as a value. Nevertheless it is possible to verify The signature and prove lemmas. We provide two models. First one comes from master's thesis and is modeled in the way described above. The second model is improved version of the first and includes signature without blinding.

 

### original model
The first model not include signature witohut blinding, becouse at the time i produced it i didn't know how to provide this property. I tried to model it using only standard functions (not private) and without oracle. This led me to situation where equation providing signature without blinding wasn't convergent: unblind(signn(blind(r,m),ltk), r) = signn(m,ltk). Nevertheless it is possible to verify The signature and prove lemmas.


### improved model
The second model is an improved version of the first. We used rule Oracle_unblind_sig and three private functions. The functions are able to recover values used to produce blinded signature: message, random value, long term key. They are used in the rule Oracle_unblinding_sig to produce not blinded signature. Nevertheless, the model correctly represents the protocol, as the aforementioned functions are private and the recovered values are not revealed. 

### Lemmas
for both models we provide following lemmas: 
* __lemma executable__: there exist trace where steps leads to proper signing and verification
* __secrecy__: until verified is processed, adversary do not know the message. <br> notes:
    * first model doesn't allow adversary to evaluate message, so the lemma is trivial
    * second model also doesn't allow the adversary to evaluate the message, because we commented out an equation that enables it. We comented out the equation, because it causes non termination, it is probably possible to resolve issue by adding relevant sources lemmas.

* __non_repudiation__: if signature under a message was verified using a party's public key, We can be sure that the party signed the message..
