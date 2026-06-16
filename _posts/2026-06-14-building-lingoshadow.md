---
layout: post
title: "Building LingoShadow"
date: 2026-06-14
comments: true
---

<img class="post-hero" width="1000" alt="Landscape screenshot of the LingoShadow Gradio app" src="/images/building-lingoshadow.jpg" />

# Building LingoShadow

I built version one of this app from the gym wiht my buddy, Codex. It's really a different world for software development now. 😅

Anyway, I had been watching videos about language learning, shadowing, and the idea that you can get surprisingly far by practicing the sentences you are actually likely to say. Not random textbook sentences... that's how we all learnt to speak as children, isn't it? We repeated the phrases we heard in the contexts we were growing up in.

So the first prompt to Codex was basically: let's make a Gradio app where I describe my normal life, choose a target language, and get useful sentences plus audio... and then I passed codex some of the links to the videos I had watched so it would figure out more context from the transcripts. With an agentic coding assistant, whenever possible, show, don't just tell!

The default language would be French, of course, since that's the only international language apart from English for which I can hope to catch a translation model in a lie. 😅

And that's how LingoShadow was born: a daily language practice app that turns your real routines into a small study pack.

## The Core Idea Was Not The Model

The first version was already fairly clear:

- ask the learner to describe their daily use cases
- generate target-language sentences around those situations
- include useful verbs
- create a 45-minute practice routine
- generate downloadable audio
- package the output so it can be used away from the app

That last part was important. I did not want a toy that only works while the browser tab is open. If the app is for practice, the output has to travel with you.

This is one of the biggest lessons from the project: for learning tools, the artifact matters.

A nice response on screen is fine. A ZIP file with MP3 tracks, a sentence table, verbs to review, and a routine is better. It turns the model output into something you can actually use on a walk, in a trotro, at the gym, or while pretending you are about to become fluent this week. 😄

## Audio Changed Everything

The first audio packaging idea was too granular: one sentence per audio file. That sounded organized, but it would have been annoying in real life. Who wants a folder full of tiny clips?

So the app moved to tracks of up to 20 sentences. Later, generated files switched from WAV to MP3 for better phone compatibility. That sounds like a boring file-format change, but it is exactly the kind of thing that decides whether a tool survives contact with real users.

Phones like MP3. People share MP3. Audio players understand MP3. So MP3 it is.

The audio work also led to a long chain of practical questions:

- should there be a break between sentences?
- how slow should shadowing audio be?
- should we slow down speech itself or just add padding?
- how much work should happen on Modal?
- can cold start begin when the page opens?
- should audio generation be split across workers?

This was where LingoShadow stopped being just an LLM wrapper.

The app needed a TTS service, backend routing, warmup behavior, progress feedback, and failure handling. The final stack routes target languages to different TTS models: Kyutai for English/French, MMS for German, and Kokoro for Spanish, Italian, Portuguese, and Japanese.

That is a lot more interesting than a dropdown. A language dropdown is just the user interface. The real product decision is what happens behind each option.

## Multilingual Is A Routing Problem

Early on, it would have been easy to pretend one model could do everything well enough. But language apps punish vague thinking.

Text generation, translation, and speech are different jobs. A model that is good at writing practice sentences is not automatically the best translation model. A TTS model that speaks French well may not be the right choice for Japanese. And if the user selects German, the app should not quietly fall back to a French voice and hope nobody notices.

So LingoShadow ended up with a small-model stack rather than a single magic model:

- `Qwen/Qwen3-8B` for sentence generation
- `CohereLabs/tiny-aya-global` for translation
- language-specific TTS backends for audio

Each individual model stays comfortably under the hackathon limit, and the app discloses the stack instead of hiding it.

One of the funniest little lessons was from the model stack copy itself. At one point there was a weird "Hugo the Frenchie" style artifact in the app copy, which led to the obvious question: where did that come from? The answer was basically: from placeholder/demo language being allowed to hang around too long.

Tiny copy mistakes are not tiny when they appear in a model disclosure section. They make the whole app feel less serious. So the model stack moved into the README, and the rendered app focused more on the learner experience.

Good call.

## The UI Had To Feel Like Learning

The first functional app worked, but it did not feel especially like a language learning app. Then came the bright-theme pass.

This is where the project became more itself: flags, a cheerful hero section, warmer colors, clearer cards, and a layout that made the app feel less like a form and more like a study studio.

I went back and forth on contrast a few times. Some labels looked fun but not readable enough. Some badges were too loud. Some sections had too much visual weight. The commit history has a whole little story of brightening the app, then fixing contrast, then restoring some badges, then polishing the layout again.

That taught me something I keep relearning: visual design is not decoration after the fact. It changes how understandable the workflow feels.

For LingoShadow, the UI needed to say:

- this is practical
- this is friendly
- this will give you something downloadable
- you do not need to understand the model stack to begin

That is why the first screen became the actual app experience, not a marketing page. Describe your world, choose your language, build the pack. Done.

## Progress Feedback Was Not Optional

Study-pack generation takes time. The app may be calling a generation model, translating, building audio, zipping files, and waiting on a remote TTS service. If the UI just sits there, people assume it is broken.

So progress feedback became a real feature:

- show that the build has started
- show sentence generation progress
- show audio packaging status
- keep the success state obvious
- add timestamped download names
- make the maximum sentence count sane for mobile use

The timestamped filenames were another tiny but useful change. A downloaded `daily_language_pack.zip` is fine once. After the third build, it becomes a mess. A file like `daily_language_pack_20260614_214438.zip` is boring in the best possible way.

Boring wins a lot in software.

## Codex Was Useful, But The User Feedback Mattered More

Looking through the Codex chats, the app improved most when the requests were concrete:

- "It should not be one sentence per audio file. I want 20 sentences per audio file."
- "The audio files should be MP3 for phones."
- "The app is slow on mobile."
- "The section titles do not have enough contrast."
- "I want a motivation button."
- "Any audio or ZIP file downloaded should end with a timestamp."

That is the good stuff.

A model can generate a lot of code, but the product direction came from noticing friction. Too many files. Wrong format. Slow mobile flow. Weak contrast. Confusing model disclosure. Each complaint made the app more real.

The motivation panel is a good example. Embedding the video directly would have been heavy and distracting. A small expandable explanation fit better. People who care can open it. Everyone else can keep building their study pack.

## What I Learned

The main lesson is that language learning apps should start from the learner's life, not from the model's capabilities.

The model can produce sentences all day. That is not the hard part. The hard part is making those sentences likely to matter tomorrow morning.

I also learned that audio is not an export option. In a shadowing app, audio is the product. The track length, file format, pacing, warmup, packaging, and download names all matter because they decide whether practice actually happens outside the browser.

And finally, I learned that small models are more than enough when the task is shaped properly. LingoShadow does not need a giant frontier model to ask about your day, generate practical phrases, translate them, and package audio drills. It needs a focused workflow and honest constraints.

If I keep going, I would add spaced repetition, learner feedback, better pronunciation support, and maybe a way to regenerate only the weak parts of a pack instead of rebuilding everything.

For now, the app does what I wanted at the start: it turns "I want to learn French for my actual life" into something I can listen to.

That feels small in the good way. ✌🏾
