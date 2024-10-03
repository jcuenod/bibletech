---
layout: post
title:  "AMTA Reflections"
subtitle:  "Some thoughts related to Bible translation after AMTA '24"
date:   2024-10-03 14:00:00 +0300
author: James Cuénod
header-img: "img/post-bg-hebrew.jpg"
---

- **Post-editing** workflows:
	- Semantic divergence
		- (see https://www.proquest.com/openview/3145eaf5b2b930c434b6afd1766294e8/1?pq-origsite=gscholar&cbl=18750&diss=y)
			- This paper shows that semantic divergences *do* negatively affect trained models (leading to model collapse). I don't think the author tried labelling the types of divergence during training (which I suspect may have improved results, but would still require her work on labelling).
			- There was some awesome work on classifying divergences and identifying the specific words/phrases for a user. The author also measured the impact on translation speed of showing these details to a user and found significant advantages.
		- Some thoughts:
			- Can aqua identify aligned pairs to remove that will improve quality?
			- Can we train a model that will classify insertions, deletions, divergences?
	- Fuzzy match repair
		- Lot's of content related to using translation memory and doing "fuzzy match repair" (FMR). The idea is to find similar phrasing that's already been translated and then replace the bits that are different. This can require more than simple substitution (e.g., the gender of your noun changes but there's a pronoun somewhere else).

- **Data and training** required
	- There was a paper about how much data was "enough". Fine-tuning llama3 on relatively high-resource languages was able to exceed the performance of chatgpt-3.5 with about 10k samples. With 5k, they were able to exceed the llama3 baseline (but saw regressions with 1k and 2k - they didn't know why). But (1) this involved higher resource languages (like Korean) and (2) they were comparing to chatgpt-3.5 (and that's no longer a meaningful baseline).

**Notes**:
- LLMs are where things are headed (including closed LLMs)
- Ultra low-resource is not a focus (but the challenges are of interest)
	- That said, we don't have a definition for low resource (Cantonese was counted as low-resource, for example)
- Quality assurance is an issue.
	- Document level consistency came up a bunch of times. LLMs were slated as a solution because, instead of translating segments (sentences), they can do larger chunks and more reliably take context (including key terms) into account.
	- There was some resonance with legal translation challenges, where key term translation is critical. LLMs were again slated as a (partial) solution, because they allow us to put stuff like glossaries in context. 

**Ideas**:
- Let's do ngram selection to identify closest related languages in ebible and augment training data (we could use this in NLLB)
- Let's ft much smaller models (maybe qwen?/phi)
- Can we do phrase matching and suggestion for KTs?
- Can we use phrase tables for alignment and key terms? (or can we do other kinds of automated key term extraction)
