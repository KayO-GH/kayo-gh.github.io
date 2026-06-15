---
layout: post
title: "Building Legislation Explainer"
date: 2026-06-14
comments: true
---

<img class="post-hero" width="1000" alt="Landscape screenshot of the Legislation Explainer Gradio app" src="/images/building-legislation-explainer.jpg" />

# Building Legislation Explainer (Build Small Hackathon)

This project started from a very Ghanaian kind of panic.

In May 2026, a [draft NITA bill](https://nita.gov.gh/wp-content/uploads/2025/NITA-2008-act-2025-1.pdf) started making the rounds on Ghanaian X. The reaction from the tech community was not exactly calm. There were concerns about licensing fees, taxes on revenue, restrictions around foreign co-founders, and a general feeling that something important had been sitting in plain sight while most of us were busy living our lives.

It bothered me even more when I found out that the bill had not been public for months before we all had a collective panic attack. But it was a long dense legal document written in the kind of language that puts people like me to sleep. 😅

During an online townhall-style meeting with the minister responsible, I offered to build a tool to help people like me figure out these kinds of docs... I mean, it's basically just RAG. Nobody took me up on it, but not long afterwards,the good folks at Hugging Face 🤗 and Gradio came along with the Build Small Hackathon (and precious credits 😋).

So yeah, I built it... a tool for regular people to drop in a piece of legislation and get a summary with explanations of how it affects us. It also allows you to ask follow-up questions.



So the idea for Legislation Explainer was simple: what if a normal person could drop in a bill and quickly get a readable first-pass explanation of what it says, who it affects, what obligations it creates, and what questions they should be asking next?

Not an AI lawyer. Not a replacement for policy experts. Just a small-model assistant that makes the first reading less painful.

## The First Version Was Really About Shape

The earliest commits were the obvious scaffolding work: project config, a Gradio app, ingestion for PDFs/DOCX/text/URLs, and a RAG pipeline. That sounds neat in hindsight, but the most important decision was not the model or the vector store. It was the shape of the output.

I did not want a chatbot that says, "Ask me anything about this bill." That is too vague. If the user does not already understand the bill, they may not even know what to ask.

So the app generates a structured brief first:

- an executive summary
- a plain-language bill summary
- implementation implications
- critique and recommendations
- SWOT analysis
- follow-up Q&A

That structure matters. It gives the user handles. A journalist can scan the critique. A founder can jump to obligations and risks. A student can read the summary first. A civic group can use the SWOT as a discussion starter.

One thing I learned here is that for public-interest tools, "summarize this" is usually not enough. The real product is the set of questions you force the system to answer.

## The Demo Had To Be Instant

At first, the app leaned more heavily on live document fetching and retrieval. That is fine when you are developing locally with patience and good internet. It is less fine when you are trying to demo a hackathon app and the public PDF source decides to be slow, unavailable, or just weird.

The commit history tells the story pretty clearly. I added bundled example bills, then precomputed analysis assets, then rebuilt the precomputed retrieval path so it no longer depended on committed FAISS indexes.

That last part was a funny little lesson.

Hugging Face rejected a push because the repo contained binary FAISS index files. The files were not even huge. The issue was the storage policy around binaries. So I changed the app to keep the useful human-readable artifacts in Git (`analysis.json`, `chunks.json`, `document.txt`, `metadata.json`) and rebuild retrieval from chunks when needed.

That was a better architecture anyway.

It made the demo more reliable, kept the repository cleaner, and forced me to separate what needed to be precomputed from what could be rebuilt. The app became less magical and more inspectable. I like that.

## Streaming Was Not As Simple As I Wanted

At some point I wanted the app to feel more alive.

The obvious request was: can the executive summary and bill summary stream into the page as the model writes them? In normal chat apps, streaming is straightforward enough. In this app, the main analysis flow depends on structured outputs. The model is not just writing a paragraph; it is producing content that has to be parsed, validated, and rendered into sections and tables.

That led to a useful distinction:

1. UI-only streaming after a completed section returns.
2. True provider token streaming while the model is still generating.

For the Q&A flow, I could make the answer appear incrementally in the chatbot without changing the entire architecture. For the structured analysis, true streaming would have meant splitting some sections into plain-text calls, streaming those, then separately validating the final structured object.

I did not take that whole detour during the hackathon. Instead, I made smaller UX improvements: better progress states, rerun controls, source resets, queued question handling, stop buttons, and responsive layouts.

This was one of those moments where the boring product work mattered more than the shiny technical ambition. A stop button can be more useful than a clever stream.

## The Model Stack Became A Product Constraint

The hackathon was Build Small, so the model constraint was not a footnote. It shaped the project.

The shipped path uses `Qwen/Qwen3-14B` through the Hugging Face router, with `sentence-transformers/all-MiniLM-L6-v2` for local embeddings. I also explored provider flexibility and even a Modal/Nemotron direction, but the repo eventually came back to the simpler Qwen path.

That reversal was a good lesson.

There is always a temptation to make the app more flexible: bring your own provider, more model choices, more infrastructure options, more switches. But for a hackathon demo, flexibility can become noise. The user does not care that the app can theoretically talk to five providers if the one path they need is not reliable.

So the public app became more opinionated:

- one primary generation path
- clear model disclosure
- examples that load quickly
- Q&A grounded in the full document
- no public-facing provider settings that distract from the civic use case

Small models make you choose. That is annoying, but it is also healthy.

## Codex Was Good At The Churn

A lot of the progress on this project came from very practical Codex sessions: creating commits in sensible order, pushing to GitHub, connecting the Space remote, cleaning rewritten history, explaining why a Hugging Face push failed, and testing the UI in Chrome.

The best use of Codex was not "build the whole thing while I disappear." It was more like having a fast pair programmer for the churny middle of the project.

The chats show the project getting corrected in small ways:

- rename the project so it is about legislation, not just one bill
- hide future provider settings from the public app
- make supporting snippets collapsible
- answer follow-up questions from the full document, not only the summary
- move buttons around until the UI felt less awkward
- remove binary artifacts from history before pushing to the Space

None of those steps is glamorous. Together, they are the difference between a prototype and something I can actually show someone.

## What I Learned

The biggest lesson is that civic AI tools need humility.

A model can help summarize a bill, but the dangerous part is pretending that summarization is understanding. Legislation is about power, obligations, money, enforcement, loopholes, and the people who will be affected when the document stops being a PDF and starts being policy.

So the app has to keep inviting questions. It has to show sources. It has to make the user more curious, not more passive.

I also learned that demo reliability is a feature. Precomputed examples, clean deployment metadata, no binary surprises in Git, and a UI that can be visually verified are not side quests. They are part of the product.

If I keep working on Legislation Explainer, I would improve citation quality, add better clause-level source linking, test it with more civic groups, and maybe bring back audio summaries in a careful way. I still like the idea of letting someone listen to a plain-language brief while commuting.

For now, I am happy with the lesson: sometimes the most useful AI app is not the one that answers everything. It is the one that helps people start reading what already affects them.

And for public legislation, that feels like a pretty good place to start. ✌🏾
