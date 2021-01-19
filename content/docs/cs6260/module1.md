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

<style>
    .red{
        color: red;
        }
</style>

## Introduction

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

Most of us use cryptography behind the scenes every day without realizing. It's used all the time when we do every day tasks like shopping online, paying bills, using our cellphones, etc. We can even stream online and crypto will be used in the background. Half of the internet's traffic is encrypted nowadays, and this might increase in the future.

## L3: Players and Settings

Let's look at our main crypto players and settings. 

When two parties want to communicate securely with data privacy. They are often called Alice and Bob but we will call them \\(S\\) asdasd (sender) and \\(R\\) (receiver). A third, usually an eavesdropper named Eve, is called \\(A\\) (attacker) here. The sender wants to wants to send messages to the receiver. For security to exist, there needs to be some secret information not known to the attacker. Otherwise \\(A\\) can do anything \\(S\\) and \\(R\\) can do.

### Symmetric-key Setting

![](module1-0004.png)

One of the settings in cryptography is called the symmetric-key setting. A secret key \\(K\\) is shared by \\(S\\) and \\(R\\), but not \\(A\\). The old cryptos: Caesar's cipher, enigma machine, substitution ciphers, they all operate in this symmetric key setting. There are better modern symmetric-key cryptographic schemes, and they have much better efficiency.

For this setting to work, the sender and receiver have to agree on a shared secret key before communicating. This can be difficult, e.g. when shopping online, how can you send your credit card number to a site you're visiting for the first time? For this a different setting may be needed called: the asymmetric-key setting or public-key setting.


### Asymmetric-key Setting

![](module1-0005.png)

In this setting, only the receiver will hold the secret key \\(skR\\), and everyone will have the public key \\(pkR\\) of the receiver. Most importantly, though, so will the sender. How everyone gets the public key we will learn later on, but for now let's assume there's a shared trusted directory which lists all party's names and public keys together for anyone to look up.

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


In this course, we'll study how a lot of crypto-algorithms operate. We will use the terms "protocol" and "scheme" interchangeably. How do we determine how *good* the scheme is? There are two approaches:

* Trial and error
* Provable security

![](module1-0008.png)

### Trial and error

In the trial and error approach, you design a scheme that you and your colleagues feel is secure. You try to attack it, if you do find a vulnerability, you fix it, but if you can't you go ahead and use it. If you an attack is found in the future, it may be a bit more expensive to fix, there may be some bad press, but you still fix it and deploy it. This cycle continues until there are no more attacks, and at that point you consider the scheme stable. Does that mean the scheme is secure? We don't really know, but we hope it is. An alternative to this is the provable security approach.

### Provable Security

This was designed in the 1980's, and with this approach, it's possible to prove that a scheme is secure. It provides security guarantees. It's a proof by contradiction. You posit that if an attack is found then a *hard problem* will have to be solved, like factoring primes. This approach requires a good definition what you mean by "secure" means and that is what we will explore in this course. We will learn this approach in depth later on, studying definitions for various cryptographic goals and discover that this can also be used to prove insecurities. That will lead us to find attacks more easily.

## L6: Symmetric Encryption 

For now, let's learn the syntax of a symmetric encryption scheme. All symmetric encryption schemes consist of:

* **The message space:** \\(MsgSp\\) — This describes the limit of the message we can encrypt. For many schemes, this is a set of all bit strings, or a bit string of a specific length, e.g. 120 bits
* **The key generation algorithm:** \\(K\\), this describes how it is that we determine the key for encryption. However, in many algorithms, the key may be as simple as a random number from a key space \\(KeySp\\).
* **The encryption algorithm:** \\(\varepsilon\\)
* **The decryption algorithm:** \\(\mathcal{D}\\)

## L7: Key Generation Algorithm 

![](module1-0011.png)

Usually the key generation algorithm \\(K\\) is a randomized algorithm, so we feed it some randomness, and the output is a secret key \\({\color{Red} K}\\). This secret key is then shared by both the sender and the receiver.

We're only discussing the inputs and outputs of the algorithms for now, and so:

* **To encrypt a message** — the sender will run the encryption algorithm \\(\varepsilon\\) that produces a cipher text \\(C\\). The inputs for \\(\varepsilon\\) are:
    * The shared secret key \\({\color{Red} K}\\)
    * The message \\(M\\) such that \\(M ∈ MsgSp\\)
    * One last input to ensure that the same \\({\color{Red} K}\\), and \\(M\\) don't map to the same output. This may be:
        * Randomness
        * Any other state, such as a counter
* **To decrypt the message** — the receiver runs the decryption algorithm \\(\mathcal{D}\\)  which is deterministic, and will always produces the same output \\(M\\) for the same ciphertext \\(C\\). It takes as inputs:
    * The shared secret key \\({\color{Red} K}\\)
    * The ciphertext \\(C\\).

For an encryption scheme to be useful, it should ensure that for combination of valid messages, and keys, we should be able to encrypt and decrypt the original message back. Or more formally, for all \\(M \in MsgSp\\) and \\({\color{Red} K} ∈ KeySp\\), ensure that \\(\mathcal{D} ({\color{Red} K},\varepsilon({\color{Red} K},M)) = M\\).

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

The first formal definition of security was proposed by Claude Shannon, known as the "father of information theory". An encryption scheme is perfectly secure (or Shannon secure) if for a ciphertext \\(C\\) and messages \\(M_1\\) and \\(M_2\\), the probability of the either message \\(M_1\\) or \\(M_2\\) being encrypted to \\(C\\) is equal.

That is to say, for any given ciphertext it is just as likely that it is one message, than it is another message if the key is unknown. This captures the intuition that ciphertexts leaks no information, since it is impossible to ascertain the contents of the message. More formally, though, this is represented as:

$$Pr[\varepsilon({\color{Red} K},M_1 ) = C] = Pr[\varepsilon(,M_2 ) = C]$$

## L10: Theorem and Proof 

Let's prove that the OneTimePad is Perfectly/Shannon Secure.

![](module1-0015.png)

Recall, that we need prove that it is likely that any ciphertext can be any message for a variable key. Let's work through this step-by-step

1. Let's select a ciphertext specific cyphertext \\(C\\), and a specific message \\(M\\). For the OneTimePad we know that \\(C \in \\{0,1\\}^n\\) and \\(M\in \\{0,1\\}^n\\)
1. Let's now determine the probability that a randomly selected key \\({\color{Red} K}\\) will successfully encrypt the message into the cyphertext, i.e  \\(Pr[\varepsilon({\color{Red} K},M_1 ) = C]\\):
    1. Due to the way the OneTimePad scheme operates, we know that the probability of the key encrypting, is the same as the probability of the same key decrypting and so \\(Pr[\varepsilon({\color{Red} K},M_1 ) = C] = Pr[{\color{Red} K} = M \oplus C] \\)
    1. To evaluate the probability that a randomly selected key will successfully decrypt the ciphertext, we recall that \\({\color{Red} K} \in \\{0,1\\}^n\\)
    1. And so, since there are \\(2^n\\) possible keys, we can determine that \\(Pr[{\color{Red} K} = M \oplus C] = \frac{1}{2^n} \\)
1. With this, we have proven that for any ciphertext \\(C\\) and messages \\(M_1\\) and \\(M_2\\), the probability of the either message \\(M_1\\) or \\(M_2\\) being encrypted to \\(C\\) using an unknown key \\({\color{Red} K}\\) is equal.

The limitation of the OneTimePad is that it requires a secret key of a length just as long as the message. In practice we want to share a short secret key with long messages.

## L11: Shannon Theorem

![](module1-0016.png)

The Shannon Theorem says that the OneTimePad's limitation of a secret key just as long as the message is unavoidable for perfect secrecy. Let's prove this:

1. Let's assume a keyspace smaller than the message space, i.e \\(KeySp < MsgSp\\)
1. Let's select a specific message \\(M_1\\), and a secret key \\({\color{Red} K_1}\\) and compute the ciphertext specific cyphertext \\(C = \varepsilon({\color{Red} K}, M_1)\\).
1. Thus we, know the probability that some key encrypts \\(M_1\\) to \\(C_1\\) is non-zero, because we know that \\({\color{Red} K_1}\\) exists, i.e. \\(Pr[\varepsilon({\color{Red} K},M_1 ) = C] > 0\\).
1. Now, since we know the that the keyspace is smaller than the message space, let's assume that there exists some message \\(M_2\\) that cannot be encrypted to the same ciphertext \\(C\\), i.e. \\(Pr[\varepsilon({\color{Red} K},M_2 ) = C] = 0\\)
1. With last two statements combined, we realize that for we can no longer map any two messages onto the same ciphertext, i.e \\(Pr[\varepsilon({\color{Red} K},M_1 ) = C] \neq Pr[\varepsilon({\color{Red} K},M_2 ) = C]\\)
1. And so we have proven that if the keyspace is smaller than the message space, we will not have perfect secrecy.

This implies that the keyspace should at least as large as the message space.

## L12: Lesson 1 Summary 

![](module1-0017.png)

We can't do better than the OneTimePad. That's a fact. However, it's still okay, we don't really need perfect secrecy in practice. This is the end for "information-theory" cryptography. However, "computational-complexity" cryptography opens up many more avenues.

If we relax the security requirement, we should still be fine. Some of the some assumptions include:
* Our adversaries are computationally bounded
* Our adversaries will only spend a reasonable amount of time attacking out system, e.g. a hundred or even a million years


Additionally, we have the following assumptions:
* There are "hard" problems that we can build upon, e.g. factoring large numbers. Cryptography these kinds of problems.
* Secret keys are kept secret: no viruses, compromised computers, etc. Later on we'll discuss how cryptography can help with this as well.
* Algorithms themselves are public (Kerckhoff's principle)

With all this in place, we can begin exploring "computational-complexity" cryptography which opens a lot more possibilities.

