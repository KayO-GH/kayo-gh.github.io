---
layout: post
title: "AI Can't Show You The Time... A Lesson in Bias"
date: 2026-01-17
comments: true
---


#### The Problem
Apparently this is a thing that a lot of people already knew, but I heard it for the first time in August last year, on Day 1 of the Deep Learning Indaba 2025 as a passing comment: generative AI models cannot (almost 100%) produce images of analogue clock faces with any time other than 10 past 10.

This is strictly an image problem, which is why I used to word "show", not "tell" in the title of this article.

Speaking of show and tell, let me just show you an example of what I mean from everybody's favourite PhD-level thinker, ChatGPT.

*Now, [ChatGPT does not allow us to embed shared chats](https://www.reddit.com/r/OpenAI/comments/101cb7e/anyone_figured_out_how_to_put_chatopenaicom/), so you can either follow along with the screenshots below, or look at tht chat [here](https://chatgpt.com/share/68a1fde5-669c-8007-aeca-684bbebd3510). The screenshot route is better if I say so myself.* 😅

So let's begin. Simple first ask:  
<img src="https://github.com/user-attachments/assets/b4fcfd55-43c1-432f-9456-b86b6f195529" alt="WhatsApp Image 2026-01-17 at 08 59 21" width="400px" />

Well done, ChatGPT.  
<img src="https://github.com/user-attachments/assets/6072b4c8-e75d-44ad-b045-57afaaec6e60" alt="clapping-congrats" width="200px" />

On to my simple second ask:  
<img src="https://github.com/user-attachments/assets/b1668883-d0fe-4332-8587-206924edb33b" alt="WhatsApp Image 2026-01-17 at 08 59 22" width="400px" />

Wait, what?  
<img src="https://github.com/user-attachments/assets/c5741c29-f753-4f82-8040-37c5903d3a8f" alt="shock" width="200px" />

Come on. It's not that hard. Let me spell it out for you...  
<img src="https://github.com/user-attachments/assets/fe2bbfa2-3109-4f2e-901c-0ce3b135318d" alt="WhatsApp Image 2026-01-17 at 08 59 22 (1)" width="400px" />

At this point, despite being adequately warned, I couldn't help but feel that ChatGPT was being an idiot. I knew why (and we will discuss the 'why' shortly) it was behaving this way, but as far as feelings are concerned, all I felt was contempt for the performance of this billion-dollar oaf.

And so I switched my approach. Surely, ChatGPT should be able to draw the correlation between digital and analogue time. I tried to give our beloved ChatGPT one more shot...  
<img src="https://github.com/user-attachments/assets/1cb71f1d-e8d7-45ed-af45-aaec898184c8" alt="WhatsApp Image 2026-01-17 at 08 59 22 (2)" width="400px" />
<img src="https://github.com/user-attachments/assets/d4a6ec42-c723-41cd-8017-4836c29adc7a" alt="WhatsApp Image 2026-01-17 at 08 59 23" width="400px" />

Yeah... Nope!  
<img src="https://github.com/user-attachments/assets/c28627a3-cab9-4bfa-a85a-59560ea4ed15" alt="giphy" width="300px" />

---
#### How We Got Here

So how exactly did we get here? Well, it turns out machine learning models (and LLMs by extension) are basically pattern-matching machines on steroids. They learn by ingesting massive datasets of images, text, whatever—and then reproducing what they've seen most often. The catch? They don't actually *understand* anything. When you ask an AI model to draw a clock, it's not thinking "okay, the hour hand goes here and the minute hand goes there." Nope. It's just regurgitating what it saw during training.

And here's the kicker: if you look at stock photos, watch ads, or basically any marketing material featuring clocks, they're almost always set to 10:10. This is a fundamental marketing gimmick: the 10:10 position creates a nice symmetrical "smile" with the hands, frames the brand logo perfectly, and doesn't cover up any important details on the watch face. While this is aesthetically pleasing, when it becomes 99% of what AI models see during training, you've got a problem.

So what happens when you ask ChatGPT to generate a clock showing literally any other time? It trips over itself. Because in its "brain" (and I use that term *very* loosely), "clock face" equals "10:10." The model hasn't been malicious or stupid—it's just doing exactly what we trained it to do: find patterns and reproduce them. And when one pattern dominates the dataset, that's what you get.

Makes sense? Let's continue.


#### The Bias Implications
Now, our example is a quirky clock problem, but the implications go way beyond that; it's a toy example of how bias works in AI systems across the board. Whether it's race, gender, language, or culture—if your training data is skewed, your model will be skewed. The bias isn't intentional; it's mathematical. Genrative AI is holding up a mirror to humanity, and this could play out in serious ways downstream.

##### Racism, Sexism, and Other 'isms'
Moving away from clocks, remember when Google's Gemini famously overcorrected for diversity so hard it started generating historically inaccurate images... like a black pope 😅. This was an over-exertion of efforts to move away from the typical stereotypes that have plagued and characterized history: white male CEOs, female nurses and male doctors, dark-skinned criminals.

While sad, it's fundamentally an issue of representation. AI learns from examples, so until we shift the representations present in our data examples, AI will continue to produce what it sees: the good, the bad, and the ugly.

*Here's an interesting question with no right answer (I'm curious to hear your thoughts in the comments though): would you be interested in seeing more height diversity in the NBA? I mean, would you enjoy seeing much shorter players? Would you care at all? The distribution of heights is obviously skewed, and if you care to see the global height distribution, here's [a link](https://ourworldindata.org/human-height#height-is-normally-distributed) for you to nerd out.*

Beyond images, AI systems are being used in courts of law for tasks like determining bail amounts, and even influencing sentencing. If those systems were trained on biased data (and they likely were), then they're baking historical prejudices directly into "objective" legal decisions. A person's freedom might hinge on an algorithm that learned from a world that was already unfair. The shift from viewing this as just "interesting" to "unethical" and "absurd" may depend on which side of a jury or defendant's bench you sit on.

Then there's the creative side. As generative AI becomes the go-to tool for content creation, we're staring down the barrel of the death of originality.... at least, as originality is generally defined today. If every AI model is trained on the same corpus of data—largely dominated by Western, English-speaking, male perspectives—then every output starts to look the same. The quirks, the cultural nuances, the *weirdness* that makes human creativity interesting is sucked into an internet data smoothie of "what worked before."

The clock problem is funny. The rest? Not so much. This isn't science fiction; it's the reality of 2026.

<!--
#### The language Implications
- emdashes
- delving into things with Paul Graham

#### Humanity's Advantage
- Sensory linkages (https://youtube.com/shorts/hvv3lnseVY4?si=48N5OeTP2HKjYMXb). 
  - Taste
  - Feeling -->
