---
title: "Hallucinations"
subtitle: "I do not think it means what you think it means"
date: 2025-02-21 14:00:00 +0300
author: James Cuénod
---

# "Hallucinations"

If you ask anyone using LLMs what the biggest challenge with using them is, "Hallucinations" is one of the first answers that will come up. That is LLMs "make stuff up". They "invent facts" of all kinds, famously [inventing case law](https://www.forbes.com/sites/mattnovak/2023/05/27/lawyer-uses-chatgpt-in-federal-court-and-it-goes-horribly-wrong/) that the lawyer did not validate.

## One. What LLMs Do

The word "hallucination" suggests a misunderstanding of what LLMs are doing. It implies that there's a distinction between what an LLM is doing when it generates true and accurate text and what it's doing when it "doesn't know the answer".

When I tell you about my holiday at the beach, I'm remembering something I experienced. When I tell you about my holiday on the moon, I'm making up a story. But, for an LLM, these two outputs involve precisely the same process. **LLMs are just numbers flowing through huge matrices of other numbers**; there is no "know" or "don't know", there's no "remember" or "learned", let alone "made up". There's just a probability distribution that indicates what word would probably come next in the kind of text that was in its training data.

This also means that LLMs don't "know" when they're "unsure" of something. There is no way for an LLM to attach a probability or likelihood to the veracity of its own output. The only probabilities it knows are those of the next word appearing in a sequence of text, completely devoid of meaning (let alone truth).

So, the phenomenon we call _hallucination_ is a judgement entirely **dependent upon _us_** and has nothing to do with the LLM.

## Two. Fixing the Problem

The fact that using the word "hallucination" means we don't really know what LLMs are doing and that means we also don't know how to fix the problem. Ever since ChatGPT launched, we've been waiting for different providers to release bigger models trained on more data. "The more parameters, the less hallucination", we assume. **But this problem is not going to go away** if we give LLMs more weights or more training data. Again, it's what they do. It's also not going to go away if we give them more context. So all the talk about "RAG" (Retrieval-Augmented Generation) solving the problem of hallucination is also completely mistaken. It helps with this phenomenon, but it doesn't change the fact that, again:

> LLMs are just numbers flowing through huge matrices of other numbers; there is no "know" or "don't know", there's no "remember" or "learned", let alone "made up". There's just a probability distribution that indicates what word would probably come next in the kind of text that was in its training data.

Using the word "hallucination" seems (to me at least) to suggest that there is a way to fix it. But it's literally what they're designed to do.

Another way to think of this is that LLMs are designed to produce sequences that look like human-created text and appear to have meaning. That process produces both accurate statements and "hallucinations", because both "accurate statements and hallucinations" are "sequences that look like human-created text and appear to have meaning".

## Three. What LLMs _Are_

I deferred this point to the end, because it's well known, and I think the first reason is more important. But the last concern I have with "hallucination" is that of **anthropomorphism**. I am concerned that we're giving LLMs too much implicit agency.

There's a host of problems with giving LLMs agency. Perhaps the most troubling one is that I see people deferring responsibility to them. I hear the logic "we let people do this task and they don't get 100% accuracy, so we may as well let the LLM do it". That argument is stupid, because the LLM will never be held responsible.

![A computer cannot be held accountable](/bibletech/img/post-images/accountable.png)

In addition, we have the insane ["research"](https://www.anthropic.com/news/alignment-faking) that Anthropic has done to figure out whether LLMs might turn evil when we scale them up enough. Don't get me wrong, I mostly love Anthropic. But this research is just nonsense, and it comes from this confused anthropomorphized view of what LLMs are. The fact that we use the language of _hallucination_ is indicative of the fact that, at least subconsciously, we don't know what they are: again, numbers flowing through matrices of other numbers.

## Three. The Solution

I don't know the solution. Some people have suggested:

- [BS](https://link.springer.com/article/10.1007/s10676-024-09775-5) (you can expand that in your own mind)
- [Confabulations](https://www.beren.io/2023-03-19-LLMs-confabulate-not-hallucinate/)
- Probably a bunch of others.

I don't like either of these options, because they still have the problem of anthropomorphization. The problem with finding a new word is that I want one that communicates either that "hallucinations" are the same thing as any other thing that LLMs produce (which obviates the need for a word), or I want to communicate that "hallucinations" are a subjective judgement _we_ are making (which would really help remove agency). But I don't have a good word to propose and, collectively, we've basically settled on hallucination. So what are ya gonna do except complain at the internet?
