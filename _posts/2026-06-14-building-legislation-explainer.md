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

<img alt="screenshot" width="400" src="https://github.com/user-attachments/assets/066210b4-05bf-4dc4-9ea2-5679912c53d0" />

## Version 1 - Qwen3-32B + Structure

The earliest commits were the obvious scaffolding work: project config, a Gradio app, ingestion for PDFs/DOCX/text/URLs, and a RAG pipeline using the largest Qwen model that barely met the hackathon requirements of staying under 32B parameters.

The structured brief was set up as follows:

- an executive summary
- a plain-language bill summary
- implementation implications
- critique and recommendations
- SWOT analysis

And after the summary, an attached chat interface for follow-up Q&A

Simple and sweet... until I put it out and found out some people didn't care for the summary, they just wanted to ask questions, and that broke the flow. 🤦🏾‍♂️

## Version 2 - Qwen3-14B + Embeddings

So it turns out small models are really capable, and I could have gone way lower than 14B, but since it's a legal government doc and I wanted to preserve reasoning, I dropped to Qwen3-14B and stuck there for a balance between size and capability.

The app leaned on full document scans at this point, which is great for context, but can get slow, so we had to go the traditional embeddings chunking route. Enter sentence-transformers/all-MiniLM-L6-v2 and FAISS for indexing, and a bunch of git errors because the repo contained binary FAISS index files. The files were not even huge 😒. The issue is GitHub's storage policy around binaries. So we (Codex and I) changed the app to keep the useful human-readable artifacts in Git (`analysis.json`, `chunks.json`, `document.txt`, `metadata.json`) and rebuild retrieval from chunks when needed.

That was a better architecture anyway.

It made the demo more reliable, kept the repository cleaner, and forced me to separate what needed to be precomputed from what could be rebuilt. The app became less magical and more inspectable. I like that.

## Version 3 - Streaming Text can be tough

At this point the app felt laggy, if that's a word, so I wanted the thing where the model writes to the screen one character at a time. You know, that typewriter effect.

The obvious request was: can the executive summary and bill summary stream into the page as the model writes them? In normal chat apps, streaming is straightforward enough. In this app, the main analysis flow depends on structured outputs. The model is not just writing a paragraph; it is producing content that has to be parsed, validated, and rendered into sections and tables.

That led to a useful distinction:

1. UI-only streaming after a completed section returns.
2. True provider token streaming while the model is still generating.

For the Q&A flow, I could make the answer appear incrementally in the chatbot without changing the entire architecture. For the structured analysis, true streaming would have meant splitting some sections into plain-text calls, streaming those, then separately validating the final structured object.

I could see the headache coming a mile away... swerve! Not worth the headache for an MVP 😅. I focused my energy on smaller UX improvements which added up to better gains overall: better progress states, rerun controls, source resets, queued question handling, stop buttons, and responsive layouts.

This was one of those moments where the boring product work mattered more than the shiny technical ambition. A stop button can be more useful than a clever stream.

Oh, this is also where we decoupled the Q&A chat from the summary, so that one didn't have to depend on the other.

## Model Constraints - Keep it Simple

The hackathon was Build Small, so the model constraint shaped the project.

There is always a temptation to make the app more flexible: bring your own provider, more model choices, more infrastructure options, more switches. But for a hackathon demo, flexibility usually becomes noise. We need a good demo, and nothing more if we can help it, so the public app became more opinionated:

- one primary generation path
- clear model disclosure
- examples that load quickly
- Q&A grounded in the full document
- totally dropped a bring-your-own-model idea I had been toying with

Small models make you choose. It';'s like finding a spouse... you choose just one 😂.

## Codex - My New Friend

This was my first time using Codex extensively. Connecting from my phone for remote sessions was such a productivity hack! I learnt from a video once that I could set things up so that the agent would work and verify its output with screenshots before I check, feature by feature. If the screenshot looks wrong, I send Codex back to work it out before I bother to spend my precious time reviewing. I think this is my new workflow for side gigs moving forward 🙌🏾.

A lot of the progress on this project came from very practical Codex sessions: creating commits in sensible order, pushing to GitHub, connecting the Space remote, cleaning rewritten history, explaining why something failed, and testing the UI in Chrome.

The future really is agentic!

## Takeaways

1. this is non-technical, but just from feedback, people will rally around a solution that addresses real pain points.
2. Small models can do a lot! Not every problem requires an OpenAI, Anthropic or Google solution.
3. The different inference providers in HuggingFace come with diffrent perks... some are cheaper, some are faster, but they are there! Before starting on this, I had not realised the variety that was available.
4. Various git platforms can get iffy over binaries. Read docs!
5. UIs that can be visually verified by an agent with a browser tool is how to develop for the frontend moving forward.

I had one suggestion to add an audio naration so people can listen to a plain-language brief while commuting. Sounds cool doesn't it? I just didn't have the time, unfortunately.

For now, I am happy with the lessons and the progress... the most useful AI app is not the one that answers everything, in this case, it is the one that helps people start reading what already affects them.

And for public legislation, that feels like a pretty good place to start.  
✌🏾😎

App available here: https://huggingface.co/spaces/build-small-hackathon/legislation-explainer

Demo video here:
<div style="position: relative; padding-bottom: 64.98194945848375%; height: 0;"><iframe src="https://www.loom.com/embed/ed7b682ab6774d5499fca4c06c290bfc" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe></div>
