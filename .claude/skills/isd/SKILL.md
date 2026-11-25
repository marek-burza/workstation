---
name: isd
description: One-pager glance sheet answering a system design interview question (staff level, 60 minutes), from a task summary passed in by the user. Use when the user sends "isd".
---

From the question in the summary passed with the message that invokes this skill, write a
one-pager answer: the whole design on a single page, read at a glance. The answer begins
with the QUESTION line taken from the summary.

This interview type is a 60-minute, staff-level system design interview in a shared document
that takes typing and drawing. It scores framing an underspecified prompt, decomposing it,
going deep where it is hard, naming trade-offs and committing, and production thinking about
failure.

THE ONE-PAGER MUST HOLD THE WHOLE DESIGN. There is no second pass, so everything a well
designed system needs is here, in few words, rather than some of it at length.

THE MECHANISM-OVER-PRODUCT RULE, WHICH OUTRANKS EVERY SYSTEM DESIGN HABIT:
Explain how a component works; do not name the product that provides it. "A log with
per-partition offsets so consumers replay independently" scores; "use Kafka" does not, and
invites a question the answer cannot back up. When a product name is genuinely the clearest
label, it arrives with the mechanism in the same line.

WHAT A STRONG ANSWER CONTAINS, AND WEAK ANSWERS BURY OR LEAVE OUT:
- THE EXPECTED MECHANISM, NAMED EARLY. Most classic questions have one core mechanism the
  interviewer is waiting for: deterministic hashing for rollout bucketing, a token bucket for
  rate limiting, consistent hashing with virtual nodes for a cache, counter ranges for short
  codes, leases with fencing tokens for a scheduler. Name the naive design it replaces (a
  stored mapping, a batch job, a scan, a lock, a per-request remote call) and why it loses,
  in one line, because that naive design is exactly what the interviewer will push back with.
- THE PROPERTIES THAT MECHANISM BUYS, ONE PER LINE. They are what scores, not its name: what
  stays stable with nothing stored, what happens when a parameter grows (does everyone
  already in stay in), what work is needed when entities are created or deleted (ideally
  none), what stays independent of what.
- THE HOT PATH AND ITS LATENCY. Say where the data sits when a request needs it and whether
  any network call is on that path. If the answer is a local in-memory lookup, say so and
  say what the request passes in.
- CONTROL PLANE VERSUS DATA PLANE, when the system has one: who writes (and that it is a
  single, versioned writer) and how what is written reaches every reader. For distribution
  to many readers: each reader subscribes only to what it needs, a fan-out tree or regional
  relays rather than every reader on the source, push or long-poll of deltas keyed by
  version, and jitter so a fleet does not reconnect at once.
- THE CORE RECORD, WITH EVERY FIELD, on one line, plus its key. The fields show the design:
  a salt field is the independence property, a version field is the delta protocol, an
  owner field is governance.
- THE FALLBACK CHAIN for every dependency the hot path has, down to the last step: last
  known good copy, then a default compiled into the code.
- THE HOT SPOT: the one key, account or moment that takes far more than its share - a
  celebrity, a hot key, the top of the hour, a fleet reconnecting - and what absorbs it.
- THE SELF-INFLICTED FAILURE. A bad change propagates as fast as a good one, so changes are
  validated and rolled out in stages themselves. The safety action (kill, disable, roll back)
  jumps the queue ahead of normal updates.
- GOVERNANCE THE PROBLEM IMPLIES: audit log, ownership, access control. Designed in with one
  line, never deferred.

Hard rules - the one-pager is read at a glance:

- AT MOST 24 LINES OF CONTENT, headings excluded. Cut the least load-bearing line rather
  than run over. A line that any answer would contain scores nothing: cut it first.
- Plain text only. No markdown: no asterisks, no backticks, no fences, no "#" headings.
- Every line stands alone and is under 20 words, the QUESTION line excepted. Never a
  paragraph. A line over 20 words is two ideas - split it or cut one.
- WRITE THE CONTENT, NOT ITS CATEGORY. The names above are for writing the page, not for
  the page itself: no line starts with "Naive design:", "Expected mechanism:", "Properties:",
  "Hot spot:", "Self-inflicted failure:", "Fallback chain:" or "Trade-off:". "Stored
  user-to-flag table: rewritten on every rollout" says it in the words the label would have
  cost.
- NO CAPACITY ESTIMATES. Request rates, storage and machine counts depend on details the
  question leaves open and cost lines the design needs. State a number only where it is a
  requirement - a latency target, a propagation deadline, a size that decides whether
  something fits in memory - and mark it "(assume)" right after the number when invented.
- EXPAND EVERY ACRONYM AT FIRST USE: "WAL (write-ahead log)", "CDC (change data capture)".
  Do NOT gloss what any engineer knows: API, CPU, GPU, SQL, JSON, AWS, HTTP, TCP, DNS.
- COMMIT. Never write "it depends" without choosing for this problem and saying the
  condition that would flip it.

LAST, BEFORE YOU ANSWER, RUN THESE CHECKS OVER YOUR OWN DRAFT:
1. Every capital-letter sequence is expanded at first use, or is one any engineer knows.
2. No product name stands alone without the mechanism it provides on the same line.
3. No line exceeds 20 words except QUESTION, and there are at most 24 content lines.
4. Each point above that this problem has is present - a missing one costs more than any
   line you would cut to make room for it.

Begin with this line:

QUESTION: <what is being asked, one line>

If the summary holds no question, say so on that line and stop there.

IF THE QUESTION LISTS ITS OWN DELIVERABLES, those are the headings, in the order
asked, and the skeleton below becomes a checklist of what to cover inside them.

Otherwise use this skeleton, dropping any heading this question does not reward:

QUESTION: <as above>

ASK
<2-3 questions that would most change the design, then one line: "assume:" plus the
assumptions the design proceeds on, together>

CORE
<the naive design and why it loses, then the expected mechanism>
<its properties one per line: what is stable with nothing stored, what happens as it grows,
the work on create and delete, what stays independent>

DESIGN
<one ASCII flow to redraw in the doc, control plane and data plane if it has them>
<the hot path: where the data sits, what the request passes in, why it meets the latency>
<how writes reach readers, and that each reader receives only what it needs>

DATA MODEL
<the core record with every field, its key, and what it is partitioned by>

FAILURE
<the fallback chain, the hot spot, the self-inflicted failure and its guard, the safety
action's priority>
<one trade-off: the choice, the alternative rejected, the condition that flips it>
