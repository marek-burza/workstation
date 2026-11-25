---
name: imla
description: One-pager glance sheet answering an ML/AI system architecture interview question (staff level, 60 minutes), from a task summary passed in by the user. Use when the user sends "imla".
---

From the question in the summary passed with the message that invokes this skill, write a
one-pager answer: the whole architecture on a single page, read at a glance. The answer
begins with the QUESTION line taken from the summary.

This interview type is a 60-minute, staff-level ML/AI system architecture interview in a
shared document that takes typing and drawing. Its questions are deliberately
underspecified. Answers are scored on framing the problem from first principles, on
solutions that scale, on naming two or three approaches with their trade-offs and then
deciding and saying why, on going deep rather than wide, and on settling clarifying
questions instead of assuming silently.

This is a one-pager, not an essay. Hard rules:

- AT MOST 45 LINES IN TOTAL. Fewer is better. Cut the least load-bearing line rather than
  run over. Breadth is assumed; coverage that does not fit a 60-minute interview scores
  nothing.
- Plain text only. No markdown: no asterisks, no backticks, no fences, no "#" headings.
- Every line stands alone and is under 20 words. Never a paragraph.
- EXPAND EVERY ACRONYM AT FIRST USE, model, metric and library names included:
  "QPS (queries per second)", "ACL (access control list)", "GBDT (gradient-boosted decision
  trees)", "NDCG (normalised discounted cumulative gain)", "ANN (approximate nearest
  neighbour)", "TTFT (time to first token)". Before you finish, re-read your own answer and
  fix every capital-letter sequence you left bare - that is the most common defect here.
  Do NOT gloss what any engineer already knows: AI, ML, API, CPU, GPU, SQL, JSON, AWS.
- DERIVE NUMBERS, NEVER ASSERT THEM. Give the inputs and the arithmetic, not the answer:
  "50 QPS x 100 candidates = 5k reranker calls/s", not "5k reranker calls/s". The
  calculation is what scores, and a number without its inputs cannot be checked. At
  least three lines in the answer must show their arithmetic this way: the load, the thing
  that dominates cost or latency, and the money.
- Numbers that depend on each other must agree. If ingest rate, scoring rate and cost are
  all stated, they must reconcile, and the line should show that they do.
- Mark every invented input "(assume)". Never present an assumption as given.
- The reader is a staff software engineer who learned ML working alongside data
  scientists. Systems, serving, scale and cost need no explanation. But every ML-specific
  choice - a model family, a loss, a sampling scheme, a metric, a training trick - carries
  its reason in the same line, after "because" or a dash, in under ten words.

Begin with this line:

QUESTION: <what is being asked, one line>

If the summary holds no question, say so on that line and stop there.

BEFORE USING THE SKELETON, CHECK WHETHER THE QUESTION LISTS ITS OWN DELIVERABLES.

Many do - "I want the architecture, trade-offs for ingestion and storage, safety mechanisms,
and a plan for multi-cloud and noisy alerts". When it does:

- THOSE ARE YOUR HEADINGS, in the order asked.
- The skeleton below stops being the shape of the answer and becomes a checklist of what to
  cover inside them.
- Every named deliverable gets at least four lines. One named deliverable left on one line
  loses more marks than every skipped skeleton section put together.
- Anything the question named that the skeleton has no home for - storage, retention, cost
  tiering, multi-tenancy, per-provider differences - is exactly where the marks are, because
  it is what the skeleton would otherwise crowd out.

Only when the question names no deliverables do the skeleton headings below apply as written.
Either way, spend lines where the problem is hard rather than evenly, and drop any heading
this question does not reward.

QUESTION: <as above>

HARD PART
<the one or two places this specific problem is genuinely hard, and where to spend depth>
<the thing most answers miss here>

ASK
<3-4 clarifying questions that would most change the design, one per line>
<then one line: "assume:" plus the assumptions the design proceeds on, together>

SCOPE + METRICS
<user, unit of work, one thing declared out of scope>
<the load-bearing numbers, each derived from its inputs>
<one primary metric, two guardrails>

FRAMING
<two or three paradigms, one line of trade-off each>
<the pick, why, and the condition under which the other one wins>

DATA + FEATURES
<sources, where labels come from, the split and the leakage that split prevents>
<raw signals, then the numeric representation for two of them>

ARCHITECTURE
<one ASCII flow to redraw in the doc>
<one short line per box that is not self-explanatory>

MODEL + SERVING
<the model, and why it beats the simpler one>
<validation scheme, and how overfitting would show up here>
<what makes it production ready, with the latency or cost arithmetic>

EVAL + PRODUCTION
<offline suite, online experiment, the metric to ship on, the metric to watch for harm>
<top failure mode, drift, retraining, rollback>

RISKS
<3 lines, one each: bias or unfairness and how it is measured; the privacy or access
constraint; when the system must abstain>

CLOSE
<v1, what is deferred to v2, the one risk that kills it>
<one adjacent surface the same model unlocks>
