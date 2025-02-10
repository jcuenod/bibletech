---
layout: post
title:  "Trust and LLMs"
subtitle:  "A taxonomy of trust in the age of Large Language Models"
date:   2025-02-05 14:00:00 +0300
author: James Cuénod
isStarred: true
header-img: "img/post-bg-06.jpg"
---

Let’s say we don’t want our data scarfed down by the hungry hungry LLM trainers. We have a range of options. From worst to best:

## 1. Untrusted Providers

The default is really providers that we can't trust with our data. There are two kinds of providers in this category:

1. On one hand, we have the vendors who sweetly swear they won't train on our data, but you can hear they have a rumbly in their tumbly and they just want a small smackerel of something sweet (this is the concern with [Deepseek](https://www.deepseek.com/)). Whether it's fair or not, I trust OpenAI to do what they say with your data more than I trust Deepseek (and I don't particularly trust OpenAI). This comes down to an America vs China issue, which is to say, geopolitics are going to play a role in what providers you consider trusted or untrusted. So maybe Deepseek belongs in category #2 in your judgement.
2. On the other hand, we've got the brutally honest bunch who just flat-out tell us they're going to feast on our data buffet (see the free versions from OpenAI and Google, where they tell you your data will be ["Used to improve our products"](https://ai.google.dev/pricing)).  Either way, they're going to take a byte of your data ;)

**Risk**: You're literally giving away your data.

## 2. Trusted Providers (Unaligned Incentives)

OpenAI and Google are both willing to barter a deal with us that says they will preserve the privacy of our data. But they are in the business of training models, so...  trusting a giant model factory to *not* use your data may be a bit like asking a cookie monster to guard the cookie jar.  Sure, they might *promise* not to peek, but their entire business model is built on gobbling up data to make ever-bigger and better models (even just for human preferences). Their incentives are still pointed at data-munching, even if they promise to guard the jar.

**Risk**: The provider you trusted is not trustworthy after all.

## 3. Trusted Provider (Aligned Incentives)

The problem of unaligned incentives is the appeal of open models hosted by providers whose business is hosting models (think [Fireworks](https://fireworks.ai/), [Replicate](https://replicate.com/), [Groq](https://groq.com/)...). These providers have no incentive to gather and retain your data because it's not their business. Of course, it could be their business if Meta knocks on the door and says "we sure could use some more training data". So maybe these providers end up in the same bucket (an example of this problem is the way cellphone carriers sell your location data). "Aligned" incentives today can become "unaligned" tomorrow when a new revenue stream or business opportunity winks into existence.

Two subsections under this category:

1. There are some providers in this bucket who are willing to stake their business on the veracity of the claim that your data will remain private with them ([Prediction Guard](https://predictionguard.com/)). They are just promising that if Meta came knocking, they would say "we're not home".
2. If providers can host models, then so can you (provided you have sufficient compute). So you may consider hosting a model on a gpu in the cloud, or to be ultra-safe, in your server-room. This allows you to control where your data is encrypted and decrypted as it moves across the interwebs.

**Risk**: The provider you trusted isn't trustworthy, perhaps because the provider is compromised by a third party.

## 4. Trusted Computing

Of course, the above approaches are subject to third party attacks and leaks. If you host a model on AWS and some attacker is monitoring your VM, it's not private after all. This is true all the way back up the stack. Want to take trust out of the equation altogether? Enter Trusted Computing! This is where your LLM computations happen in a secure enclave, a sort of digital fortress within the processor.  It's like having a tiny, tamper-proof room where your data and the model can dance without anyone peeking or pilfering. The provider can host the hardware, but even *they* can't easily snoop on what’s happening inside the enclave. Think Fort Knox for your LLM queries. This is what Apple pitched as [Private Cloud Compute](https://security.apple.com/blog/private-cloud-compute/) and it's also what [Tinfoil](https://tinfoil.sh) are selling.

**Risk**: The hardware & software attestations (or the software itself) have vulnerabilities. This is possible, but compromising this level of security is an order of magnitude more challenging than everything in category 3.

## 5. End to End Encryption

Trusted computing isolates a space in the hardware where you can trust the software running there to be software you allowed (and, perhaps, vetted) and where even the OS cannot spy on you. But what if you didn't even need to trust the OS. What if you could send encrypted data to a provider, the provider didn't have a way to decrypt it but could send you back a response that was encrypted with the same method as your original data. Now you can decrypt it, and no one has seen it along the way. This is called Homomorphic Encryption (HE) and although it sounds like it's out of Harry Potter, it is a real thing. I'm just not aware of any providers offering this, and LLMs have to be trained in such a way that they support the mathematical operations required. Cryptography is also computationally expensive (by design), this means slower inference. But because we're already talking about LLMs, the speed is not actually the issue; the implied higher costs are likely to be the deal-breaker. So your best bet right now is to go with trusted computing solutions.

**Risk**: Your HE algorithm is insecure or your software supply chain is compromised. This happens in cryptography, but provable security is _really_ hard to break.
