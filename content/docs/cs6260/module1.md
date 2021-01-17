---
title: "Module 1"
description: ""
lead: ""
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "cs6260"
weight: 100
toc: true
---

<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
</script>

<style>
    .red{
        color: red;
        }
</style>

## Module Introduction

![](module1-0000.png)

This module will describe the topics of the course, and explain symmetric key encryption. We'll learn what security is in the context of symmetric-key cryptography and our first symmetric-key encryption scheme. This scheme will turn out to be the most efficient and secure encryption scheme in the world, but we'll also learn what its limitations are.

## L1: Course Objectives

Objectives:

+ We will study practical cryptographic schemes in both settings (n.b.: the two schemes alluded to here are explained in a later lecture), e.g.:
    + AES-based modes of operation
    + HMAC
    + RSA-OAEP encryption
    + RSA and DSA signatures
+ We will learn the more fundamental principles of:
    + What is a good scheme
    + How to estimate and compare schemes: this rarely done ins security courses but is very important in modern cryptography


## L2: Introduction

Cryptography literally means "secret writing"

Remember that Julius Caesar used cryptography (n.b. research the [caeser cipher](https://en.wikipedia.org/wiki/Caesar_cipher)),
The [Eniga machine](https://en.wikipedia.org/wiki/Enigma_machine) was employed by Nazi Germany in World War 2. Modern day eryptography was created in the past 40 years, and this course is an introduction to modern cryptography.

The main goals of cryptography are:

+ **Data privacy:** how to protect private data from eavesdroppers
+ **Data authenticity:** ensuring the origin of the data is what we think it is
+ **Data integrity:** ensuring that the data hasn't been changed since it was created

That said, these may not all be required. And we will also talk about other goals at the end of this course.

Most of us use cryptography behind the scenes every day without realizing. It's used all the times when we do every day tasks like shopping online, paying bills, using our cellphones, etc. We can even stream online and crypto will be used in the background. Half of the internet's traffic is encrypted nowadays, and this might increase in the future.

## L3: Players and Settings


Let's look at our main crypto players and settings. 

When two parties want to communicate securely with data privacy. They are often called Alice and Bob but we will call them _S_ (sender) and _R_ (receiver). A third, usually an eavesdropper named Eve, is called _A_ (attacker) here. The sender wants to wants to send messages to the receiver. For security to exist, there needs to be some secret information not known to the attacker. Otherwise _A_ can do anything _S_ and _R_ can do.

### Symmetric-key Setting

![](module1-0004.png)

One of the settings in cryptography is called the symmetric-key setting. A secret key _K_ is shared by _S_ and _R_, but not _A_. The old cryptos: Caesar's cipher, enigma machine, substitution ciphers, they all operate in this symmetric key setting. There are better modern symmetric-key cryptographic schemes, and they have much better efficiency.

For this setting to work, the sender and receiver have to agree on a shared secret key before communicating. This can be difficult, e.g. when shopping online, how can you send your credit card number to a site you're visiting for the first time? For this a different setting may be needed called: the asymmetric-key setting or public-key setting.


### Asymmetric-key Setting

![](module1-0005.png)

In this setting, only the receiver will hold the secret key _skR_, and everyone will have the public key _pkR_ of the receiver. Most importantly, though, so will the sender. How everyone gets the public key we will learn later on, but for now let's assume there's a shared trusted directory which lists all party's names and public keys together for anyone to look up.

This setting is much more difficult than the symmetric-key setting. In fact, all of public-key cryptography happened since the late 1970's. Even in the 1970's people didn't believe the asymmetric-key setting was possible. This setting means that two people can meet in public for the first time, without sharing a secret, and be able to communicate securely. The RSA cryptosystem is an example of a public-key cryptographic scheme.

## L4: Goals and Primitives

As discussed before, in this course, we'll explore the main goals of cryptography: 

+ data privacy
+ data authenticity, and
+ data integrity

![](module1-0006.png)

We'll explore all these in the contexts of both the symmetric and asymmetric key settings.
### Data Privacy

For data privacy:

+ the solution in the symmetric key setting will be symmetric key or secret key encryption, and

+ the solution in the asymmetric key setting will be asymmetric key or public key encryption.

### Data Authencitiy and Data Integrity


For data authenticity and for data integrity, the solutions will in each setting will be the same. And so:

+ in the symmetric key setting the solution will be message authentication code (MAC)
+ in the asymmetric key setting, the solution will be digital signature scheme

## L5: How Good is a Scheme


In this course, we'll study how a lot of crypto-algorithms operate. We will use the terms "protocol" and "scheme" interchangeably. How do we determine how _good_ the scheme is? There are two approaches:

* Trial and error
* Provable security

![](module1-0008.png)

### Trial and error

In the trial and error approach, you design a scheme that you and your colleagues feel is secure. You try to attack it, if you do find a vulnerability, you fix it, but if you can't you go ahead and use it. If you an attack is found in the future, it may be a bit more expensive to fix, there may be some bad press, but you still fix it and deploy it. This cycle continues until there are no more attacks, and at that point you consider the scheme stable. Does that mean the scheme is secure? We don't really know, but we hope it is. An alternative to this is the provable security approach.

### Provable Security

This was designed in the 1980's, and with this approach, it's possible to prove that a scheme is secure. It provides security guarantees. It's a proof by contradiction. You posit that if an attack is found then a _hard problem_ will have to be solved, like factoring primes. This approach requires a good definition what you mean by "secure" means and that is what we will explore in this course. We will learn this approach in depth later on, studying definitions for various cryptographic goals and discover that this can also be used to prove insecurities. That will lead us to find attacks more easily.

## L6: Symmetric Encryption 

For now, let's learn the syntax of a symmetric encryption scheme. All symmetric encryption schemes consist of:

* **The message space:** _MsgSp_ — This describes the limit of the message we can encrypt. For many schemes, this is a set of all bit strings, or a bit string of a specific length, e.g. 120 bits
* **The key generation algorithm:** _K_, this describes how it is that we determine the key for encryption. However, in many algorithms, the key may be as simple as a random number from a key space _KeySp_.
* **The encryption algorithm:** _ε_
* **The deencryption algorithm:** _D_

## L7: Key Generation Algorithm 

![](module1-0011.png)

(n.b. I'm swapping the uppercase red <span class="red">_K_</span>, for a lowercase red <span class="red">_k_</span> to aid plaintext and colorblind readers)

Usually the key generation algorithm _K_ is a randomized algorithm, so we feed it some randomness, and the output is a secret key <span class="red">_k_</span>. This secret key is then shared by both the sender and the receiver.

We're only discussing the inputs and outputs of the algorithms for now, and so:

* **To encrypt a message** — the sender will run the encryption algorithm _ε_ that produces a cipher text _C_. The inputs for _ε_ are:
    * The shared secret key <span class="red">_k_</span>
    * The message _M_ such that _M ∈ MsgSp_
    * One last input to ensure that the same <span class="red">_k_</span>, and _M_ don't map to the same output. This may be:
        * Randomness
        * Any other state, such as a counter
* **To decrypt the message** — the receiver runs the decryption algorithm _D_  which is deterministic, and will always produces the same output _M_ for the same ciphertext \\(C\\). It takes as inputs:
    * The shared secret key <span class="red">_k_</span>
    * The ciphertext \\(C\\).

For an encryption scheme to be useful, it should ensure that for combination of valid messages, and keys, we should be able to encrypt and decrypt the original message back. Or more formally, for all _M ∈ MsgSp_ and _<span class="red">_k_</span> ∈ KeySp_ / output of K, ensure that _D(<span class="red">_k_</span>,ε(<span class="red">_k_</span>,M)) = M_.

## L8: OneTimePad

![](module1-0012.png)

The **OneTimePad** is the best-known encryption scheme in the world. It originated as the Vernam cipher in 1919. It's a very simple scheme.

* The **Message Space** \\(\mathit{MsgSp}\\), and the **Key Space** \\(\mathit{KeySp}\\) are both a set of \\(n\\) bit long strings, i.e. \\( \mathit{MsgSp} = \mathit{KeySp} = \\{ 0 , 1 \\}^{n} \\).
* The **Key Generation algorithm** \\(\kappa\\) selects a random \\(n\\) bit long string \\({\color{Red} K}\\), where \\(n\\) is the same bit length as the message \\(M\\).
* The **Encryption Algorithm** \\(\varepsilon\\) operates by applying exclusive or (XOR) to the message and the key to produce the ciphertext \\(C\\), i.e. \\(\varepsilon({\color{Red} K}, M) : C \leftarrow M \oplus {\color{Red} K}\ \mathrm{return}\ C\\)
* The **Decryption Algorithm** \\(\varepsilon\\) operates by XOR-ing the ciphertext and the key to produce the message back again, i.e. \\(\varepsilon({\color{Red} K}, C) : M \leftarrow C \oplus {\color{Red} K}\ \mathrm{return}\ M\\)

It is important that the key is only used once, and so you need a new key to encrypt a new message, or you need a very long key so you can use different chunks to encrypt different messages. But you can never reuse the same key for different messages.

Intuitively, we can tell that a scheme is secure if someone without the secret key cannot discover any information by looking at the message. Now let's take a look at how to describe it more formally.

## L9: Perfect Shannon Security 

The first formal definition of security was proposed by Claude Shannon, known as the "father of information theory". An asymmetric encryption scheme is perfectly secure (or Shannon secure) if for every ciphertext \\(C\\) and messages \\(M_1\\) and \\(M_2\\), the probability of the either message \\(M_1\\) or \\(M_2\\) being encrypted to \\(C\\) is equal.

That is to say, it is just as likely that it is one message, than it is any other message. This captures the intuition that ciphertexts leak no information, since it is impossible to ascertain the contents of the message. More formally, though, this is represented as:

$$Pr[\varepsilon({\color{Red} K},M_1 ) = C] = Pr[\varepsilon({\color{Red} K},M_2 ) = C]$$

## L10: Theorem and Proof 

Let's prove that the OneTimePad is Perfectly/Shannon Secure.

![](module1-0015.png)

Recall, that we need to proove that it is likely that any ciphertext can be any message.

1. So, let's select a ciphertext \\(C \in \\{0,1\\}^n\\)

<!-- ![](module1-0012.png)
![](module1-0013.png)
![](module1-0014.png)
![](module1-0015.png)
![](module1-0016.png)
![](module1-0017.png) -->