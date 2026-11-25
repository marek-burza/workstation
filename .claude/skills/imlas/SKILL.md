---
name: imlas
description: One-pager prose brief answering an ML/AI system architecture interview question (staff level, 50 minutes), from a task summary passed in by the user. Use when the user sends "imlas".
---

From the question in the summary passed with the message that invokes this skill, write a
one-pager answer as a short prose brief. The answer begins with the QUESTION line taken from
the summary.

This interview type is a 50-minute, staff-level ML/AI system architecture interview in a
shared document that takes typing and drawing. Its questions are deliberately
underspecified. Answers are scored on framing the problem from first principles, on naming
a choice and its reason in the same breath, on going deep rather than wide, and on settling
clarifying questions instead of assuming silently.

The one-pager is a short brief in prose, not an outline and not an essay. Hard rules:

- AT MOST 300 WORDS in the brief. Up to seven paragraphs of two to three sentences each.
  Fewer is better. Cut the weakest paragraph rather than run over.
- NAME AT MOST FIVE DECISIONS. A decision earns a paragraph only if a competent engineer
  could plausibly have chosen otherwise. Anything every answer says - use a vector index,
  cache aggressively, monitor latency, add guardrails, separate the read and write paths -
  is assumed, scores nothing, and must be cut to make room. Coverage is not the goal: the
  interviewer already has the checklist and is looking for judgement.
- LEAD EACH PARAGRAPH WITH ITS CLAIM, then the reason. Never build up to the point.
- EVERY CHOICE CARRIES ITS REASON IN THE SAME SENTENCE, after "because" or a dash. A choice
  stated without its reason is worth nothing - cut it rather than leave it bare.
- WHERE A CHOICE RESTS ON A MECHANISM, GIVE THE MECHANISM IN ONE CLAUSE: what the thing
  does, not what it is called. "Fill-in-the-middle training, because a left-to-right model
  cannot see the code below the cursor" beats "use fill-in-the-middle training".
- FOR THE PRIMARY METRIC AND FOR THE PARADIGM PICK, NAME THE OBVIOUS WRONG ANSWER AND WHY IT
  IS WRONG. That contrast is the highest-scoring sentence in the brief and the one most worth
  getting right.
- Mark every invented input "(assume)". Never present an assumption as given.
- EXPAND EVERY ACRONYM AT FIRST USE, model, metric and library names included: "TTFT (time
  to first token)", "ANN (approximate nearest neighbour)", "NDCG (normalised discounted
  cumulative gain)". Before finishing, re-read your own answer and fix every bare
  capital-letter sequence - that is the most common defect here. Do not gloss what any
  engineer knows: AI, ML, API, CPU, GPU, SQL, JSON, AWS.
- Plain text only. No markdown: no asterisks, no backticks, no fences, no "#" headings.
  Emphasis comes from sentence position, not formatting.
- No hedging, no "it depends", no listing options without picking one.
- The reader is a staff software engineer who learned ML working alongside data
  scientists. Systems, serving, scale and cost need no explanation. Every ML-specific choice
  - a model family, a loss, a sampling scheme, a metric, a training trick - carries its
  reason with it.

Begin with this line:

QUESTION: <what is being asked, one line>

If the summary holds no question, say so on that line and stop there.

Then one line, before the prose:

ASK: <two or three clarifying questions, semicolon-separated, that would most change the
design>

If the question lists its own deliverables - "I want the architecture, the ingestion
trade-offs, the safety mechanisms, and a plan for noisy alerts" - then those become the
paragraphs, in the order asked, and the sequence below becomes a checklist of what to cover
inside them. Every named deliverable gets its own paragraph; one left unaddressed costs more
than every omitted topic put together.

Otherwise the brief runs in this order, one paragraph each, dropping any this problem does
not reward and spending the extra length where it is hard:

the reframing - what this problem actually is once the product language is stripped off,
  and the assumptions the brief proceeds on
the primary metric, and the obvious metric it replaces
the paradigm pick, its reason, and the condition under which the rejected option wins,
  and the mechanism that makes the pick work
where the data/labels come from and the split that prevents the leak, naming the leak,
  and data features engineered and fed into the model
the serving move that buys the latency or the cost
the hard constraint or the abstain rule
what kills it - the failure that ends adoption, last, alone, in two sentences
