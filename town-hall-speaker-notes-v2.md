# AI Town Hall Speaker Notes — V2

This version assumes roughly a couple of minutes per question and is written to be easy to read from live.

## 1. What’s earned my trust recently?

Probably repeatability.

I’ve seen a lot of impressive demos, but what actually earns my trust is being able to use an approach over and over and start to understand where it works and where it breaks.

A big part of that for me is not getting too attached to frameworks. I like to pull them apart and ask what they’re actually adding. Better context? A useful tool? A verification step? Some decomposition that genuinely improves the result? Great. Keep it.

But if there’s a lot of process around something the model already does pretty well, I want to know that too.

The mental model I use is basically a leash. I’m always testing how much freedom I can give the model.

If the leash is too tight, I’m doing too much of the work myself. If it’s too loose, I get slop. So I keep moving that boundary.

And the question I come back to constantly is: **do I still need this?**

Maybe I added a planning step because the model needed it six months ago. Fine. But I want to try removing it later. Same with personas, intermediate artifacts, giant instruction files, whatever it is.

If I remove something and quality stays the same, great. The workflow got simpler. If quality drops, then I’ve found a real constraint worth engineering around.

That’s the danger I see in one-size-fits-all frameworks. A workaround can become permanent ceremony, especially when the underlying models are changing this fast.

Context fits into that too. The models already know a lot. What they usually need from us is our situation — our codebase, our decisions, our constraints, what just happened. So I want to be deliberate about what context I’m giving them instead of piling on instructions by default.

And when something doesn’t need model judgment, I’d rather make it deterministic. If Prettier can enforce formatting, let Prettier do it. If a test can catch a bad change, give the workflow that check instead of another paragraph telling the model to be careful.

That’s really how I think about trust now. I’m not asking myself whether I trust the model in some broad sense. I’m asking whether I trust this particular way of working with it.

Have I used it enough to know where it tends to go wrong? Do I know what happens when it does? Are the right checks in place? Which pieces of scaffolding are actually helping, and which ones are just there because we started with them?

Once I can answer those questions, I’m a lot more comfortable giving the workflow more responsibility. And if I can’t answer them yet, that just tells me where I need more reps or a tighter boundary.

If I had to put it in one line: **I trust a system a lot more once I understand how it fails.**

---

## 2. What would you say to someone who’s hesitant to experiment because they don’t want to look foolish if it doesn’t work?

I think the pressure to be “AI-native” can actually make people more cautious.

If every experiment feels like it has to become a success story, people are going to play it safe. Nobody wants to try a new workflow, have it go badly, and feel like they just proved they don’t know what they’re doing.

But experimenting is how you get calibrated.

A lot of the way I’ve learned is just trying different ways of working and seeing what happens. Give the model more autonomy. Add a planning step. Remove one. Let it take a bigger piece of the implementation. See where you have to intervene.

I’d keep the blast radius small at first and use real work where you already have a decent sense of what good looks like.

Then ask simple questions afterward: did this save me time? Did the result get better? Did it create more work than it saved? Where did I have to step in?

If it helped, push a little farther next time. If it didn’t, change it or throw it away.

That’s how I think about the leash here too. Experimentation is how you find the right length. Sometimes you discover you can delegate way more than you thought. Sometimes you hit a boundary. Both are useful.

I’d also be completely fine with somebody trying five things and deciding three of them were bad ideas. If the other two save them hours, that’s a good outcome.

What I wouldn’t want is someone forcing AI into all five just because they feel pressure to show adoption.

The point isn’t to have every experiment succeed. The point is to get faster at figuring out what’s worth keeping.

## Pocket lines

- I’m always testing how much leash I can give the model.
- The question I keep coming back to is: do I still need this?
- A workaround can turn into ceremony if you never test whether it’s still necessary.
- These models already know a lot. Usually what they’re missing is our situation.
- I’m not asking whether I trust the model in the abstract. I’m asking whether I trust this way of working with it.
- I trust a system a lot more once I understand how it fails.
- Experimentation is how you find the right leash length.
- I’m fine with trying five things and throwing three away if the other two are genuinely useful.
- The point isn’t to have every experiment succeed. It’s to get faster at figuring out what’s worth keeping.
