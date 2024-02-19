+++
title = 'Symmetric Encryption 101'
date = 2022-08-09T00:00:00-00:00
draft = false
+++

A few weeks ago, we covered the very basics of cybersecurity; confidentiality, integrity, and availability. Now we're going to put them to use with with some symmetric encryption!

# Why do we need encryption?

Among many reasons:
* Encryption prevents unauthorized access to data
* We may need encryption to meet regulatory requirements
* We use encryption as a means of defense in depth

To anybody that is defending themself from attack, things have become a lot more complex and therefore much, much harder to secure. For an attacker, it has likely never been easier to steal a company's data or cause utter chaos and financial ruin. Thanks to the remote and decentralized nature of companies in the modern age, most of the exfiltration now occurs from the comfort of the attacker's couch. You can safely assume that at some point you and/or your data will be breached. If you have any control over this data whatsoever, your best line of defense against that breach is encryption.

# What is it?

Encryption means making readable data unreadable. Decryption means making unreadable data readable. That's it! It's similar to another concept, hashing. The biggest difference is, encrypted information is intended to be decrypted, and hashed information is not. Depending on how you look at it, you could see encryption as a preventative, deterrent, technical, administrative, or even regulatory control.

There are two main types of encryption: symmetric and asymmetric. This article is meant to highlight and demonstrate symmetric encryption, though I will briefly touch on asymmetric. Neither are inherently better or worse than the other; it is simply a matter of which one should be used in which situation. In some cases, such as TLS, both are used in tandem. 

Let's define a few more terms and jump in:
* *plaintext* - this is the text that you would like to encrypt
* *encryption key* - this is what is used to encrypt your plaintext
* *ciphertext* - this is the text that you get once you encrypt plaintext with an encryption key

### Symmetric

Symmetric encryption is simple; whoever has the encryption key can both encrypt and decrypt a message. This key is typically shared among only the people or systems who need to access some data or communicate with each other. When you hear "encryption at rest", it is synonymous with symmetric encryption. 

Pros:
* Symmetric encryption is much faster than asymmetric
* It can be used for large datasets 

Cons:
* All parties who need access to the information need to share the key beforehand
* Key management can be difficult, and keys may need to be rotated when any party should no longer have access

Used for:
* Encrypting hard disks on devices and servers
* Field-level encryption

[blurb about generating ciphertext]

### Asymmetric
Asymmetric encryption is a little more difficult to understand, and I will cover it in-depth later. The math behind it is very cool but much more complex. Asymmetric encryption rose from the question: "How do I send someone an encrypted message that they can read, without them giving me the key that they use to decrypt that message?" It addresses a fundamental flaw of symmetric encryption; that a single key that is shared among multiple parties is vulnerable to exposure, which can easily defeat the purpose of encryption.

Pros:
* Fewer keys needed to encrypt messages between systems and people
* Two people or systems do not need to share private keys in order to communicate securely with one another
* Much larger key size and stronger encryption
* Also provides authentication; you can be guaranteed that the recipient is the intended party intrinsically through the decryption process since they are the only one who holds the key 

Cons:
* Much, much slower and resource-intensive
* Works better for smaller datasets 
* To securely and privately share data the same data with more than one party, each party's public key must be used to encrypt the plaintext

Used for:
* Authentication (i.e. SSH)
* Safely exchanging symmetric encryption keys
* TLS (Diffe Hellman exchange)
* Keeping communication between two people secret i.e. PGP

# Will Encryption Make Me More Secure?
*Can* encryption make you more secure? Absolutely. *Will* encryption make you more secure?Absolutely ***not***. Don't get me wrong; encryption is absolutely necessary, and everyone can benefit from it. If you need to guarantee privacy of confidential information, encryption is most likely the best solution readily available to you. Just don't go encrypting things all willy-nilly just because you think you should. Encryption that is used poorly will actually make a system *less* secure. Also consider that ransomware uses encryption to deny targets access to their data.

Also, you should not implement encryption until you have a way to manage your encryption keys. If you do not secure your encryption keys, this is not much better than not having encryption. If you lose your encryption keys, then you lose the ability to read and use the data that you have encrypted. This can be devastating if money is involved; as we have seen with the folks who have lost millions of dollars in crypto assets because they didn't have a system for managing their keys. At worst, it can be life-threatening to people relying on medical systems that use encryption with poor management. Key management is its own entire topic that I will cover later.

As computers are getting stronger, encryption is also becoming easier to break (This is still not easy by any means (point to Maersk example)). Our encryption is becoming stronger in response to computing power also becoming stronger. Although we are likely several years away from the average citizen needing to concerned, there are reports of the NSA being able to break encryption. Quantum computing will also reportedly weaken the strength of our encryption by allowing it to be broken much faster. It is a cat-and-mouse game; encryption will continue to evolve to match the weaknesses that threaten its usefulness. 

# How do you use it?
I am a huge fan of Docker. Most of my examples will be available as Docker containers, so that the commands I give you will run all the same, regardless of the environment where you're running Docker. [Install it](https://nortronon.com/2022/07/31/contain-yourself-part-1-a-docker-primer/) if you have not already to follow along.

We're going to focus on symmetric encryption for this example, using Fernet (god I hate the drink that unfortunately shares the same name) from the Python `cryptography` library. Fernet currently uses the AES-128 algorithm with Cipher Block Chaining (CBC) mode by default. Although there are much stronger algorithms and a number of modes, I don't want us to get too caught up in the details, and many other configurations would make the example more difficult to demonstrate. Just know that this is a very simple example of how encryption works, where an encryption key and some plaintext work in tandem to produce ciphertext, and the encryption key can be used to decrypt the ciphertext back into the plaintext from whence it came.

Here is the accompanying repository: [insert repo link]

To get started, pull the image. Re-tagging the image will allow us to more conveniently run this image as a container in the following commands:
```
docker pull n0rtr0n/symmetric-encryption-example:latest
docker tag n0rtr0n/symmetric-encryption-example:latest python-encrypt
```

Let's generate an encryption key. Run the following command will generate an AES-128 key and store it in an environment variable called `ENCRYPTION_KEY`, then echo that key to the terminal to show you what the string representation of it looks like:
```
$ ENCRYPTION_KEY=`docker run --rm python-encrypt new_key` && echo $ENCRYPTION_KEY
```
The output will be completely different but should look similar to the following:
```
x_cjzWUwkUUpHT_kkhZ-ukPMwHjXOfJnwAiydpGUD8c=
```

The value that it output, `x_cjzWUwkUUpHT_kkhZ-ukPMwHjXOfJnwAiydpGUD8c=` is the text format of the encryption key that will be used here. The following code powers the generation of this key:

```
from cryptography.fernet import Fernet
return Fernet.generate_key()
```

Let's try to encrypt some text!  Run the following command:
```
$ ENCRYPTED_TEXT=`docker run --rm python-encrypt encrypt_text $ENCRYPTION_KEY some_text_you_want_to_encrypt` && echo $ENCRYPTED_TEXT
```
If you use the key from the previous example, the output should look like the following:
```
gAAAAABi8erJV3F-5n_ChsHWREm8HuQ7Zuf55_znzXc69AeC5EQdkOQTwc4r7IZwb9NdZfTXGJUmS0m0J0Jyc220N-66LsnyV0FQs-BFsjquf59j3FH5jAU=
```

With our particular example, there are two things that will change what this ciphertext looks like: a) the encryption key and b) the plaintext. In more advanced algorithms, there are more factors that can go into the encryption and decryption process. Let's now decrypt that text using the key that we generated in the first step:

```
$ docker run --rm python-encrypt decrypt_text $ENCRYPTION_KEY $ENCRYPTED_TEXT
```

If all goes well, the text output should match the original text that you supplied in the encryption step:
```
some_text_you_want_to_encrypt
```

Try to break this; if you change even a single character of the text that you want to encrypt, you will get a much different result:
```
$ ENCRYPTED_TEXT=`docker run --rm python-encrypt encrypt_text $ENCRYPTION_KEY Some_text_you_want_to_encrypt` && echo $ENCRYPTED_TEXT
```

I only capitalized the first letter of our plaintext, and this is the result, which is completely different than before:

```
gAAAAABi8erqsIcFOrjS-RekvPjxHFatsMwgIOP5d9lI4Pb5ixNXPNedI-TjGXYctQCoBVUtKhEupOSHUGZ2vay4spZOe9CMil5yESsldDh2QyVgtt0XQd0=
```

Now try generating a different key, then using that key to try to decrypt the ciphertext generated by the original key:

```
$ ANOTHER_ENCRYPTION_KEY=`docker run --rm python-encrypt new_key` && echo $ANOTHER_ENCRYPTION_KEY

54wl_LiZbEE3Ag0Pt9ZaYwX4vjF6NNhE1dD3BHZO0L4=

$ docker run --rm python-encrypt decrypt_text $ANOTHER_ENCRYPTION_KEY $ENCRYPTED_TEXT 
```

You should get a lengthy error at this point, with a line similar to the following:
```
cryptography.exceptions.InvalidSignature: Signature did not match digest.
```
Followed by another lengthy stack trace, with the following message:
```
cryptography.fernet.InvalidToken
```

Long story short: you cannot use another encyption key to decrypt data from the same ciphertext, and changing any single bit of the input will alter the ciphertext in a significant way.

Play around with these examples!  Change it, try to break it, and try to improve it. How would you use these techniques to help secure the data that you work with? How would you secure your encryption keys? What are some other symmetric encryption algorithms you can use? What else is required for more advanced algorithms? I will also cover these topics in depth later.

# Closing Thoughts
Encryption is a powerful tool that we can use to protect the privacy of our data. Symmetric encryption is most commonly used to protect data at rest. Used carefully, encryption can add a strong layer of security to our data. Used incorrectly or nefariously, encryption can have disastrous consequences!

### Resources:
* Practical Cryptography for Developers: https://cryptobook.nakov.com/
* Python `cryptography` documentation: https://cryptography.io/en/latest/
* Symmetric Encryption 101: https://www.thesslstore.com/blog/symmetric-encryption-101-definition-how-it-works-when-its-used/
* Another comprehensive symmetric encryption guide: https://medium.com/codeclan/what-are-encryption-keys-and-how-do-they-work-cc48c3053bd6