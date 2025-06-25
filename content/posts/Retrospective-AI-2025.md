---
title: "5 Months of Professional AI Development : A Retrospective"
date: 2025-06-23T16:11:48+01:00
tags: [AI, LLMs]
featured_image: ""
description: "A short debrief on developing with LLMs, Neural Networks and Spacy"
---

# 5 Months of Professional AI Development : A Retrospective *In Progress*

At my current workplace, which is critical naval infrastructure in the UK, we have been investigating possible uses for LLMs and other AI systems in our work. As a developer with a Graduate Degree in the subject, I was assigned to investigate possible use cases for the business. Throughout this process, and a not negligible amount of development with LLMs on the side, I have been increasingly less and less convinced by LLMs as something useful outside of very specific contexts. I have found this to be particularly true for local LLMs running on Ollama. This may be surprising to some readers, but I have found writing software systems with LLMs as an integral part of them to be a generally frustrating experience, even after implementing Prompt Engineering techniques suggested by figures such as Andrew Ng. 

In the interest of conveying information that I think will be less likely known to the reader quickly, I will start by enumerating considerations that I think are less likely to have been addressed explicitly in other places.

## Legal Considerations Are Extraordinarily Important

One thing that I have not seen touched on very strongly, but is present, is that LLMs can very easily generate easily avoidable legal situations. For example, while LLMs are considered to be quite effective in summarising texts, there is still a significant chance of either placing the focus of the summary in such a way that misconstrues its source, or even outright hallucinating details not present in the base text[^1]. While this would be fine for situations where we want a basic summary of some unimportant article, this becomes extraordinarily problematic if we are using the results of this AI to judge a person.

Even in situations where it is lawful to process public personal data in such a way (Which are quite rare inside of the EU or the UK), there is often a legal requirement to be able to defend such judgements. If HR decides to terminate a person on the basis of an LLM's output, how will they be able to defend themselves in court if the LLM has invented a detail or misconstrued a person? What if important details are omitted, such as a person's prior public criminal history? How would a company be able to justify hiring a violent criminal, when they could have reasonably known by googling it, but have decided to rely on an LLM anyway?

The reader may respond "A person can consult multiple sources". This is true, yet I don't think that the culture is here yet for such a thing. There is not an appropriate understanding of these systems in our overarching culture yet. There are many people who think that these systems are some sort of panacea. Organisations must be aware of this, be aware that they're likely in no position to change this culture, and act accordingly. 

## Prompts That Generate Data Can Break Your Program 

It has been stated, by Andrew Ng nonetheless, that one way of significantly cutting down issues with data provided to a software system is to specify in a prompt that you would like the desired result in JSON, probably assigned to a variable named in that prompt[^2]. While this does make any results from an LLM much easier to interpret in the code, one very irritating problem that I have had (at least with local LLMs) is that, despite being told contrary, there LLM still decides to provide commentary on why it has made such a decision occasionally. 

This means that it's paramount that all LLM output has significant preprocessing to remove such unprompted details. LLM output should never ever be treated as reliable or devoid of fault, because if you do not do this your program could break. 

## LLMs at Scale Will Break Very Frequently

The absolute most reliable systems which we have at this moment hallucinate at [roughly](https://huggingface.co/spaces/vectara/leaderboard) a rate of 1 in 140 as of time of writing. This is slightly more reliable than an ANPR system, which is at about 1 in 100. While this is fine for most prompts, this can compound very quickly if you want to get multiple insights from some text or have multiple layers of LLMs acting on each other. Multiple users produce the same problem.  Therefore there should be significant errors on a daily basis, and this is before we even consider the aforementioned ignorance of prompt commands. 

This is, in itself, not something that's atypical in software systems. Packets lose details along a wire, for instance. However, this is, currently, a much more significant problem as it's much slower than sending another packet and the result is at least somewhat dependent on the innate structure of the prompt. At time of writing, I'm unaware of any patterns for handling this currently[^3].





## How did I come to these conclusions?

Although this may not be in the interest of any reader who just wishes for a quick summary, those who are interested in how I came to the conclusions

I have used LLMs significantly in the following contexts:

- Classifying large amounts of documents into a discrete set of categories to determine levels of danger for certain incidents
- Ranking documents on the basis of different criteria such as levels of violence present in a text
- Summarising documents from the internet
- Producing text to provide context for learning vocabulary in foreign languages
- Producing first drafts for code and text

As for where I am approaching this from, I have been programming for about 14 years (I.E. since I was 13), but only in a professional setting for roughly the past 4 years. Therefore, many of these observations may have emerged from my own lack of experience. In this world of noise around LLMs, let the reader experiment himself and take everything that he reads with a pinch of salt.  





[^1]: The idea that this hallucination problem can be entirely eliminated, I think, is absurd
[^2]: This latter detail is added by me
[^3]: A couple of approaches could involve 1) Shuffling a list if a list of items is involved or 2) Alternative prompts with the same result 

