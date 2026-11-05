# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- 2-3 lines. Who is talking when you comment on an issue: your
experience level stated plainly, what you are doing in this repo, what
readers can expect from you. This is the register your rules protect. -->

I'm a CS junior at Grinnell College with some experience in full-stack web dev (MERN stack), currently transitioning to AI Engineering. I would consider my experience novice to intermediate. I try to keep my communications on time and I value communication and iterating until a solution is proper.

## Rules I write by

<!-- 3-5 rules, drafted from the lecture's slide-12 moment. Each rule
needs a wrong/right pair from your own hand: one line you might
actually have written that breaks the rule, and the line you would
post instead. The pair is what makes a rule executable; a rule without
one is a wish.

Format each rule like this:

### Rule: <short name>

<The rule, one or two sentences.>

- Wrong: "<a line that breaks it>"
- Right: "<the line to post instead>"
-->

### Rule: no promised timelines
No dates, durations or "soon", including hedged ones like "I expect ... within a few days". Offer a way out instead of a deadline.
- Wrong: "I'll do it tomorrow." / "I'll have it done as soon as possible." / "I expect to open a PR within a few days."
- Right: "If I can't continue, I'll say so here."

### Rule: clear reproduction steps
- The bug occurs on my machine
- Reproduced steps on release x.x.x on Windows 11 24H2

### Rule: avoid meaningless sentences (ai-slop)
- Thanks for reporting this, I'll take a look and get back to you with more details soon!
- Reproduced on v2.3.1 / Windows 11 24H2 — stack trace below.

### Rule: state uncertainty as uncertainty
When I plan or report something I have not verified, I say exactly what I did not check, and I do not describe an expectation as a fact.
- Wrong: "This fixes how permission extensions see bridged tools."
- Right: "I haven't checked how a real permission extension reacts, only what the bridge passes to `registerTool`."

### Rule: follow maintainer direction, and ask when unsure
If a maintainer has said how they want something handled, I follow it or ask before building against it. When my plan depends on a choice that is theirs (title type, scope, version), I name it and invite a correction.
- Wrong: "I'm going with `feat`." (said as settled)
- Right: "The issue is titled `feat`, so I'll use that. Tell me if you'd rather ship it as `fix`."

### Rule: disclose AI help in my own words
Say which tool, what it did, and what I checked myself, even when the repo has no disclosure rule.
- Wrong: "Thanks to my assistant for the help."
- Right: "I used Claude Code (Sonnet 5.5) to read the code and draft the test. The output above is from a real run on my machine, and I reviewed it before posting."

## Things I never post

<!-- A short list. Promises you cannot keep, tones you refuse,
shortcuts you know you reach for when tired. The skill quotes this
list back at you when a draft crosses it. -->
- Passive/agressive tones
- Unreasonable time commitments
- Promises I can't keep
- Claim a task beyond my skill level without notifying
- A description that says a check passed when it ran on nothing (for example, coverage on an empty report)
- A claim about my own work that I did not verify myself
