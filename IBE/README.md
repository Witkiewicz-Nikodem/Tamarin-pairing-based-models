# IBE
model is based on this paper: https://eprint.iacr.org/2001/090.pdf.
We provide algebraic and abstract models in the files named IBE-algebraic.spthy and IBE-abstract.spthy, respectively.

### lemmas
for both models we prove following lemmas:

* __proper_execution__ — This indicates the possibility of correct protocol execution. Specifically, it means that it is possible to decrypt a message using the party's private key that corresponds to the public key used to decrypt message;

* __message_secrecy__ — If B received and correctly Dec message then adversary didn't obrain the message;

* __receiver_ltk_revealed__ — If receiver key is compromised then adversary is able to decrypt messages;

* __master_ltk_revealed__ — If master key is compromised then adversary is able to decrypt message.