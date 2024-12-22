*Almost the same could be told for Unity or any other complex engine/tool*

> [!info] TLDR
> I *(and most experienced people on the Unreal Source Discord server)* recommend not using any LLMs or AI assistants, **especially if you're a beginner**.
> - LLMs are bad at C++ and even more at Unreal Engine methods and systems.
> - LLMs are wrong more often than not.
> - LLMs answer with complete confidence even when wrong.
> - If you're new you likely don't known enough to know when answers are good or wrong.

---

> [!Warning] Please read before going further
> I will mostly mention ChatGPT to keep the paragraphs short, but please keep in mind that I am talking about any LLMs.

Regarding UE, ChatGPT isn't recommended, but if you really want to use it for starters, it isn't recommended to use it once you know the basics of UE.

**Why ?**<br>
Well any human can read the source code of UE, where there is everything you need, but AI models can't be trained on it because the source code is "private" (you need to create an Epic and GitHub account, link them and agree to the Unreal Engine EULA).<br>
This means that ChatGPT trained itself on publicly available sources, which are only the Official Docs, forums such as the official one and Reddit, blogs, and maybe YouTube video transcripts.
It can sometimes have some source code snippets that users copied from the source, but that's it.

The biggest issue is that forums posts holds the vast majority of data, and the most used forums publicly available are the official one and Reddit. This would mean that a big % of the answers ChatGPT gives you when asking a UE question comes from posts and comments user wrote on those forums.<br>
And this leads to an issue, forums posts are written by a lot of different kind of people, people asking, people answering right and people saying bad stuff. ChatGPT can hardly tell what is a good or bad answer, also, a lot of posts are old (between 2015-2020), which means some of the good answers can be irrelevant now because the methods are deprecated or changed.

A good example that shows that ChatGPT gives bad answers is [this simple question](https://chatgpt.com/share/6735c149-4e30-8000-8b07-8c836247955a).<br>
Also, people that read a lot of forums posts on any engine can confirm that maybe 20% to 40% of answers are not correct or ideal. This helps you imagine how wrong any AI trained on that data can be.

Even locally trained AI such as Github Copilot or Jetbrain's AI Assistant are not "good enough" in most cases for experienced people in the engine. They are usually good for single line completion (and in some specific cases good for a few more lines).

# Conclusion
Try to not use ChatGPT or any other LLMs AI **on complexes engines/tools such as Unreal Engine**, some can be less worse than ChatGPT like Perplexity because it gives you the used sources. So you can at least check the context of the answer.

**But please learn by yourself!** There is no better way to learn and improve, it's okay to be slow or to not do the correct thing the first times.<br>
It is very important to learn doing research's on the internet (or elsewhere) by yourself to find answers, this makes you better at finding how to express your thoughts, find keywords and improve your criticism.

> [!quote] Joviex
> *"Mastery of a foundational skill precedes effective use of advanced tools."*

And there is a ton of friendly humans that can answer your questions with more knowledge and correctness that any LLMs could ever have *(for now)*, find them in the correct forums or communities (shout-out to the [Unreal Source Discord](https://discord.gg/unrealsource) and [Unity Official Discord](https://discord.gg/unity)).

# More on the subject
**Disclaimer!**
I am not an expert in this domain, please be cautious when reading the listed links. Before writing this I mostly used trustworthy articles and my personal (and others) experienes.
- https://en.wikipedia.org/wiki/Generative_pre-trained_transformer
- https://torstenvolk.medium.com/chatgpt-is-revolutionary-but-it-calculates-one-word-at-a-time-694bc2c951ed
- https://community.openai.com/t/chatgpt-cannot-count-words-or-produce-word-count-limited-text/47380/16
- https://help.openai.com/en/articles/6681258-doing-math-with-openai-models
- https://www.youtube.com/watch?v=wjZofJX0v4M


