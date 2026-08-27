# AI Town Hall Speaker Notes

## What’s earned my trust recently?

Probably repeatability more than anything.

I’ve seen plenty of really impressive demos, but what changes my mind is when I can take an approach, use it over and over, and it keeps working. Once I’ve run something enough times that I have a feel for where it breaks, I’m much more comfortable relying on it.

That’s also how I look at new frameworks and workflows. I usually want to pull them apart a little bit and see what they’re actually doing.

If somebody shows me a framework, I’m asking: what did this add? Did the model get better context? A useful tool? A real check somewhere? Did breaking the work into steps actually improve the result?

Sometimes the answer is yes. Sometimes you peel it back and there’s a lot of prompting, personas, Markdown, and process around something the model was already pretty capable of doing.

That’s where I try to be careful about hype. A complicated system can look really sophisticated, but I still want to know what part of it is actually doing the work.

Take something like generating an ADR. I can build a huge process around that. I can have an architect persona, multiple stages, several intermediate artifacts, a giant set of instructions. Or I can give the model our ADR template, the relevant code, our standards, and the actual decision we’re trying to make.

Then I can compare the results.

Maybe the extra machinery helps. Great. Then I know why I want it. I just don’t want complexity to get a free pass because it looks like a mature process.

### How much leash can I give it?

This is probably the mental model I use the most.

I’m always playing with how much leash I can give the model.

If I hold it too tightly, I can get very safe, controlled output, but I’m also doing a lot of the work myself. If I’m spelling out every step, every artifact, every decision, every bit of reasoning, eventually I’m just managing the model instead of getting much leverage from it.

If I let it run too far, especially without enough context or a way to check itself, then yeah, I can get slop.

So I keep moving that line around.

How much can I hand off today? What can I stop specifying? What can the model figure out on its own now that maybe it couldn’t six months ago?

The leash shouldn’t stay the same length forever.

One of the questions I keep coming back to is: **do I still need this?**

Maybe I added a very explicit planning step because the model used to fall apart without one. Fine. Keep it while it’s helping. But a few model generations later, I want to try taking it out.

Same thing with a persona, an intermediate artifact, a giant instruction file, whatever it is. Take it away. Shorten the prompt. Give the model a little more room.

If the quality stays the same, cool. The workflow got simpler.

If the quality gets worse, that’s useful too. Now I’ve found a real constraint. I know that piece of scaffolding is buying me something, and I can engineer around it deliberately instead of keeping it around because a framework told me to.

That’s probably my biggest concern with one-size-fits-all frameworks. The structure might make total sense when it gets created. The danger is that it just stays there.

These models move really quickly. A workaround that was necessary six months ago can turn into ceremony without anybody noticing.

And the more layers you have, the harder it gets to tell what’s helping. You can have a framework that makes a weaker model look great, but now there are twenty layers around it and it’s hard to tell which five matter, which ten do nothing, and which five are actually hurting because they’re filling context or pushing the model down some rigid path.

So I like the loop of: I see a failure, I add something to address it, I make sure it actually helped, and then every once in a while I try taking it back out.

That feels healthier to me than deciding we found the right framework and freezing there.

### Context matters more than people sometimes realize

The models already know a lot. Most of the time, what they’re missing is our situation.

They don’t know the decision we made six months ago. They don’t know the weird constraint in this codebase. They don’t know why this team intentionally chose one pattern over another. They don’t know what just failed unless that information is available to them.

So when something goes wrong, one of my first questions is whether the model actually had the context it needed.

And this is also part of token stewardship for me. Context is finite. I’d rather spend it on the stuff the model genuinely could not know than on pages of generic instructions it probably doesn’t need.

If something can be enforced deterministically, I’d rather do that too. If Prettier can handle formatting, let Prettier handle it. If ESLint can catch a rule, use ESLint. If a test can tell the model whether it broke something, that’s better than another paragraph saying, “please be careful.”

That leaves the model’s attention for the parts where judgment actually matters.

I think that’s where my trust has come from. I’ve used these systems enough that I have a better feel for what I can hand off, where I need a check, and where I’m probably asking the model to do something the wrong way.

If I had to boil it down to one sentence:

> I trust a system a lot more once I understand how it fails.

---

## What would you say to someone who’s hesitant to experiment because they don’t want to look foolish if it doesn’t work?

I think the pressure around being “AI-native” can make this harder than it needs to be.

If every experiment feels like it has to turn into a success story, of course people are going to play it safe. Nobody wants to try a new workflow, have it be a mess, and feel like they’ve demonstrated that they’re bad at this.

But experimentation is how you get calibrated.

A lot of the way I’ve learned has just been trying different ways of working and seeing what happens. Maybe I give the model more autonomy and the output gets worse. Maybe I add a planning step and it helps. Maybe three months later I realize that planning step isn’t doing anything anymore and I remove it.

That’s the process.

I wouldn’t judge an experiment by whether the first version worked. I’d judge it by whether I learned something useful about how to structure the work.

And I’d start with real work, but keep the blast radius small.

Take something where you already have a decent sense of what good looks like. Try a different way of working with the model. Maybe you let it investigate more before you step in. Maybe you give it a larger piece of the implementation. Maybe you try a planning flow you haven’t used before. Then compare that against how you normally work.

Did it save time? Did it make the result better? Did it create more work than it saved? Where did you have to intervene?

If it was useful, keep going. If it wasn’t, change it or throw it away.

That’s also where the leash metaphor comes back in. Experimentation is how you find the right leash length. Give the model a little more room and see what happens. Sometimes you discover you can delegate way more than you thought. Sometimes you find a boundary. Both are useful.

I also think we should be comfortable with people trying five things and deciding three of them were bad ideas.

If the other two save them hours, that’s a good outcome.

What I don’t want is somebody forcing AI into all five because they feel pressure to show adoption. That doesn’t make them more AI-native. It just makes it harder to tell where the tool is genuinely helping.

So I’d tell somebody: keep experimenting, keep your own judgment in the loop, and don’t feel like every experiment has to earn a victory lap.

The point isn’t to have every experiment succeed. The point is to get faster at figuring out what’s worth keeping.

---

## Shorter version if I need to answer quickly

I think a lot of this comes down to understanding what the model can actually do and then constantly testing how much freedom I can give it.

I use the leash metaphor a lot. Too tight and I’m doing too much of the work myself. Too loose and I get slop. So I’m always trying to move that boundary and see how far I can push it.

The question I keep asking is, “Do I still need this?” If I added a planning step, a persona, an artifact, or some giant instruction file because the model needed it, fine. But I want to come back later and try removing it. If nothing gets worse, get rid of it. If the model falls apart, now I’ve found a real constraint.

That’s also how I look at frameworks. I want to know which parts are actually helping and which parts are just inherited structure.

The models already know a lot. Usually what they’re missing is our context: our codebase, our decisions, our constraints, what happened five minutes ago. That’s where I want to spend context, not on ceremony.

And for somebody who’s nervous about experimenting: I’d try to take the pressure off the individual experiment. Try a different workflow on real work, keep the blast radius small, and see what you learn. If it doesn’t help, throw it away. If it does, push a little farther next time.

The point isn’t to have every experiment succeed. It’s to get faster at figuring out what’s worth keeping.

## Lines I may want to use

- I trust a system a lot more once I understand how it fails.
- I’m always playing with how much leash I can give the model.
- The leash shouldn’t stay the same length forever.
- The question I keep coming back to is: do I still need this?
- A workaround that was necessary six months ago can turn into ceremony without anybody noticing.
- I want to know what part of the framework is actually doing the work.
- I see a failure, I add something to address it, and later I try taking it back out.
- The models already know a lot. What they usually don’t know is our situation.
- If Prettier can do it, I don’t need to spend tokens asking the model to remember it.
- Experimentation is how you find the right leash length.
- I’m fine with trying five things and throwing three away if the other two are genuinely useful.
- The point isn’t to have every experiment succeed. It’s to get faster at figuring out what’s worth keeping.
