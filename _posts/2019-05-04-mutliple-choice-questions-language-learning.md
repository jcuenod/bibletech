---
layout: post
title:  "Multiple Choice Questions for Language Acquisition"
subtitle:  "Gradable Quizzes that don't give away the answer"
date:   2019-05-04 14:00:00 +0300
author: James Cuénod
header-img: "img/contact-bg.jpg"
---

Thoughts about language acquisition (particularly the academic kind that is only there for reading) are often brewing in the back of my mind. Recently I read <https://www.gwern.net/Spaced-repetition> and found it quite stimulating even though the ground it covers is now well-worn.

One of the things that is clearly important is testing. To do lots of testing though, requires lots of grading (or "marking" if you're from South Africa). But lots of grading is lots of work. Lots of work, unless you can automate it, that is. The easiest (best?) way to automate grading is by using multiple choice questions but often multiple choice questions in language courses are silly. They look something like this:

```
Choose the correct gloss for λόγος:
 1. lord
 2. word
 3. boat
 4. city
```

The student then has the job of elimitating the least likely contenders and (probably) also comparing the other words in the quiz only to see πλοῖον listed somewhere else. But if, on the other hand, you give students a quiz that looks like this:

```
Gloss λόγος: \_\_\_\_\_\_\_
```

Grading can't be automated because of the amount of variation in possible answers.

So we need something that forces students to know more than the bare minimum required to use the process of elimination to guess an answer. But we also need to give a list of options so that grading can be done automatically.

The solution came from [the article I mentioned above](https://www.gwern.net/Spaced-repetition):

> With some NLP software, one could write dynamic flashcards which test all sorts of things: if one confuses verbs, the program could take a template like “$PRONOUN $VERB $PARTICLE $OBJECT % {right: caresse, wrong: caresses}” which yields flashcards like “Je caresses le chat” or “Tu caresse le chat” and one would have to decide whether it was the correct conjugation. (The dynamicism here would help prevent memorizing specific sentences rather than the underlying conjugation.)

But I don't think we need NLP to do this. All we need is syntactically tagged texts that we can replace individual words on. This is an idea that must be out there in the wild somewhere so I am hesitant to build it but it seems so close... Email me if you've done it or seen it done. I have a feeling bible-online-learner was doing something like this. I have a positively associated memory of how they were doing testing from a youtube video maybe? I'll update this post if I find it.
