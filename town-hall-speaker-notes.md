# AI Town Hall Speaker Notes

## What’s earned my trust recently?

Probably repeatability more than anything.

I’ve seen plenty of really impressive demos, but the thing that changes my mind is when I can take an approach, use it over and over again, and it keeps working. Once I’ve run something a bunch of times and I have a decent feel for where it breaks, I’m much more comfortable relying on it.

That’s also how I tend to look at new frameworks or workflows. I usually want to pull them apart a little bit and see what they’re actually doing.

If somebody shows me a framework, I’m asking things like: what did this add? Did it give the model context it didn’t have before? Did it give it a useful tool? Did it add a check somewhere? Did breaking the work into a couple of steps actually improve the result?

Sometimes the answer is yes, and that’s useful. Sometimes you peel it back and there’s a lot of prompting, personas, Markdown, and process around something the model was already pretty capable of doing.

That’s where I try to be careful about hype. A complicated system can look really sophisticated, but I still want to know what part of it is actually doing the work.

Take something like generating an ADR. I can build a pretty elaborate workflow around that. I can have a special architect persona, multiple stages, several generated artifacts, a bunch of instructions. Or I can give the model our ADR template, the relevant code, our standards, the actual decision we’re trying to make, and see what it does.

Then I can start asking whether all the extra machinery made it better. Maybe it did. If it did, great — now I know why I want it. But I don’t want complexity to get a free pass just because it looks like a mature process.

### How much leash can I give it?

This is probably the mental model I use the most.

I’m always playing with how much leash I can give the model.

If I hold it too tightly, I can get very safe, very controlled output, but I’m also doing a lot of the work myself. If I’m spelling out every step, every artifact, every decision, every bit of reasoning, at some point I’m just managing the model instead of getting much leverage from it.

If I let it run too far, especially without enough context or a way to check itself, then yeah, I can get slop.

So I’m constantly moving that line around.

How much can I hand off today? What can I stop specifying? What can the model figure out on its own now that maybe it couldn’t six months ago?

And I think that last part matters a lot. The leash shouldn’t stay the same length forever.

One of the questions I keep coming back to is: **do I still need this?**

Maybe I added a very explicit planning step because the model used to fall apart without one. Great. Keep it while it’s helping. But a few model generations later, I want to try taking it out and see what happens.

Same thing with a persona, an intermediate artifact, a giant instruction file, whatever it is. Take it away. Shorten the prompt. Give the model a little more room.

If the quality stays the same, cool — I can simplify the workflow.

If the quality gets worse, that’s actually useful too. Now I’ve found a real constraint. I know that piece of scaffolding is buying me something, and I can engineer around that deliberately instead of keeping it around because a framework told me to.

That’s probably my biggest concern with one-size-fits-all frameworks. The structure might be totally reasonable when it gets created. The danger is that it just stays there.

These models are changing really quickly. A workaround that was necessary six months ago can turn into ceremony without anybody noticing.

And the more layers you have, the harder it gets to tell what is actually helping. You can have a framework that makes a weaker model look great, but now you’ve got twenty layers around the model and it’s hard to tell which five matter, which ten do nothing, and which five are actually hurting because they’re filling up context or pushing the model down some overly rigid path.

So I like the loop of: I see a failure, I add something to address it, I make sure that actually helped, and then every once in a while I try taking it back out.

That feels healthier to me than deciding we found the right framework and freezing there.

### Give the model things it can actually work with

I’ve also started thinking less about clever prompts and more about what the model can actually see and interact with.

Can it see the repo? Can it run the tests? Does it have our engineering standards? Does it have the ADR template? Can it inspect the application? Can it pull the issue or whatever other context it needs when it needs it?

That tends to matter a lot more to me than finding the perfect wording for a prompt.

The models already know a lot. What they usually don’t know is *our* situation. They don’t know the decision we made six months ago. They don’t know this weird constraint in our codebase. They don’t know what just failed in CI unless I give them a way to see it.

So a lot of the work is just giving them good surfaces to operate on.

### What I mean by being a steward of tokens

For me, that mostly comes down to context.

I don’t want to burn context repeating things the model doesn’t need or stuffing every possible instruction into every session. I’d rather use that space for the things it genuinely couldn’t know otherwise: the code, the architecture, the business constraint, a prior decision, the current test failure, whatever is actually relevant right now.

And if something doesn’t really require model judgment, I’d rather take it out of the model loop entirely.

If Prettier can enforce formatting, use Prettier. If ESLint can catch a rule, let ESLint catch it. If a test can tell the model it broke something, that’s much better than another paragraph saying, “please make sure you don’t break anything.”

That makes the workflow cheaper and it also makes the model better, because now it has real feedback.

I think that’s probably where my trust has come from. I’ve used these systems enough that I have a much better sense of what I can hand off, where I need a check, and when I’m asking the model to do something that would be better handled some other way.

If I had to boil that down to one sentence, it would probably be:

> I trust a system a lot more once I understand how it fails.

---

## What would you say to someone thinking, “I don’t want to look foolish trying this and having it not work”?

I’d probably say: don’t make your first experiment something where you need it to work.

Pick something cheap.

Give it a bug you haven’t looked at yet. Have it explain a piece of unfamiliar code. Ask it to write a test. Give it a small refactor. Let it dig through a log. Give it some little change where you already know roughly what a good answer should look like.

Then just see what happens.

If it works, great. Give it a little more room next time.

If it doesn’t, that’s fine too, because now you’ve learned something. Maybe it didn’t have enough context. Maybe the task was vague. Maybe it had no way to check its own work. Maybe you let it run too far. Or maybe that’s just a bad task for the model right now.

That process of figuring out where it works and where it doesn’t is basically the skill.

I don’t think you need a magic prompt. You definitely don’t need to know every framework. You need enough reps that you start getting a feel for what you can hand off and what kind of setup makes a task go well.

I also think we make this harder when every AI experiment has to look like a success story.

If somebody tries five things and three of them are a waste of time, but two of them save them hours, I think that’s a good outcome. I’d much rather have that than somebody forcing AI into all five because they feel like they’re supposed to show adoption.

Failure is part of the calibration.

So I’d keep it small at first, keep your own judgment in the loop, and start loosening the leash as the model earns it.

You don’t have to prove that AI works. You’re just trying to figure out where it’s actually useful for you.

---

## Shorter version if I need to answer quickly

I think a lot of this comes down to understanding what the model can actually do and then constantly testing how much freedom I can give it.

I use the leash metaphor a lot. Too tight and I’m doing too much of the work myself. Too loose and I get slop. So I’m always trying to move that boundary and see how far I can push it.

The question I keep asking is, “Do I still need this?” If I added a planning step, a persona, an artifact, or some giant instruction file because the model needed it, that’s fine. But I want to come back later and try removing it. If nothing gets worse, get rid of it. If the model falls apart, now I’ve learned where a real constraint is.

That’s also how I look at frameworks. I want to know which parts are actually helping. Better context? Useful tools? Real verification? Great. Keep those. If it’s mostly more process around something the model already handles, I don’t want to carry that forever.

And for somebody who’s nervous about trying this and looking foolish if it fails: start with something cheap. Give it a bounded task, see what happens, and learn from that. You’re not trying to prove anything. You’re getting calibrated.

## Lines I may want to use

- I trust a system a lot more once I understand how it fails.
- I’m always playing with how much leash I can give the model.
- The leash shouldn’t stay the same length forever.
- The question I keep coming back to is: do I still need this?
- A workaround that was necessary six months ago can turn into ceremony without anybody noticing.
- I want to know what part of the framework is actually doing the work.
- I see a failure, I add something to address it, and later I try taking it back out.
- Give the model good surfaces to work with: the repo, tests, standards, tools, and the context it actually needs.
- If Prettier can do it, I don’t need to spend tokens asking the model to remember it.
- You don’t have to prove that AI works. You’re trying to figure out where it’s actually useful.
