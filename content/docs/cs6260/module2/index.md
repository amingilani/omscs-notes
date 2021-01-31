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
    1. For each $n$-bit string we apply the underlying blockcipher encryption algorithm $E_{\color{Red} K}$ and concatenate them, i.e.$C \leftarrow (E_{\color{Red} K}(M_1) \rightarrow C_1 \mathbin\Vert E_{\color{Red} K}(M_2) \rightarrow C_2 \mathbin\Vert \ldots \mathbin\Vert  E_{\color{Red} K}(M_m) \rightarrow C_m)$
* **The decryption algoritm** $\mathcal{D}$: Here's how we decrypt a ciphertext $C$ to a message $M$
    1.  We break apart the message $C$ into $n$-bit strings, i.e. $C \leftarrow (C_1 \mathbin\Vert C_2 \mathbin\Vert \ldots \mathbin\Vert  C_m)$
    1. For each $n$-bit string we apply the underlying blockcipher decryption algorithm $E_{\color{Red} K}^{-1}$ and concatenate them, i.e.$M \leftarrow (E_{\color{Red} K}^{-1}(C_1) \rightarrow M_1  \mathbin\Vert E_{\color{Red} K}^{-1}(C_2) \rightarrow M_2  \mathbin\Vert \ldots \mathbin\Vert  E_{\color{Red} K}^{-1}(C_m) \rightarrow M_m )$

For the pseudocode of this encryption scheme, refer to the lecture notes[^bellare-rogaway]

**Is this scheme correct?** Yes. By the properties of the blockcipher, this encryption scheme is correct, and so if you encrypt any message, and decrypt it, you will get the original message, i.e. $\mathcal{D}(\mathcal{E}({\color{Red} K},M)) = M$

**Is this scheme secure?** ECB can't be Shannon Secure: One short key is being used to encrypt multiple chunks of long messages. While we haven't studied anything but Shannon Security yet, we can use intuition at this point: even if you don't know the keys or the message, and even thought the underlying blockchiper is secure, the ECB's weakness is that if some chunks of messages, are repeated then by the properties of the blockcipher, the ciphertext of that chunk will also be the same. i.e.if $M_a = M_b$ then $C_a = C_b$. That is some leakage of information. How bad that is in practice is up to the application of the cipher, but as cryptogrophers we **don't want any leakage of information**. We'll analyze this scheme formally later.

[^bellare-rogaway]: Lecture notes: [_Introduction to Modern Cryptography_](https://www.cc.gatech.edu/~aboldyre/teaching/LectureNotes.pdf) by Mihir Bellare and Phillip Rogaway

## L6: Encrypting an Image with EBC

{{< img src="module2-0006.png" alt="ECB leakage illustrated with an image" class="border-0" >}}

This is an illustration of leakage when the same results in the same ciphertext. Even though the image is not the same, we can still tell what the image is by looking at the result.

## L7: Cipher-block Chaining: CBC mode

{{< img src="module2-0007.png" alt="CBC function illustrated" class="border-0" >}}

Let's say we have a blockcipher $E : \\{0,1\\}^k \times \\{0,1\\}^n \rightarrow \\\{0,1\\}^n$, then the CBC is simply defined using a set of functions $CBC = \\{0,1\\}^k, \mathcal{E}, \mathcal{D})$. Let's explore these:

* **The keyspace** $\\{0,1\\}^k$: is exactly the keyspace of the underlying blockcipher, and from here we select a key ${\color{Red} K}$.
* **The encryption algoritm** $\mathcal{E}$: Here's how we encrypt a message $M$ to a ciphertext $C$
  1. We pick a random $n$-bit string and take it as the initialization vector $IV$, i.e.$IV \xleftarrow{$} \\{0,1\\}^n$, _note the new syntax for a random pick._
  1. We break apart the message $M$ into $n$-bit strings into message blocks, i.e. $M \leftarrow (M_1 \mathbin\Vert M_2 \mathbin\Vert \ldots \mathbin\Vert  M_m)$
  1. For the first message block $M_1$, we XOR it with the $IV$, apply the underlying blockcipher encryption algorithm $E_{\color{Red} K}$ and get the first cipher block $C_1$, i.e.$C_1 \leftarrow E_{\color{Red} K}(M_1 \oplus IV)$
  1. For each subsequent message block string we XOR them with the previous cipher block and apply the underlying blockcipher encryption algorithm $E_{\color{Red} K}$ to get their corresponding cipher block, i.e.$C_i \leftarrow E_{\color{Red} K}(M_i \oplus C_{i-1})$
  1. Finally, we concatenate the initialization vector with the ciphertexts of the messages in sequential order to produce the final ciphertext, i.e.$C \leftarrow (IV \mathbin\Vert C_1 \mathbin\Vert C_2 \mathbin\Vert \ldots \mathbin\Vert  C_m)$
* **The decryption algoritm** $\mathcal{D}$: Here's how we decrypt a ciphertext $C$ to a message $M$
  1.  We break apart the message $C$ into $n$-bit strings, and we take the first string as the initialization vector $IV$ i.e. $C \leftarrow ( IV \mathbin\Vert C_1 \mathbin\Vert C_2 \mathbin\Vert \ldots \mathbin\Vert  C_m)$
  1. For the first cipher block $M_1$, we decrypt it with the underlying blockcipher decryption algorithm $E_{\color{Red} K}^{-1}$ and XOR the result with the initialization vector to get original $M_1$, i.e.$M_1 \leftarrow E_{\color{Red} K}^{-1}(C_1) \oplus IV$
  1. For each subsequent message cipher block we apply the underlying blockcipher decryption algorithm $E_{\color{Red} K}^{-1}$ and XOR the result with the previous cipher block to get their corresponding message block, i.e.$M_i \leftarrow E_{\color{Red} K}^{-1}(C_i) \oplus C_{i-1}$
  1. Finally, we concatenate the message blocks in sequential order to produce the message, i.e. $M \leftarrow (M_1 \mathbin\Vert M_2 \mathbin\Vert \ldots \mathbin\Vert  M_m)$


**Is this scheme correct?** Yes, due to the properties of the blockcipher, and the XOR operation, you can be sure the message you send will be the message you get back, i.e. $\mathcal{D}(\mathcal{E}({\color{Red} K},M)) = M$

**Is this scheme secure?** Unlike the ECB, the same message block will no longer give the same ciphertext because of the random initalization vector and the chained XOR operation. A new $IV$ should be chosen_ at random for every encryption operation but there are no obvious flaws here. We'll revisit the security of this scheme at a later point. That said, it still can't be Shannon secure because we're using a single key short to encrypt multiple message blocks.



## L8: Stateful Cipher-block Chaining mode

{{< img src="module2-0008.png" alt="Stateful CBC function illustrated" class="border-0" >}}

This scheme is very similar to the previous CBC scheme, the only difference is that instead of an initialization vector we'll use a sequential counter $ctr \in {0,1}^n$.

**The scheme** is based on a blockcipher $E : \\{0,1\\}^k \times \\{0,1\\}^n \rightarrow \\\{0,1\\}^n$. With this, we can then define the CBC as a set of functions $CBC = \\{0,1\\}^k, \mathcal{E}, \mathcal{D})$. Let's explore these:

* **The keyspace** $\\{0,1\\}^k$: is exactly the keyspace of the underlying blockcipher, and from here we select a key ${\color{Red} K}$.
* **The encryption algoritm** $\mathcal{E}$: Here's how we encrypt a message $M$ to a ciphertext $C$
  1. We set a counter $ctr$ which is initialized as a string of all zeroes, and increments by 1 on every encryption use and wraps around starting from 0 if it gets too big for the counter space. i.e. $ctr_i = ctr_{i-1} + (0^n-1 \mathbin\Vert  1)$ and $ctr_0 = \\{0\\}^n$ where $i$ is number of times the encryption function has been run, and the $+$ operation is performed modulus $n$.
  1. We break apart the message $M$ into $n$-bit strings into message blocks, i.e. $M \leftarrow (M_1 \mathbin\Vert M_2 \mathbin\Vert \ldots \mathbin\Vert  M_m)$
  1. For the first message block $M_1$, we XOR it with the $ctr$, apply the underlying blockcipher encryption algorithm $E_{\color{Red} K}$ and get the first cipher block $C_1$, i.e.$C_1 \leftarrow E_{\color{Red} K}(M_1 \oplus ctr)$
  1. For each subsequent message block string we XOR them with the previous cipher block and apply the underlying blockcipher encryption algorithm $E_{\color{Red} K}$ to get their corresponding cipher block, i.e.$C_i \leftarrow E_{\color{Red} K}(M_i \oplus C_{i-1})$
  1. Finally, we concatenate the counter with the ciphertexts of the messages in sequential order to produce the final ciphertext, i.e.$C \leftarrow (ctr \mathbin\Vert C_1 \mathbin\Vert C_2 \mathbin\Vert \ldots \mathbin\Vert  C_m)$
* **The decryption algoritm** $\mathcal{D}$: Here's how we decrypt a ciphertext $C$ to a message $M$
  1.  We break apart the message $C$ into $n$-bit strings, and we take the first string as the counter $ctr$ i.e. $C \leftarrow ( ctr \mathbin\Vert C_1 \mathbin\Vert C_2 \mathbin\Vert \ldots \mathbin\Vert  C_m)$
  1. For the first cipher block $M_1$, we decrypt it with the underlying blockcipher decryption algorithm $E_{\color{Red} K}^{-1}$ and XOR the result with the counter to get original $M_1$, i.e.$M_1 \leftarrow E_{\color{Red} K}^{-1}(C_1) \oplus ctr$
  1. For each subsequent message cipher block we apply the underlying blockcipher decryption algorithm $E_{\color{Red} K}^{-1}$ and XOR the result with the previous cipher block to get their corresponding message block, i.e.$M_i \leftarrow E_{\color{Red} K}^{-1}(C_i) \oplus C_{i-1}$
  1. Finally, we concatenate the message blocks in sequential order to produce the message, i.e. $M \leftarrow (M_1 \mathbin\Vert M_2 \mathbin\Vert \ldots \mathbin\Vert  M_m)$


**Is this scheme correct?** Yes, due to the properties of the blockcipher, and the XOR operation, you can be sure the message you send will be the message you get back, i.e. $\mathcal{D}(\mathcal{E}({\color{Red} K},M)) = M$

**Is this scheme secure?** The attacker can learn and predict the counter since it's part of the ciphertext, but it may not lead to compromise of the data or the secret key. There may be other weaknesses that we will analyze later for this scheme, but for now let's move on.

## L9: Randomized Counter Mode (CTR$)

{{< img src="module2-0009.png" alt="Stateful CBC function illustrated" class="border-0" >}}

This scheme is a bit different, there is no chaining, it's called a counter mode. This is a randomized version, and sometimes it's called the XOR-mode. Unlike CBC, and like EBC, this is parallelizable, and each block is encrypted independently. 

**The primitive of the scheme** in practice is almost always a blockcipher, but doesn't _have_ to be. And so we use a new function family and notation $F : \\{0,1\\}^k \times \\{0,1\\}^l \rightarrow \\\{0,1\\}^L$. This function:
* Has a key of the same length as we've been using before
* Does *not* need the length of the input to match the length of the output
* Does *not* need to be reversible, and therefore is not a permutation

With this, we can then define CTR\\$ as a set of functions $CTR\\$ = \\{0,1\\}^k, \mathcal{E}, \mathcal{D})$. Let's explore these:

* **The keyspace** $\\{0,1\\}^k$: is exactly the keyspace of the underlying function $F$ which may be a blockchiper, and from here we select a key ${\color{Red} K}$.
* **The encryption algoritm** $\mathcal{E}$: Here's how we encrypt a message $M$ to a ciphertext $C$
  1. We pick a random $l$-bit string $R \xleftarrow{$} \\{0,1\\}^n$
  1. We break apart the message $M$ into $n$-bit strings into message blocks, i.e. $M \leftarrow (M_1 \mathbin\Vert M_2 \mathbin\Vert \ldots \mathbin\Vert  M_m)$
  1. For each message block string we XOR it with the $F_{\color{Red} K}$ of $R$ incremented by the index of the message, i.e. $C_i \leftarrow F_{\color{Red} K}(R + i) \oplus M_i$, e.g. To calculate the first cipher block from the first message block, we do $C_1 \leftarrow F_{\color{Red} K}(R + 1) \oplus M_1$
  1. Finally, we concatenate $R$ and the ciper blocks sequentially to form the ciphertext i.e.$C \leftarrow (R \mathbin\Vert C_1 \mathbin\Vert C_2 \mathbin\Vert \ldots \mathbin\Vert  C_m)$
* **The decryption algoritm** $\mathcal{D}$: Here's how we decrypt a ciphertext $C$ to a message $M$
  1.  We break apart the message $C$ into $n$-bit strings, and we take the first string as the $R$ i.e. $C \leftarrow ( R \mathbin\Vert C_1 \mathbin\Vert C_2 \mathbin\Vert \ldots \mathbin\Vert  C_m)$
  1. For each cipher block string we XOR it with the $F_{\color{Red} K}$ of $R$ incremented by the index of the message, i.e. $M_i \leftarrow F_{\color{Red} K}(R + i) \oplus C_i$, e.g. To calculate the first message block from the first cipher block, we do $M_1 \leftarrow F_{\color{Red} K}(R + 1) \oplus C_1$
  1. Finally, we concatenate the message blocks in sequential order to produce the message, i.e. $M \leftarrow (M_1 \mathbin\Vert M_2 \mathbin\Vert \ldots \mathbin\Vert  M_m)$

Note that a reverse of $F$ was never needed, hence this function does not need to be a blockcipher.


**Is this scheme correct?** Yes, due to $F$ being deterministic, and the properties of the XOR operation, you can be sure the message you send will be the message you get back, i.e. $\mathcal{D}(\mathcal{E}({\color{Red} K},M)) = M$

**Is this scheme secure?** We'll analyze this soon.


## L10: Stateful Counter Mode (CTRC)


{{< img src="module2-0010.png" alt="Stateful CBC function illustrated" class="border-0" >}}

Now let's see a stateful variant of this mode. The only difference here is that instead of $R$ we use a stateful counter $ctr$.

**The primitive of the scheme** in practice is almost always a blockcipher, but doesn't _have_ to be. And so we use a new function family and notation $F : \\{0,1\\}^k \times \\{0,1\\}^l \rightarrow \\\{0,1\\}^L$. This function:
* Has a key of the same length as we've been using before
* Does *not* need the length of the input to match the length of the output
* Does *not* need to be reversible, and therefore is not a permutation

With this, we can then define CTR\\$ as a set of functions $CTR\\$ = \\{0,1\\}^k, \mathcal{E}, \mathcal{D})$. Let's explore these:

* **The keyspace** $\\{0,1\\}^k$: is exactly the keyspace of the underlying function $F$ which may be a blockchiper, and from here we select a key ${\color{Red} K}$.
* **The encryption algoritm** $\mathcal{E}$: Here's how we encrypt a message $M$ to a ciphertext $C$
  1. We set a counter $ctr$ which is initialized as a string of all zeroes, and increments by 1 on every encryption use and wraps around starting from 0 if it gets too big for the counter space. i.e. $ctr_i = ctr_{i-1} + (0^n-1 \mathbin\Vert  1)$ and $ctr_0 = \\{0\\}^n$ where $i$ is number of times the encryption function has been run, and the $+$ operation is performed modulus $n$.
  1. We break apart the message $M$ into $n$-bit strings into message blocks, i.e. $M \leftarrow (M_1 \mathbin\Vert M_2 \mathbin\Vert \ldots \mathbin\Vert  M_m)$
  1. For each message block string we XOR it with the $F_{\color{Red} K}$ of $ctr$ incremented by the index of the message, i.e. $C_i \leftarrow F_{\color{Red} K}(R + i) \oplus M_i$, e.g. To calculate the first cipher block from the first message block, we do $C_1 \leftarrow F_{\color{Red} K}(R + 1) \oplus M_1$
  1. Finally, we concatenate $ctr$ and the ciper blocks sequentially to form the ciphertext i.e.$C \leftarrow (R \mathbin\Vert C_1 \mathbin\Vert C_2 \mathbin\Vert \ldots \mathbin\Vert  C_m)$
* **The decryption algoritm** $\mathcal{D}$: Here's how we decrypt a ciphertext $C$ to a message $M$
  1.  We break apart the message $C$ into $n$-bit strings, and we take the first string as the $ctr$ i.e. $C \leftarrow ( R \mathbin\Vert C_1 \mathbin\Vert C_2 \mathbin\Vert \ldots \mathbin\Vert  C_m)$
  1. For each cipher block string we XOR it with the $F_{\color{Red} K}$ of $ctr$ incremented by the index of the message, i.e. $M_i \leftarrow F_{\color{Red} K}(R + i) \oplus C_i$, e.g. To calculate the first message block from the first cipher block, we do $M_1 \leftarrow F_{\color{Red} K}(R + 1) \oplus C_1$
  1. Finally, we concatenate the message blocks in sequential order to produce the message, i.e. $M \leftarrow (M_1 \mathbin\Vert M_2 \mathbin\Vert \ldots \mathbin\Vert  M_m)$

Note that a reverse of $F$ was never needed, hence this function does not need to be a blockcipher. The receiver does not need to maintain the state, they can get it from the ciphertext, but the sender needs to maintain the state.


**Is this scheme correct?** Yes, due to $F$ being deterministic, and the properties of the XOR operation, you can be sure the message you send will be the message you get back, i.e. $\mathcal{D}(\mathcal{E}({\color{Red} K},M)) = M$

**Is this scheme secure?** We'll analyze this soon.