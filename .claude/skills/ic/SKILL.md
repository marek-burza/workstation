---
name: ic
description: One-pager answer to a Python coding interview question (45 minutes), from a task summary passed in by the user. Use when the user sends "ic".
---

From the problem in the summary passed with the message that invokes this skill, write a
one-pager answer: clarifications, approaches, runnable code, tests and complexity on a
single page. The answer begins with the QUESTION line taken from the summary.

This interview type is a 45-minute Python coding interview in a shared editor. It is scored
on four separate axes, and only one of them is the code:
- ALGORITHMS: the top score needs several solutions laid out with their drawbacks, then the
  optimal one chosen. One correct solution with no alternatives scores a 3, not a 4.
- CODING: working, clean, idiomatic Python with no syntax errors.
- COMMUNICATION: the thought process is visible throughout. Jumping straight to code is
  explicitly penalised.
- PROBLEM SOLVING: clarifying questions settled first, then a solution fast enough to leave
  time for trade-offs. Skipping the clarifying questions is explicitly
  penalised.
A fifth thing, verification, is what separates a hire from a strong hire: tests run by hand
over typical and corner cases, bugs found and fixed before the interviewer points at them.
The one-pager covers every axis, in the order the interview runs.

Hard rules:

- THE CODE MUST RUN AS WRITTEN. Complete Python, standard library only, no pseudocode, no
  ellipses, no TODO, no unimplemented helper. Copied verbatim, it passes the tests given
  with it. This is the one thing in this one-pager that cannot be approximately right.
- The asserts in TEST must pass against the code in CODE exactly as both are written.
- Plain text only. No markdown: no asterisks, no backticks, no fences, no "#" headings.
  Indentation inside CODE is real Python indentation and must be preserved.
- No comments in the code. The WALK lines carry the explanation instead, and they are worth
  more than a comment because communication is scored separately.
- Prose lines stand alone and stay under 20 words. Code lines are exempt.
- EXPAND EVERY ACRONYM AT FIRST USE: "BFS (breadth-first search)", "DSU (disjoint set
  union)", "LRU (least recently used)". Do not gloss what any engineer knows: API, CPU, SQL.
- Name the complexity of every approach you list, in big-O, for time and space.
- Prefer clear names over short ones. The code has to read clearly without comments.
- CORRECTNESS BEATS CLEVERNESS. If the optimal algorithm is one you cannot write correctly
  and verify in this one-pager, write the clear one that works, state its complexity
  honestly, and put the optimal one in COMPLEXITY as the improvement on offer. Working simple
  code plus "here is how to get it to O(n)" outscores broken clever code every time.

BEFORE YOU ANSWER, EXECUTE YOUR OWN CODE ON EVERY ASSERT, BY HAND.
Take each assert in turn. Walk the code line by line with those exact arguments, carrying the
real values of the variables, and write down what the function returns. Compare it to the
expected value. If they differ, one of the two is wrong: fix the code if the algorithm is
wrong, fix the expected value if you computed it carelessly. Do this for the corner cases
especially, because that is where both errors hide.
An assert that fails against your own code is the worst defect this one-pager can have: both
halves get copied as written and the failure shows the moment they run.
Then check three more things: every acronym is expanded at first use, the code has no
comments, and the code needs nothing that is not defined in CODE or TEST.

Begin with this line:

QUESTION: <the problem in one line>

If the summary holds no problem, say so on that line and stop there.

Then use exactly this shape:

QUESTION: <as above>

CLARIFY
<3 questions this problem needs settled: input size, duplicates or empties, and whatever
this problem is actually ambiguous about>
<then one line: "assume:" plus the assumptions the answer proceeds on, together>

APPROACHES
<the brute force in one line, with its time and space complexity>
<the better idea in one line, with its complexity, and what it costs>
<the pick, why, and the insight that makes it work - this is the line that scores>

PLAN
<3-5 steps of the chosen algorithm, one short line each>

CODE
<complete runnable Python, type-hinted signature, no comments>

WALK
<3 lines explaining the code: the invariant, why this data structure, the one subtle line>

TEST
<first a TRACE line: the worked example, and the value of each variable the loop updates,
step by step, ending in the returned value - write it out, do not summarise it>
<then assert statements that run against the code above: the examples the problem gave,
verbatim, then the corner cases that matter - empty, single, all-equal, duplicates, boundary>
<one line naming the bug this code is most likely to have, and what would catch it>

COMPLEXITY
<time and space of the final code, and one line on why it cannot be beaten>
<if the code is not optimal, one line on how to reach the optimal complexity>
