# AI Town Hall Speaker Notes — V2

This version assumes roughly a couple of minutes per question and is written to be easy to read from live.

## 1. You said you try to separate hype from what’s real, and you’d rather be a steward of tokens than chase adoption metrics. What’s earned your trust recently?

I think, more generally, I trust approaches that lean on the model.

I’ve run hundreds of agentic sessions at this point, and one thing I’ve gotten more comfortable with is starting from the assumption that the model is pretty capable. I don’t need to build a huge process around it before I let it work. I can give it a lot of room, see where it struggles, and then fill in those gaps.

That’s probably the biggest thing that’s earned my trust lately: seeing how far I can go with the model itself before I need extra scaffolding.

Maybe it needs context about our codebase. Maybe it needs to know a decision we made six months ago. Maybe it needs a test or some other feedback so it can tell whether it broke something. Maybe a planning step really does help for a certain kind of task. Those are useful additions because I can point to the gap they’re filling.

What makes me skeptical is when the orchestration comes first and the model is buried underneath it.

So when I see a new framework, I tend to pull it apart. What is this actually adding? Better context? A useful tool? A check? Something that fixes a failure I’ve actually seen? Great. Keep it. But if it’s a lot of prompts, personas, artifacts, and process around something the model already handles well, then I start asking whether I need any of that.

That’s where the leash metaphor comes in for me. I’m always testing how much freedom I can give the model.

If I hold it too tightly, I’m leaving speed on the table because I’m still doing a lot of the orchestration myself. If I let it go too far and the output starts getting sloppy, then I’ve found a boundary.

And I keep coming back to: **do I still need this?**

If I added a planning step or an artifact because it helped six months ago, I want to try taking it back out later. If the workflow still holds up, great — I can simplify it and give the model a little more leash. If quality drops, now I know that piece is actually buying me something.

That’s the danger I see in one-size-fits-all frameworks. The scaffolding can stick around long after the reason for it is gone.

The token stewardship piece fits into the same idea. Context is limited, so I want to spend it on the gaps the model actually has. These models already know a lot. What they usually need from us is our situation — our code, our constraints, our history, whatever just happened in the environment.

And if something is deterministic, I’d rather make it deterministic. If Prettier can enforce formatting, use Prettier. If a test can catch a bad change, give the model the test instead of another paragraph telling it to be careful.

So, yeah, I think the things that earn my trust are the ones that are actually leaning on the model’s capability, not hiding it under a lot of machinery. Then I can add structure where I see a real need for it.

And once I understand where that approach fails, I’m much more comfortable relying on it.

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

- I trust approaches that lean on the model and add structure where the model actually needs help.
- I’ve gotten more comfortable starting from the assumption that the model is capable, then filling in the gaps.
- I’m always testing how much leash I can give the model.
- The question I keep coming back to is: do I still need this?
- A workaround can turn into ceremony if you never test whether it’s still necessary.
- These models already know a lot. Usually what they’re missing is our situation.
- I trust a system a lot more once I understand how it fails.
- Experimentation is how you find the right leash length.
- I’m fine with trying five things and throwing three away if the other two are genuinely useful.
- The point isn’t to have every experiment succeed. It’s to get faster at figuring out what’s worth keeping.
