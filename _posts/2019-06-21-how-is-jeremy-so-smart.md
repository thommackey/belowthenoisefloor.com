---
title: "How does Jeremy write such smart code?"
excerpt: "He gets a lot of it right the first time, but he definitely doesn't know everything off the top of his head — he’s just faster at solving problems."
---

**BLUF**: Don’t think Jeremy[^0] is “smarter” than you. He’s just faster at solving the same problems you have.

---

[^0]: Hopefully obviously, it's not just Jeremy. This is true of all experienced & productive practitioners (programmers, woodworkers, cooks, writers, artists…) I know.

The other night, someone asked me what I learned doing the in-person [fast.ai](https://course.fast.ai) course that I wouldn’t have picked up doing the MOOC. One thing jumped to mind - the chance to see that Jeremy[^1] is, in fact, mortal.

[^1]: For context, [Jeremy Howard](https://www.fast.ai/about/#jeremy) is a founding researcher and instructor of fast.ai. He's an all-around smart bloke and has a pragmatic, non-academic approach to a very complex, academic field, yet still produces a lot of [successful software](https://github.com/fastai/fastai) and truly [meaningful results](http://nlp.fast.ai/classification/2018/05/15/introducting-ulmfit.html). It's fair to say that most fast.ai students look up to him — some of them in downright awe.

During the in-person course, a study group met every day at USF to work through the material. Most days, Jeremy is there too, not for "office hours", but to work on the course and the library. So, we get to see (and help) him work on new features, model architectures, and the like.

Now, the fastai library’s [API](https://docs.fast.ai/tutorials.html) is very expressive and makes it super easy to get started building and training a deep learning model. Under the hood, though, it uses relatively complex software engineering techniques like callbacks, function composition, and exception-based flow control, all wrapped in a pragmatic-but-terse naming convention. To a student or developer looking to understand the guts of it, it’s a pretty intimidating beast.

When working in detail with the library[^2], it doesn’t take long for you to get disoriented. You forget what the names of various functions are, which param is meant to be a closure, which specific function actually ends up executing that param in your `**kwargs`… it’s just too much to keep in your head all at once.

[^2]: Or, indeed, any moderately complex software library.

You start making mistakes. Cells don’t run, functions error out, models don’t seem to be using that new optimiser parameter you’re sure you set correctly… what’s going on?

At best, you’re mentally exhausted, slightly frustrated, and struggling to keep track. At worst, you start convincing yourself you’re just an idiot who’ll never understand what’s going on, everyone else is smarter than you, and there’s no way you’ll get that job as a deep learning engineer at OpenAI if anyone sees how hard you’re finding this.

Well, the key learning from watching Jeremy himself develop the fastai codebase in person is that *he doesn’t remember all this stuff either!* Just like the rest of us, when he’s building new functionality, he gets function names wrong, makes typos, multiplies the wrong matrices together, uses the wrong type of arguments, and has to go digging into the codebase to understand just where that dictionary is being unrolled. Really! It's true!

I mean, sure, maybe he makes slightly fewer mistakes than you or I, but he definitely does make mistakes. The difference I observed between him and the rest of us is that he *expects* this to happen. When it does, rather than getting frustrated and despondent, he simply brute-forces his way through it. When a cell fails to run, he:

1. Guesses, 
2. `ctrl-F`s, 
3. `ack`s source dirs, 
4. googles, and 
5. guesses again 

for the answer — in roughly that order. Just by following that micro-process, he almost always solves any hiccup in 5 minutes at most. If it’s not working after that long, he’ll outsource it - to his colleague Sylvain, a particularly relevant forum, or twitter - and start working on a different problem while that answer comes back to him. Either way, he’s on to the next thing. 

I think, more than sheer intellect, it’s this kind of relentless focus and refusal to get caught up in self-reflection of “why can’t I get this to work” that really makes the difference. Yes, of course, Jeremy is incredibly smart. But there are lots of smart people — including you! The difference is in the *approach*.

Instead of wishing you were as *smart* as Jeremy, maybe focus on being as *effective* as Jeremy.