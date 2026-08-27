# AI Town Hall Speaker Notes

## What’s earned my trust recently?

The biggest thing for me is repeatability.

I’m less interested in the one demo where the model does something incredible, and more interested in whether I can use the same basic approach over and over and keep getting useful results. Once I can do that across a lot of sessions, and I have a decent sense of where it will fail, it starts to feel less like a demo and more like an engineering tool.

A lot of what has earned my trust lately is actually not a specific model or framework. It’s understanding the fundamentals around the model well enough that I can tell what is doing real work and what is mostly ceremony.

I try to decompose anything new I see. If somebody shows me a framework, I want to know what it is actually adding. Is it giving the model information it would not otherwise have? Is it exposing a useful tool? Is it breaking a problem into stages that genuinely improve the result? Is it adding a verification loop? Or is it mostly a large collection of prompts, personas, and Markdown files around something the base model could already do pretty well?

That distinction matters to me because it is very easy in this space to confuse complexity with capability.

For example, if a system says it can generate an ADR, I’m not automatically impressed because it has a large workflow around it or tells the model, “you are a distinguished software architect.” I want to compare that against the simpler version: give a strong model our ADR template, the relevant code and decision context, our engineering standards, and whatever constraints actually matter. Then ask how much value the rest of the machinery added.

That is usually how I try to separate hype from something real. I keep asking: what new information, control, feedback, or capability did this actually introduce?

### The leash I keep adjusting

The mental model I use a lot is that I’m constantly figuring out how much leash I can give the model.

If I keep the leash too tight, I’m leaving a lot of the value on the table. I can micromanage every step, prescribe every intermediate artifact, and tell it exactly how to reason, but at some point I’m spending more effort orchestrating the model than I’m saving. I’m moving too slowly.

If I let it run too far without the right context, feedback, or checks, then quality starts to fall apart and I get slop.

So a lot of the skill, for me, is finding that boundary. How much can I safely delegate today? What still needs a constraint? What can I stop specifying because the model is already good enough at it? As the models improve, I want to keep moving that boundary outward rather than preserving scaffolding just because it used to be necessary.

The leash should not be a fixed length. I want to keep testing whether I can loosen it. If I never do that, I’m not really taking advantage of improving models. If I loosen it too far and quality falls apart, that is useful too, because now I have found a real boundary instead of assuming one.

That leads to one of the habits I think matters most: constantly asking, “Do I still need this?”

A lot of AI workflows start with scaffolding because, at some point, that scaffolding was useful. Maybe the model needed a very explicit planning step. Maybe it needed a persona. Maybe it needed several intermediate artifacts before it could produce a good result. But models change quickly. Yesterday’s workaround can quietly become today’s ceremony.

So I like to keep stripping things away and seeing what happens. Remove a step. Shorten the prompt. Stop generating an artifact. Give the model a little more room. If quality holds, great — the system got simpler and the model earned a little more leash. If quality falls apart, that is useful too, because now I have discovered an actual constraint worth engineering around.

That is the part of one-size-fits-all frameworks that makes me nervous. It is not that structure is bad. Structure can be extremely useful. The risk is that structure becomes permanent, and then we stop asking whether the model still needs it. In a space where the underlying capability is changing this quickly, I do not want to freeze around assumptions about what the model could or could not do six months ago.

A complicated framework can also make a weaker model look impressive while obscuring what is actually doing the work. If there are twenty layers of orchestration around the model, it gets harder to answer which layer is helping, which one is redundant, which one is bloating context, and what the model can now do on its own.

That is why I would rather understand the underlying mechanism than become attached to the framework itself. Keep the pieces that are buying something. Periodically test whether they still are. If they are not, remove them.

In a weird way, one of the things that makes me trust a system more is when I can remove scaffolding and it still works.

The loop I want is pretty simple: add scaffolding when I observe a real failure, verify that it fixes the failure, and then periodically remove it to see whether the failure still exists.

### Giving the model better surfaces

I also think less than I used to about clever prompting and more about what surfaces I can give the model to interact with.

Give it the repository. Give it the tests. Give it the engineering standards. Give it the ADR template. Give it the issue or product context. Give it a way to inspect the application. Give it tools that let it pull the information it needs instead of dumping everything into one giant prompt.

The model already has a lot of general reasoning ability. What it often lacks is the specific state of your environment and a reliable way to interact with it.

That is where a lot of the real leverage has come from for me.

### Being a steward of tokens

When I say I would rather be a steward of tokens than chase adoption metrics, I mostly mean that context is a real engineering resource.

I do not want to fill the model’s context window with instructions that it does not need or information that is not relevant to the current task. I want to spend that context on the things the model could not know otherwise: our architecture, the actual code, the business constraint, a decision that was made six months ago, the current state of a test failure, or feedback from the environment.

And if something does not require model judgment, I would rather make it deterministic.

If Prettier can enforce formatting, use Prettier. If ESLint can enforce a coding rule, use ESLint. If a test can tell the model whether it broke something, give it the test. I do not want to spend tokens repeatedly reminding a probabilistic system to do something that a deterministic tool can guarantee.

That also gives the model a better feedback loop. Instead of telling it “please be careful,” I can give it something concrete that tells it whether the work is correct.

So the thing that has earned my trust is not that I now believe the model is always right. It is almost the opposite. I trust it more because I have a better understanding of what I can delegate, what I need to verify, and what I should not ask the model to do in the first place.

One line I like for this is:

> I trust systems where I understand the failure modes more than systems where I can show you the coolest success case.

---

## What would you say to someone thinking, “I don’t want to look foolish trying this and having it not work”?

I think that reaction is completely understandable, especially when there is a lot of attention on AI adoption and everybody is showing their best examples.

The thing I would say is: do not start by trying to prove that AI works.

Start with something bounded where failure is cheap.

Give it a bug you have not looked at yet. Ask it to explain an unfamiliar part of the codebase. Have it write a test. Give it a small refactor. Let it investigate a log or trace. Ask it to implement a change where you already understand what “good” looks like.

Then pay attention to what happens.

If it works, great. Push a little farther next time.

If it fails, that is useful too. Did it not have enough context? Did it misunderstand the task? Did it have no way to verify what it produced? Did I give it too much freedom? Or is this simply a type of problem that the model is not very good at yet?

That calibration is the skill.

I do not think being good at this means knowing a magic prompt or memorizing a framework. It means developing intuition for what the model can handle, what information it needs, and how far you can let it run before you need to intervene.

There is also a social piece to this. I think we make experimentation harder when every use of AI is implicitly treated as something that needs to become a success story.

I would much rather have somebody try five things, decide three of them were bad uses of AI, and find two that save them a lot of time than force all five into an adoption metric.

A failed experiment is not evidence that somebody is bad at using AI. Sometimes it is exactly how you learn that a task is a poor fit, or that the model needs a different surface, a tighter constraint, or a better feedback loop.

So my advice would be: keep the first experiments cheap, keep your own judgment in the loop, and progressively give the model more responsibility when it earns it.

You do not need to prove that AI works. Your job is to figure out where it works well enough to be useful.

---

## If I need a shorter version in the room

The way I think about this is that AI-native engineering is not about believing the model is magical. It is about understanding what the model already gives you, then adding the minimum useful scaffolding around it: the right context, the right tools, and the right feedback loops.

I am constantly adjusting how much leash I give it. Too little and I am not getting enough leverage. Too much and I get slop. The goal is to keep finding that boundary and move it outward as the models get better.

The question I keep coming back to is, “Do I still need this?” If I can remove a planning step, a prompt, an artifact, or some other piece of scaffolding and the quality stays the same, I should probably remove it. If the model starts failing, then I have found a real constraint worth engineering around. That is more useful to me than assuming a fixed framework is always the right amount of structure.

That is also why I try not to get too attached to frameworks. I want to understand what they are actually providing. If it is better context, useful decomposition, a new tool, or stronger verification, great. If it is mostly more prompts and Markdown around something the model could already do, I want to know that too.

And for somebody who is worried about looking foolish if it fails: start where failure is cheap. Try a bounded task, see what breaks, and use that to calibrate. You are not trying to prove AI works. You are learning where it works well enough to trust.

## A few lines worth keeping in my pocket

- I trust systems where I understand the failure modes more than systems where I can show you the coolest success case.
- I try to understand frameworks as mechanisms, not brands.
- Complexity and capability are not the same thing.
- I am always asking how much leash I can safely give the model.
- The leash should not be a fixed length. I want to keep testing whether I can loosen it.
- One of the most important questions is: do I still need this?
- Yesterday’s workaround can quietly become today’s ceremony.
- Add scaffolding when you observe a real failure, then periodically test whether you can remove it.
- Context is an engineering resource. I want to spend it on information the model could not know otherwise.
- If something can be made deterministic, I would rather make it deterministic than spend tokens repeatedly asking the model to remember it.
- I increasingly think about giving models better surfaces, not just better prompts.
- You do not need to prove that AI works. You need to find where it works well enough to be useful.
