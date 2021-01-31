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

{{< img src="module2-0001.png" alt="Symmetric-key Encryption Scheme Illustrated" class="border-0" >}}

Recall the syntax of a symmetric encryption scheme. Any symmetric encryption scheme is specified the key generation algorithm, the encryption algorithm, and the decryption algorithm, i.e. $\mathcal{SE}(\mathcal{K}, \mathcal{E}, \mathcal{D})$ or, usually, if the key generation algorithm is simply about picking a random number from a key space, $\mathcal{SE}(\mathcal{KeySp}, \mathcal{E}, \mathcal{D})$. In more detail:

* **The key generation algorithm $\mathcal{K}$ or $\mathcal{KeySp}$**: This specifies how the symmetric key ${\color{Red} K}$ is generated, which will be shared between the sender $\mathcal{S}$  and receiver $\mathcal{R}$. This is usually a randomized algorithm that picks a random ${\color{Red} K} \in \mathcal{KeySp}$ where $\mathcal{KeySp}$ is a set containing all valid keys.
* **The encryption algorithm $\mathcal{E}$**: This is a randomized algorithm that takes the inputs ${\color{Red}} K$ and a message $M \in \mathcal{MsgSp}$ where $\mathcal{MsgSp}$ is a set containing all valid messages and outputs a ciphertext $C$. It is randomizes so as not to map the same inputs to the same ciphertext. i.e. $\mathcal{E}({\color{Red} K}, M) \rightarrow C$. This ciphertext $C$ is transmitted over a potentially insecure channel and received by the receiver $\mathcal{R}$ that runs the decryption algorithm.
* **The decryption algorithm $\mathcal{D}$**: This is a [deterministic](https://en.wikipedia.org/wiki/Deterministic_algorithm) algorithm that takes as inputs the ciphertext $C$ and key ${\color{Red} K}$ to return the original message $M$, i.e. $\mathcal{D}({\color{Red} K}, C) \rightarrow M$.

For the scheme to be useful, it should be *correct* and for every valid message and every valid key, we should be able to encrypt and decrypt back the same message. I.e. $\mathcal{D}({\color{Red} K}, \mathcal{E}({\color{Red} K}, M)) = M$ for every $M \in \mathcal{MsgSp}$ and ${\color{Red} K} \in \mathcal{KeySp}$.

This is just a primitive of a symmetric encryption scheme and does not say anything about security or a specific construction.

## L2

{{< img src="module2-0002.png" alt="Symmetric-key Encryption Scheme more details" class="border-0" >}}

Recall that a common **key generation algorithm** is to pick a random string from a key space ${\color{Red} K} \in \mathcal{KeySp}$. A very common key space is a set of fixed-length bit strings, of some length $k$ i.e. 
$\\{0, 1\\}^k$. In such a case, the key generation function is specified by the key space, for the encryption scheme and it is assumed that the key is picked at random, i.e. $\mathcal{SE}(\mathcal{KeySp}, \mathcal{E}, \mathcal{D})$.

Recall that an **encryption algorithms** must not be deterministic, and thus should produce a different output for the same inputs. This can be accomplished by making them:

* Randomized: Take a random string as input every time.
* Stateful: The sender keeps a changing state, e.g. a counter variable, and every time they use that counter, they increment the value so that the next time they use it, it's different.

We will see examples of stateful encryption schemes soon.

## L3: Blockciphers

{{< img src="module2-0003.png" alt="Blockcipher illustrated" class="border-0" >}}

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

## L4: Blockcipher modes of operation 

For now we assume that we have a _good_ blockcipher. We'll discuss what a "good" blockcipher is exactly in the future. Blockciphers encrypt short strings but, in practice, we want to encrypt long strings. The most obvious way is to break our message into blocks and encrypt block-by-block, using the blockcipher for each block. For simplicity, let's assume the message space consists of messages whose length is a multiple of the block length, but even if this isn't true, we can pad the message.

## L5: Electronic Code Book mode

{{< img src="module2-0005.png" alt="Blockcipher illustrated" class="border-0" >}}

The method we discussed in the previous lecture will be discussed here in more detail. It is indeed the most obvious way, and it's called the Electronic Code Book. Let's say we have a blockcipher $E : \\{0,1\\}^k \times \\{0,1\\}^n \rightarrow \\\{0,1\\}^n$, then the electronic code book is simply defined using a set of functions $ECB = \\{0,1\\}^k, \mathcal{E}, \mathcal{D})$. Let's explore these:

 * **The keyspace** $\\{0,1\\}^k$: is exactly the keyspace of the underlying blockcipher, and from here we select a key ${\color{Red} K}$.
 * **The encryption algoritm** $\mathcal{E}$: Here's how we encrypt a message $M$ to a ciphertext $C$
    1. We break apart the message $M$ into $n$-bit strings, i.e. $M \leftarrow (M_1 \mathbin\Vert M_2 \mathbin\Vert \ldots \mathbin\Vert  M_m)$
    1. For each $n$-bit string we apply the underlying blockcipher encryption algorithm $E_{\color{Red} K}$ and concatenate them, i.e $C \leftarrow (E_{\color{Red} K}(M_1) \rightarrow C_1 \mathbin\Vert E_{\color{Red} K}(M_2) \rightarrow C_2 \mathbin\Vert \ldots \mathbin\Vert  E_{\color{Red} K}(M_m) \rightarrow C_m)$
* **The decryption algoritm** $\mathcal{D}$: Here's how we decrypt a ciphertext $C$ to a message $M$
    1.  We break apart the message $C$ into $n$-bit strings, i.e. $C \leftarrow (C_1 \mathbin\Vert C_2 \mathbin\Vert \ldots \mathbin\Vert  C_m)$
    1. For each $n$-bit string we apply the underlying blockcipher decryption algorithm $E_{\color{Red} K}^{-1}$ and concatenate them, i.e $M \leftarrow (E_{\color{Red} K}^{-1}(C_1) \rightarrow M_1  \mathbin\Vert E_{\color{Red} K}^{-1}(C_2) \rightarrow M_2  \mathbin\Vert \ldots \mathbin\Vert  E_{\color{Red} K}^{-1}(C_m) \rightarrow M_m )$

By the properties of the blockcipher, this encryption scheme is correct, and so if you encrypt any message, and decrypt it, you will get the original message, i.e. $\mathcal{D}(\mathcal{E}({\color{Red} K},M)) = M$

For the pseudocode of this encryption scheme, refer to the lecture notes[^bellare-rogaway]

ECB can't be Shannon Secure: One short key is being used to encrypt multiple chunks of long messages. While we haven't studied anything but Shannon Security yet, we can use intuition at this point: even if you don't know the keys or the message, and even thought the underlying blockchiper is secure, the ECB's weakness is that if some chunks of messages, are repeated then by the properties of the blockcipher, the ciphertext of that chunk will also be the same. i.e if $M_a = M_b$ then $C_a = C_b$. That is some leakage of information. How bad that is in practice is up to the application of the cipher, but as cryptogrophers we **don't want any leakage of information**. We'll analyze this scheme formally later.

[^bellare-rogaway]: Lecture notes: [_Introduction to Modern Cryptography_](https://www.cc.gatech.edu/~aboldyre/teaching/LectureNotes.pdf) by Mihir Bellare and Phillip Rogaway

## L6: Encrypting an Image with EBC

{{< img src="module2-0006.png" alt="ECB leakage illustrated with an image" class="border-0" >}}

This is an illustration of leakage when the same results in the same ciphertext. Even though the image is not the same, we can still tell what the image is by looking at the result.