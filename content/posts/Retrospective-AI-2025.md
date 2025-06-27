---
title: "8 Months of Professional AI Development : A Retrospective"
date: 2025-06-27T16:31:48+01:00
tags: [AI, LLMs]
featured_image: ""
description: "A short debrief on developing with LLMs, Neural Networks and Spacy"
---

# 8 Months of Professional AI Development : A Retrospective

At my current workplace, which is critical national infrastructure in the UK, we have been investigating possible uses for LLMs and other AI systems. As a developer with a Graduate Degree in the subject, I have been assigned to investigate possible use cases for the business since the end of January[^1] this year. Throughout this process, and a not negligible amount of development with LLMs on the side, I have been increasingly less and less convinced by LLMs as something useful outside of very specific contexts, but that they're not completely useless. In particular, I have found working with local LLMs to be a disappointing experience. This may be surprising to some readers, but I have found writing software systems with LLMs as an integral part of their operation to be a generally frustrating experience, even after implementing Prompt Engineering techniques suggested by figures such as Andrew Ng. 

In the interest of conveying information that I think will be less likely known to the reader quickly, I will start by enumerating considerations that I think are less likely to have been addressed explicitly in other places. From there, I'll go through ones that people are much more likely to know but I feel are an important part of the retrospective. Finally, I'll enumerate a list of use cases where I think that LLMs work and don't work and explain how I reached my conclusions. 

## Legal Considerations Are Extraordinarily Important

One thing that I have not seen touched on very strongly, but is incredibly important, is that LLMs can very easily generate dangerous and avoidable legal problems. For example, while LLMs are considered to be quite effective in summarising texts, there is still a significant chance of either placing the focus of the summary in such a way that it misconstrues its source, or even outright hallucinating details not present in the base text[^2]. While this would be fine for situations where we want a basic summary of some unimportant article, this becomes extraordinarily problematic if we are using the results of this AI to judge a person or some other vital situation.

Even in situations where it is lawful to process public personal data in such a way (Which are quite rare inside of the EU or the UK as a result of GDPR or DPA respectively), there is often a legal requirement to be able to defend such judgements. If HR decides to terminate a person on the basis of an LLM's output, how will they be able to defend themselves in court if the LLM has invented a detail or misconstrued a person? What if important details are omitted, such as a person's prior public criminal history? How would a company be able to justify hiring a violent criminal, when they could have reasonably known by googling it, but have decided to rely on an LLM anyway?

The reader may respond "A person can consult multiple sources". This is true, yet I don't think that the culture is here yet for such a thing. There is not an appropriate understanding of these systems in our overarching culture yet. There are many people who think that these systems are some sort of panacea, especially as one goes further up an organisation's hierarchy. Organisations must be aware of this, be aware that they're likely in no position to change this culture, and act accordingly. 

## Prompts That Generate Data Can Break Your Program 

It has been stated, by Andrew Ng nonetheless, that one way of significantly cutting down issues with data provided to a software system is to specify in a prompt that you would like the desired result in JSON, probably assigned to a variable named in that prompt[^3]. While this does make any results from an LLM much easier to interpret in the code, one very irritating problem that I have had (at least with local LLMs) is that, despite being told contrary, the LLM still decides to provide commentary on why it has made such a decision occasionally. It ignores instructions send to it, and it does this frequently enough that it must be acknowledged by any software system that handles data from them. 

### However, this can be a strength

However, this very indeterminism has some uses. For instance, given how this malformation can occur within requests for JSON, there could be a situation where LLMs are used to generate test data en masse in that format. This would give systems that are being tested the opportunity to occasionally receive curve balls that a team may not have considered, leading to better developed systems. 

## LLMs at Scale Will Break Very Frequently

The absolute most reliable systems which we have at this moment hallucinate at [roughly](https://huggingface.co/spaces/vectara/leaderboard) a rate of 1 in 140 as of time of writing (If we consider this trustworthy). This is slightly more reliable than an ANPR system, which is at about 1 in 100[^4]. While this is fine for most prompts, this can compound very quickly if you want to get multiple insights from some text or have multiple layers of LLMs acting on each other. Additionally, multiple users produce the same problem.  Therefore there will be significant errors on a daily basis, and this is before we even consider the aforementioned ignorance of prompt commands. 

This is, in itself, not something that's atypical in software systems. For instance, packets are often subject to damage when transmitted. However, this is, currently, a much more significant problem as it's much slower than sending another packet to send another prompt to an LLM and receive a response. In addition, the result is at least somewhat dependent on the innate structure of the prompt, as all LLMs are reliant on a probability distribution of subsequent possible tokens based on the prior text. If the prior text is the same, the results are going to look the same (especially if the temperature of the LLM is properly set). At time of writing, I'm unaware of any patterns for handling erroneous responses from LLMs at scale.[^5]

## Local LLMs tend to be ineffective at all but the most common tasks and are slow

Do you want to translate into a language like Latin? Well, local LLMs are typically a terrible way to go about it. Do you want to perform anything complex in a timely fashion? Well, you'd best not be running your instance of Ollama on any standard consumer hardware, because it will be astonishingly slow. Unless I have missed something in my Nvidia GPU settings, even with a 2 year old £1000 gaming system to prompt Deepseek-R1 can take as long as 1 minute to respond when doing so from the REST API built into it. Gemma 3 was much quicker, but when it comes to less common tasks it can make really basic errors. I mentioned earlier Latin, which surprisingly has a rather huge corpus (After all, it dominated western intellectual thought for over 1000 years). Gemma 3 cannot even get the basics of the grammar to work as intended, the sorts of things that are extraordinarily common in that corpus and any 1st year student of the language would be able to tell you within a few weeks of studying it. 

ChatGPT and Claude tend to get this latter use case rather effectively, but they're still not that great.

## Niche Tasks don't really work for LLMs

For example, one day out of curiosity, I wanted to see how ChatGPT would tackle classifying biblical books by their vocabulary difficulty. I've written a system that does this, looking at the frequency of the words within the biblical corpus and calculating relative difficulty of each book on the basis of this. ChatGPT came out with vastly different, incorrect results. 

Now, I think it's somewhat unreasonable to expect a system that is essentially relying on training data to recall this sort of niche information. However, I think that devs ought to be aware of this short coming when they're trying to outsource these scenarios to an LLM to save time. 

## LLMs are extraordinarily easy and quick to work with

As I am sure almost every single reader here is aware, prompting an LLM is a much easier and more intuitive task than working with code directly much of the time, in particular when it comes to working with domains (Especially inside of Natural Language Processing) which are ill understood or quite complicated to implement. For instance, I was able to save a great deal of development time by having an LLM classify pre-existing data rather than going to the effort of investigating text document classification in a great deal of detail (Lots of work understanding algorithms contemporaneous with GPT-2 such as BERT and similar systems) and implementing these. For instance, I realised fairly early on in the process of developing with LLMs that it was much more time efficient to do this than to handle text classification by training a Neural Network with SpaCy. 

I think a use case, where the result is not critical and the cost of learning a system is very high in terms of cognitive load, LLMs can provide a stopgap which is far easier on the mind. 

# Use Cases

## What I think works

- First Drafts
- Mock Frontends which exist purely to give a person an idea of what a website may look like (Sort of like more involved wireframing)
- Generating lots of ideas quickly
- Summarising low importance information
- Generating test data designed to surprise or break your system
- Situations where human text input would be acceptable or necessary
- Where some other format such as JSON is necessary but isn't critical
- When mathematical precision is not necessary
- At times when jobs can run in the background and a user is happy to wait

## What I think doesn't work

- Learning entirely new domains of knowledge (Get a textbook, read it, and memorise it)
- Summarising anything that has relevance to anything legal or you or your company could be liable for
- Anything that requires any level of precision (This includes language translation, I cannot count the amount of times I have wanted to strangle friends of mine for throwing Latin texts at it without knowing Latin)
- Situations where multiple LLMs are reliant on each other (In other words I don't think Agentic systems are generally a good idea)
- Anywhere where retaining data is necessary
- Anywhere where data must be within a set range and absolutely no deviation is tolerable
- Any system which is required to be quick



## How did I come to these conclusions?

I have used LLMs significantly in the following contexts professionally and experimenting with them in off hours:

- Classifying large amounts of documents into a discrete set of categories to determine levels of danger for certain incidents
- Ranking documents on the basis of different criteria relevant to my work place
- Summarising documents from the internet
- Producing text to provide context for learning vocabulary in foreign languages
- Producing first drafts for code and text

As for where I am approaching this from, I have been programming for about 14 years (I.E. since I was 13), but only in a professional setting for roughly the past 4 years. This hobbyist experience was mostly with C and working with lower level systems and games, while my professional experience has been almost entirely writing web applications, with a short 3 month stint working with AI. Therefore, many of these observations may have emerged from my own lack of experience. In this world of noise around LLMs, let the reader experiment himself and take everything that he reads with a pinch of salt.  





[^1]: The additional three months comes from an AI startup I worked for before my current role
[^2]: The idea that this hallucination problem can be entirely eliminated, I think, is absurd
[^3]: This latter detail is added by me
[^4]: This is also, as an aside, why I think using an LLM as a starting point to learn anything you're not familiar with is a terrible idea. You have no prior experience, how will you know you're not being sent down the garden path, even if you have other sources?
[^5]: A couple of approaches could involve 1) Shuffling a list if a list of items is involved or 2) Alternative prompts with the same result 

