---
title: "Scheduling a Data Breach for Tomorrow"
subtitle: "What happens to your data after you hit enter"
date: 2026-06-29 14:00:00 +0300
author: James Cuénod
---

# Scheduling a Data Breach for Tomorrow

Putting sensitive data into an LLM is a data-handling decision. It should be treated like sending data to any other vendor system: cloud storage, analytics, CRM, customer support tooling, source-code hosting, or collaboration software.

That does not mean the answer is always no. It means the answer should depend on the data, the vendor, the contract, the product configuration, and the failure modes we are willing to accept.

The prompt box makes this easy to forget. It feels like conversation, but operationally it is data transfer. The text may pass through an application layer, a model provider, logging systems, abuse-monitoring systems, file processors, retrieval stores, observability tools, support systems, and evaluation pipelines. Different products and enterprise agreements handle this differently, but the basic question is the same: where does the data go after someone submits it?

There are four broad risks worth separating.

## 1. Storage and Retention

LLM providers often retain prompts, outputs, uploaded files, metadata, or derived records for some period of time. In some cases this is for abuse monitoring. In others it is for debugging, product functionality, safety review, support, or legal compliance. Some enterprise agreements may reduce retention. Every point of retention is a point at which there is an opportunity for exposure.

If sensitive data is stored by a vendor, even temporarily, it can be exposed through the usual routes: breach, misconfiguration, excessive internal access, logging mistakes, support tooling, legal process, or application bugs. None of this requires a science-fiction failure where “the AI remembers everything.” It only requires normal software to fail in normal ways.

We already have examples in the category. In 2023, a ChatGPT bug allowed some users to see titles from other users’ chat histories. OpenAI later said that limited payment-related information for some active ChatGPT Plus subscribers may also have been visible during a specific window. That was not model memorization. It was an application-layer incident. In other words, it's only the authentication/authorization flows that prevent you from seeing other people's ChatGPT histories. Those flows can be broken, and so can things like "the thing we use to secure API logs". Misconfigured S3 buckets are a well-documented, recurring failure mode (see https://www.upguard.com/blog/s3-security-is-flawed-by-design).

There is also the internal-access problem. Vendors can have employees, contractors, reviewers, support staff, or operators with access to sensitive systems. This is not unique to LLMs. The FTC’s Ring complaint alleged that employees and contractors had excessive access to private customer videos, including one employee who viewed thousands of recordings from female users. LLM vendors are not all Ring, but once data enters a vendor environment, internal access controls matter.

Zero data retention policies are the main mitigation for these kinds of issues. They do not solve every problem. They may not cover metadata, abuse investigations, product state, or downstream systems. But they reduce the amount of your sensitive content that is left sitting in someone else’s infrastructure after the request is complete.

## 2. Training on your Data

Most major providers now make commitments about not training on business or API data by default. Those commitments matter. They should also be read carefully and put under contract. But these policies are often complicated and change with very little notice.

Policies can differ across consumer products, enterprise products, APIs, beta features, feedback mechanisms, safety reviews, opt-in settings, and fine-tuning workflows. A company may truthfully say that customer data is not used for training by default while still allowing training through opt-in programs, feedback flows, special product features, or separate agreements. Policies also change, and AI companies with strong incentives to improve models know that high-quality user interactions are valuable.

I often test things in Google's AI Studio. Google's policy has been that when you're on the paid tier, your prompts do not become part of their training data. So if you log into AI Studio on a free account, you know where you stand: everything you drop into the chatbox is fair game. But I'm on the paid tier, and AI Studio recently added support for using a paid API key from the environment — even for paid-tier users on the free access path. That raises an obvious question: was the free access path previously outside the paid-tier data protections, and does this change fold it in, or is this a parallel path with its own policies? I still don't know. The answer is buried in policy language that I would not be confident I had sufficiently understood even after reading the parts that seemed relevant.

In addition, Silicon Valley is not exactly a paragon of virtue. If it turned out that one of the LLM vendors had violated their own policies, maybe a class-action lawsuit would go somewhere. But more likely it would turn out that "an intern did it" and, after a very severe slap on the wrist, we would go on with our lives. At that point, to be honest, you can't put that data toothpaste back in the tube.

## 3. What the Model "Knows"

If sensitive data does enter training, there are several ways exposure can happen. The simplest is **direct disclosure**. A user asks the model about an organization, customer, codebase, project, incident, or internal process, and the model produces information that came from training data.

Direct disclosure is probably just a subset of **memorization**. Models can sometimes reproduce training examples, especially unusual strings, duplicated passages, code, secrets, or data repeated during fine-tuning. This is not the same as a database lookup, but the practical concern is similar: material that went in may later come out. This has been documented in reporting from the New York Times and others showing that models can be induced to reproduce copyrighted text and GPL-licensed code verbatim (see, for example, coverage of AI systems reproducing copyrighted articles: the New York Times lawsuit at https://www.nytimes.com/2023/12/27/business/media/new-york-times-openai-microsoft-lawsuit.html and IEEE Spectrum's account of the verbatim-reproduction examples in that complaint at https://spectrum.ieee.org/chatgpt-new-york-times, and research on code generation reproducing licensed code such as “Foundation Models and Fair Use” (Henderson et al., 2023): https://arxiv.org/abs/2303.15715).

There are also model inversion and related privacy attacks. These aim to infer or reconstruct sensitive information from model behavior. The practicality varies by model, access level, and attack method, but the category matters because it treats the model as an interface to information about its training data. If content can be inferred from embeddings, this kind of attack would presumably work against embedding models and generative LLMs alike (more on this below).

Distillation creates another path. Distillation usually means training a smaller or separate model to imitate a larger one using the larger model’s outputs. This can be legitimate. It can also be used for model theft. If the teacher model’s behavior reflects sensitive training data, a student model may inherit some of that behavior. The person doing the distillation may be trying to copy capability, not secrets, but the boundary is not clean, and it stands to reason that distillation attacks may inadvertently also exfiltrate secrets from models (potentially even circumventing post-training guardrails that are designed to prevent the model from doing things like producing private information about people).

## 4. Data Exhaust

A production LLM system usually creates more than prompts and responses. It may create logs, traces, embeddings, retrieved chunks, moderation records, cached outputs, evaluation datasets, human-review queues, analytics events, support tickets, and debugging artifacts.

A company may negotiate strong terms with a model provider and still leak sensitive data through its own application. The app logs full prompts. The observability tool captures request bodies. The support system stores user conversations. None of this is special to AI. LLMs just increase the amount of high-context information users are willing to submit. People paste in contracts, source code, credentials, employee data, and unreleased financials, because the system is useful. The more useful the system is, the more tempting it becomes to feed it the data that matters.

Embeddings and retrieval systems need particular care. Embeddings are not the original document, but they can be inverted to recover approximations of the original text. For example, “Text Embeddings Reveal (Almost) As Much As Text” (Morris et al., 2023) demonstrates that it is possible to reconstruct readable text from embedding vectors with non-trivial fidelity (https://arxiv.org/abs/2310.06816). This is not perfect reconstruction, but it is enough to show that embeddings should be treated as sensitive data when they are derived from sensitive inputs. Retrieval systems can also break access control if they return content a user would not have been allowed to read in the source system.

RAG is often better than training private data into a model because the data can remain external, governed, and removable. But that only works if the retrieval system enforces permissions, preserves tenant boundaries, avoids unnecessary logging, and supports deletion. Otherwise the privacy problem has just moved from the model to the retrieval layer.

## What this means in practice

The practical rules for handling sensitive data are familiar:

Classify the data before use. Know which categories are allowed in which tools. Use enterprise products with contractual protections. Assume that anything you send through a chat prompt or an API call to an LLM may be logged, and prefer zero data retention for high-risk workloads. Redact or summarize sensitive fields before sending them to a model. Avoid fine-tuning on raw sensitive data unless there is a strong reason. Apply the same access controls to retrieval systems as you apply to the original data. Do not log prompts and outputs by default. Limit who can review conversations. Track where data is copied. Make deletion real across logs, files, embeddings, caches, and evaluation datasets. Review new AI features the same way you would review a new vendor subprocessor or data export path. If data would require review before being sent to a vendor or partner by any other route, it should require review before being sent to a model.

Using LLMs with sensitive data **may be worth it**. In many cases it will be. But the decision should be made with the actual risk surface in view: storage, retention, internal access, policy drift, training, memorization, inversion, distillation, retrieval, logging, and deletion.