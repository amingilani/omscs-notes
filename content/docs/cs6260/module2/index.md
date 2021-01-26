---
title: "Module 2"
description: ""
lead: ""
date: 2021-01-25T02:43:59-05:00
lastmod: 2021-01-25T02:43:59-05:00
draft: false
images: []
menu:
  docs:
    parent: "cs6260"
weight: 100
toc: true
---
## Introduction

In this module, we will:

* Continue studying symmetrical encryption schemes
* See examples of these symmetrical encryption schemes
* See an important definition of security as it relates to encryption schemes
* We will use this definition to analyze the security of the constructions we study

## L1

Our _goal_ for now is to study how to solve the problem of practical data confidentiality in the symmetric-key setting. To achieve this goal, the _tool_ we're use is a [cryptographic primitive](https://en.wikipedia.org/wiki/Cryptographic_primitive) called the symmetric encryption scheme. Whenever we study a new crypto primitive for some crypto goal, we'll start with the definition of the primitive, or the syntax:

* The composition
* The inputs and outputs
* The correctness requirement

After these, we'll discuss the definition of security for the crypto goal in question. Afterwards, we'll see examples of schemes. And finally discuss security of the schemes with respect to the definition of security.

### Symmetric Encryption Scheme

{{< img src="module1-0001.png" alt="Symmetric-key Encryption Scheme Illustrated" class="border-0" >}}

Recall the syntax of a symmetric encryption scheme. Any symmetric encryption scheme is specified the key generation algorithm, the encryption algorithm, and the decryption algorithm, i.e. $\mathcal{SE}(\mathcal{K}, \mathcal{E}, \mathcal{D})$ or, usually, if the key generation algorithm is simply about picking a random number from a key space, $\mathcal{SE}(\mathcal{KeySp}, \mathcal{E}, \mathcal{D})$. In more detail:

* **The key generation algorithm $\mathcal{K}$ or $\mathcal{KeySp}$**: This specifies how the symmetric key ${\color{Red} K}$ is generated, which will be shared between the sender $\mathcal{S}$  and receiver $\mathcal{R}$. This is usually a randomized algorithm that picks a random ${\color{Red} K} \in \mathcal{KeySp}$ where $\mathcal{KeySp}$ is a set containing all valid keys.
* **The encryption algorithm $\mathcal{E}$**: This is a randomized algorithm that takes the inputs ${\color{Red}} K$ and a message $M \in \mathcal{MsgSp}$ where $\mathcal{MsgSp}$ is a set containing all valid messages and outputs a ciphertext $C$. It is randomizes so as not to map the same inputs to the same ciphertext. i.e. $\mathcal{E}({\color{Red} K}, M) \rightarrow C$. This ciphertext $C$ is transmitted over a potentially insecure channel and received by the receiver $\mathcal{R}$ that runs the decryption algorithm.
* **The decryption algorithm $\mathcal{D}$**: This is a [deterministic](https://en.wikipedia.org/wiki/Deterministic_algorithm) algorithm that takes as inputs the ciphertext $C$ and key ${\color{Red} K}$ to return the original message $M$, i.e. $\mathcal{D}({\color{Red} K}, C) \rightarrow M$.

For the scheme to be useful, it should be *correct* and for every valid message and every valid key, we should be able to encrypt and decrypt back the same message. I.e. $\mathcal{D}({\color{Red} K}, \mathcal{E}({\color{Red} K}, M)) = M$ for every $M \in \mathcal{MsgSp}$ and ${\color{Red} K} \in \mathcal{KeySp}$.

This is just a primitive of a symmetric encryption scheme and does not say anything about security or a specific construction.

## L2

{{< img src="module1-0002.png" alt="Symmetric-key Encryption Scheme more details" class="border-0" >}}

Recall that a common **key generation algorithm** is to pick a random string from a key space ${\color{Red} K} \in \mathcal{KeySp}$. A very common key space is a set of fixed-length bit strings, of some length $k$ i.e. 
$\\{0, 1\\}^k$. In such a case, the key generation function is specified by the key space, for the encryption scheme and it is assumed that the key is picked at random, i.e. $\mathcal{SE}(\mathcal{KeySp}, \mathcal{E}, \mathcal{D})$.

Recall that an **encryption algorithms** must not be deterministic, and thus should produce a different output for the same inputs. This can be accomplished by making them:

* Randomized: Take a random string as input every time.
* Stateful: The sender keeps a changing state, e.g. a counter variable, and every time they use that counter, they increment the value so that the next time they use it, it's different.

We will see examples of stateful encryption schemes soon.

## L3: Blockciphers

{{< img src="module1-0003.png" alt="Blockcipher illustrated" class="border-0" >}}

A blockcipher is usually written as two words: block cipher, but the lectures use one word. The [Intro to Information Security](http://omscs.gatech.edu/cs-6035-introduction-to-information-security) course teaches us what blockciphers are, but knowing them isn't required. DES, 3DES, and AES are specific blockciphers.

In general, a blockcipher is a tool to encrypt short strings. e.g. as short as 120 or 256 bits. Formally, a blockcipher is a function family $E : \\{0,1\\}^k \times \\{0,1\\}^n \rightarrow \\\{0,1\\}^n$, i.e. It usually takes the following inputs and returns the following output:

* Inputs:
    * a key which comes from a set of bit strings of length $k$, i.e. ${\color{Red} K} \in \\{0, 1\\}^k$.
    * an input which comes from a set of bit strings of a short length $n$, i.e. $M \in \\{0, 1\\}^n$.
* Outputs a bit string also of a short length $n$ i.e. $C \in \\{0, 1\\}^n$.

There is an **alternate notation** we may use for the function that is $E_{\color{Red} K}$, and it is equivalent to the full form $E({\color{Red} K}, M)$ for every key ${\color{Red} K} \in \\{0, 1\\}^k$. This isn't necessary, but we will use it for other encryption schemes too.

A key property of blockcipher is that **it is deterministic**, there is a one-to-one mapping:

* For the same key ${\color{Red} K_i} \in \\{0,1\\}^k$, the blockcipher will map the message onto the same cipher text, i.e. $E_{\color{Red} K_i}(M_i) \rightarrow C_i$ where $i \in \\{0, \ldots, k\\}$
* For every $C \in \\{0, 1\\}^n$, there is only a single $M \in \\{0, 1\\}^n$, such that $C = E_{\color{Red} K}(M)$

Finally, each blockcipher has an **inverse** that can be denoted $E_{\color{Red} K}^{-1}$, and it works much like you would imagine. For all valid messages $M \in \\{0, 1\\}^n$, ciphertexts $C \in \\{0, 1\\}^n$, and keys ${\color{Red} K} \in \\{0, 1\\}^k$:

* You can decrypt and encrypt a ciphertext and it'll stay unmodified, i.e. $E_{\color{Red} K}(E_{\color{Red} K}^{-1}(C)) = C$
* You can encrypt and decrypt a message and it'll stay unmodified, i.e. $E_{\color{Red} K}^{-1}(E_{\color{Red} K}(M)) = M$
