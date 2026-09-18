---
title: "How I AI in the EU :eu:"
excerpt_separator: "<!--more-->"
categories:
  - Software
tags:
  - AI
  - LLMs
  - Harness
  - Cloud Agent
  - Open Models
  - Europe
  - Sovereignty
---

In the past few years, AI has taken over coding. At least in my field, which is business software on the web. Whether we like it or not, coding by hand is just not competitive anymore, and carrying on as before is very likely a career stopper, or business stopper [in my case](https://cobot.me).

I have come to very much enjoy this new developer reality. From starting with LLM-enhanced auto-complete in VS Code, later using chat to get bigger and bigger tasks done, to today firing off a few cloud agents from my phone while on a weekend trip.

<!--more-->

## Oh Dear

But, there are huge issues. Most have to do with who brought all this upon us. Billionaires from Silicon Valley are financing companies that train their models on data they never acquired any rights to, in data centers powered by illegal gas turbines that pollute the air around them and contribute to the climate catastrophe. Not to mention the increasing concentration of power and wealth, in a country that has left the path of democracy.

At Cobot, we started using Cursor in mid-2025. It was the easiest way to get going, plenty of (US) models to choose from, plus cloud agents. It worked very well and quickly became part of how we build software.

{% include image.html url="/assets/images/elon_und_jeff_on_mars.webp" alt="Elon und Jeff on Mars comic cover" caption="Somewhat related. My favorite piece on Elon." href="https://www.carlsen.de/comics/elon-und-jeff-on-mars-comic" %}

Then Cursor got acquired by SpaceX. Our subscription money now goes to Elon Musk — something we need to change quickly.

The only well-known alternative, [Devin](https://devin.ai), has Peter Thiel as an investor. Also not an option.

So, what can we do in this wonderful world? How can we keep exploring and using the ever-increasing capabilities of AI, without giving Elon and Peter ever more money and power?

Unfortunately, when it comes to Europe and AI models, the only contender — Mistral — is not competitive at the moment. At least when it comes to coding models. I have an account, but I'm only using it for some autocomplete and for simpler non-coding tasks.

Who's left? China! I don't know why exactly Chinese labs are posting all these open-weight models for free — except to piss off the US. But as long as they are, we might as well use those.

## Local development

I have found various EU companies hosting these Chinese models in the EU. I can pay these companies, the money stays in the EU (I know, the hardware did not fall from the sky), and maybe hopefully some of that money will go towards the development of European AI models. Based on training data that was rightfully acquired, run in data centers powered by renewable energy, paying fair wages and all the rest of it.

My current setup is that I use [Zed](https://zed.dev) as my code editor, with [TensorX](https://tensorx.ai/) as my custom model provider. Since [DeepSeek 4.1 Flash](https://tensorx.ai/models/deepseek--deepseek-v4.1-flash/) came out, it has been my daily driver. At $1.50 per million output tokens, I don't even need to switch to anything cheaper for implementation or simpler tasks. And I don't need to keep fiddling with local AI. It's just not going to happen. At least not soon (500 GB of RAM, anyone?).

## Cloud agents

I want my cloud agents back! Being able to start a task from anywhere and review a (mostly, most of the time) complete solution whenever I'm back at a computer is so powerful.

I started working on a wrapper around [opencode](https://opencode.ai) that runs on my own server. It lets me (git) check out a repo from my phone, let a model work in opencode to make changes, and create a pull request. It works, but it's very basic.

{% include image.html url="/assets/photos/cloudbot.webp" alt="Screenshot of cloudbot - my cloud agent prototype" caption="My cloud coding agent prototype." %}

If anything, building it showed me what is actually needed to get to a full cloud harness: handling all sorts of git and GitHub cases (merging and rebasing, conflict handling, stacking PRs), running tools, MCP, being able to run tests along with the infrastructure they need, and so on. Much more work than I am willing to take on.

So I have been playing with [OpenHands](https://www.openhands.dev/), and that looks promising. It has a built-in web interface, can check out remote repos, run code in isolated containers, connect to MCP servers, etc. I don't have it installed remotely yet, but in a few weeks I should be ready to go.

## And now?

My local EU-only setup is already working for me. Once I have OpenHands running, I can migrate the remaining work as well.

After that I can move my dev team at Cobot onto EU infrastructure, and cancel the remaining Cursor subscriptions.

I know this is all temporary. We can't rely on China in the long term. I hope Europe will find a way to catch up (enough). With the speed at which models are improving, I don't think we even need to have top frontier models. Something in the range of the current flash models like GLM 5.3, Qwen 3.8, or DeepSeek 4.1 would be enough.

One can hope.
