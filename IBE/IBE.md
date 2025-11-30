# IBE
model is based on this paper: https://eprint.iacr.org/2001/090.pdf.
We provide algberaic and abstrac models in files respectively IBE-algebraic.spthy and IBE-abstract.spthy

### lemmas
for both models we prove following lemmas:

* __proper_execution__ — This indicates the possibility of correct protocol execution. Specifically, it means that it is possible to decrypt a message using the party's private key that corresponds to the public key used to decrypt message;

* __message_secrecy__ — If B sent encrypted message and both Receiver's secret key and Master's secrete key weren't revealed then adversary is not able to obtain message;

* __receiver_ltk_revealed__ — If receiver's key is compromised then adversary is able to decrypt messages;

* __master_ltk_revealed__ — If Master's key is compromised then adversary is able to decrypt message.