---
title: "Module 1"
description: "Doks is a Hugo theme helping you build modern documentation websites that are secure, fast, and SEO-ready — by default."
lead: "Doks is a Hugo theme helping you build modern documentation websites that are secure, fast, and SEO-ready — by default."
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

## L1: Course Objectives


Objectives:

+ We will study practical cryptographic schemes in "both settings", e.g.:
    + AES
    + HMAC
    + RSA-OAEP
    + RSA and DSA signatures
+ We will learn the more fundamental principles of:
    + What is a good scheme
    + How to estimate and compare schemes: this rarely done ins security courses but is very important in modern cryptography

## L2: Introduction


Cryptography means "secret writing"

Julius Caesar used Cryptography
Eniga machine was employed by Nazi Germany in World War 2
Modern day Cryptography was created in the past 40 years.
This course is an Introduction to modern cryptography.

Main goals are:

+ Data privacy: how to protect private data from eavesdroppers
+ Data authenticity: ensuring the origin of the data is what we think it is
+ Data integrity: ensuring that the data hasn't been changed since it was created

That said, these may not all be required.

We will also talk about other goals at the end of this course.

Most of us use cryptography behind the scenes every day without realizing. It's used all the times when we do every day tasks like shopping online, paying bills, using our cellphones, etc. We can even stream online and crypto will be used in the background. Half of the internet's traffic is encrypted nowadays, and this might increase in the future.

## L3: Players and Settings


Let's look at our main crypto players and settings. 

When two parties want to communicate securely with data privacy. They are often called Alice and Bob but we will call them _S_ (sender) and _R_ (receiver). A third, usually an eavesdropper named Eve, is called _A_ (attacker). The sender wants to wants to send messages to the receiver. For security to exist, there needs to be some secret information not known to the attacker. Otherwise _A_ can do anything _S_ and _R_ can do.

### Symmetric-key Setting

One of the settings in cryptography is called the symmetric-key setting. A secret key _K_ is shared by _S_ and _R_, but not _A_. The old cryptos: Caesar's cipher, enigma machine, substitution ciphers, they all operate in this symmetric key setting. There are better modern symmetric-key cryptographic schemes, and they have much better efficiency.

For this setting to work, the sender and receiver have to agree on a shared secret key before communicating. This can be difficult, e.g. when shopping online, how can you send your credit card number to a site you're visiting for the first time? For this a different setting may be needed called: the asymmetric-key setting or public-key setting.


### Asymmetric-key Setting

In this setting, only the receiver will hold the secret key _skR_, and everyone will have the public key _pkR_ of the receiver. Most importantly, though, so will the sender. How everyone gets the public key we will learn later on, but for now let's assume there's a shared trusted directory which lists all party's names and public keys together for anyone to look up.

This setting is much more difficult than the symmetric-key setting. In fact, all of public-key cryptography happened since the late 1970's. Even in the 1970's people didn't believe the asymmetric-key setting was possible. This setting means that two people can meet in public for the first time, without sharing a secret, and be able to communicate securely. The RSA cryptosystem is an example of a public-key cryptographic scheme.

## L4: Goals and Primitives


As discussed before, in this course, we'll explore the main goals of cryptography: 

+ data privacy
+ data authenticity, and
+ data integrity

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

### Trial and error

In the trial and error approach, you design a scheme that you and your colleagues feel is secure. You try to attack it, if you do find a vulnerability, you fix it, but if you can't you go ahead and use it. If you an attack is found in the future, it may be a bit more expensive to fix, there may be some bad press, but you still fix it and deploy it. This cycle continues until there are no more attacks, and at that point you consider the scheme stable. Does that mean the scheme is secure? We don't really know, but we hope it is. An alternative to this is the provable security approach.

### Provable Security

