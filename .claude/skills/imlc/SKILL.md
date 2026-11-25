---
name: imlc
description: One-pager answer to an ML/AI coding interview question (45 minutes, PyTorch or NumPy), from a task summary passed in by the user. Use when the user sends "imlc".
---

From the task in the summary passed with the message that invokes this skill, write a
one-pager answer: clarifications, the maths, approaches, runnable code, tests and complexity
on a single page. The answer begins with the QUESTION line taken from the summary.

This interview type is a 45-minute ML/AI coding interview in Python, in a shared editor. It
is not the algorithms interview type. It asks for a machine learning primitive implemented
from scratch, and it is scored on five things:
- THE MATHS IS RIGHT. The gradient, the normalisation, the distance, the update rule.
- IT IS VECTORISED. Looping over samples or features in Python is the defect this interview
  type exists to find. The only acceptable Python loops are over training iterations, over
  layers, or over k in k-means - never over rows of data.
- IT IS NUMERICALLY STABLE. Subtract the max before exponentiating, in softmax and in the
  sigmoid alike - a sigmoid written as 1/(1+exp(-z)) overflows on large negative z, so clip z
  or branch on its sign. Never take a log or divide without a guard. Say "overflow" once.
- SHAPES ARE STATED AND CORRECT. Every array's shape is named when it appears.
- THE DERIVATION IS EXPLAINED, not just the formula typed.

PYTORCH IS THE DEFAULT for this interview type, in tensors, because that is what the team
works in. Use NumPy only when the task is a classical primitive with no gradients in it -
k-means, k-nearest neighbours, the area under the curve - or when code given with the task
is NumPy. Match any code the task gives. Which one is wanted is a fair clarifying question.

NEVER CALL THE LIBRARY FUNCTION THAT IS THE ANSWER. "Implement attention" does not mean
torch.nn.MultiheadAttention, "implement layer norm" does not mean torch.nn.LayerNorm, and
scikit-learn is never the answer to anything here. Build the mechanism out of tensor
arithmetic. Broadcasting, matmul, reductions, indexing and einsum are all fair.

WHEN THE TASK WANTS A BACKWARD PASS, DERIVE IT BY HAND AND THEN CHECK IT AGAINST AUTOGRAD.
Write the manual gradient, then verify it with torch.autograd.grad on the same inputs and
assert the two agree with torch.allclose. That check is one of the strongest moves available
in this interview type: it shows the maths is understood and proven right.

Hard rules:

- THE CODE MUST RUN AS WRITTEN. Complete, no pseudocode, no ellipses, no TODO, no helper
  left unimplemented. Copied verbatim, it passes the tests given with it.
- The asserts in TEST must pass against the code in CODE exactly as both are written. Use
  torch.allclose or numpy.allclose for anything floating point, never == on floats. Build
  tensors with an explicit dtype of torch.float64 in tests, so tolerances are not the reason
  a correct implementation looks wrong.
- CODE CONTAINS PYTHON AND NOTHING ELSE. Not one prose line, not a shape note, not a stray
  heading. A sentence about shapes sitting between two statements is a syntax error once
  copied. Shapes are stated in WALK.
- EVERY IMPORT GOES AT THE TOP OF CODE, AT MODULE LEVEL, never inside a function. TEST runs
  at module level and needs the same names.
- Plain text only. No markdown: no asterisks, no backticks, no fences, no "#" headings.
  Indentation inside CODE is real Python indentation and must be preserved.
- No comments in the code. The WALK lines carry the explanation instead.
- Prose lines stand alone and stay under 20 words. Code lines are exempt.
- EXPAND EVERY ACRONYM AT FIRST USE: "SGD (stochastic gradient descent)", "MLP (multi-layer
  perceptron)", "AUC (area under the curve)". Do not gloss NumPy, PyTorch, API, CPU, GPU.
- Give the complexity in big-O for time and memory, in terms of the named dimensions.

IF THE TASK GIVES WORKED NUMBERS OR SHAPES, THEY ARE GROUND TRUTH. Reproduce them exactly.

BEFORE YOU ANSWER, EXECUTE YOUR OWN CODE ON EVERY ASSERT, BY HAND.
Walk the code line by line with those exact arrays, carrying real shapes and real values,
and write down what it returns. Compare against the expected value. If they differ, one of
them is wrong: fix the maths if the algorithm is wrong, fix the expected value if you
computed it carelessly. An assert that fails against your own code is the worst defect this
one-pager can have, because both halves get copied as written.
Then check: every acronym expanded, no comments in the code, nothing used that is not
defined in CODE or TEST, and no Python loop over rows of data.

Begin with this line:

QUESTION: <the task in one line, as in the summary>

If the summary holds no task, say so on that line and stop there.

Then use exactly this shape:

QUESTION: <as above>

CLARIFY
<3 questions this task needs settled: PyTorch or NumPy, the input shapes, and what this task
is actually ambiguous about - batched or single, which convention, what to return>
<then one line: "assume:" plus the assumptions the answer proceeds on, together>

MATH
<the formula being implemented, in words plus symbols, on 2-3 lines>
<the derivative or update rule if the task needs one, and where it comes from>
<the one term that is easy to get wrong here>

APPROACHES
<the naive loop version in one line, with its complexity>
<the vectorised version in one line, with its complexity and its memory cost>
<the pick and why - and if the vectorised one allocates too much, say at what size>

CODE
<imports first, then complete runnable PyTorch or NumPy with a type-hinted signature, no
comments>
<Python only - every word about shapes belongs in WALK, not here>

WALK
<4 lines explaining the code: the shape of each intermediate, why this axis, where the
stability guard goes, and the one line that carries the actual maths>

TEST
<first a TRACE line: a tiny concrete input, the intermediate arrays with their numbers, and
the returned value - write it out, do not summarise it>
<then asserts against the code above. NEVER assert a hand-computed float that falls out of
an iterative procedure - what gradient descent returns after 100 steps, or where a centroid
converges, cannot be computed in your head and the assert will be wrong. Verify by property
instead, and prefer these in order:
 1. cross-check: write a four-line naive loop reference inside TEST and assert the vectorised
    function matches it with numpy.allclose. This is the strongest check and the one worth
    naming as the verification strategy.
 2. gradient check: for anything returning gradients, compare against torch.autograd.grad on
    the same inputs, or perturb one entry by 1e-5 and compare the numerical difference.
 3. closed form: softmax of equal logits is uniform; k-means with k equal to n puts each
    point alone; normalising already-normalised input is the identity.
 4. invariant: probabilities sum to one, loss decreases across iterations, output shape,
    assignments lie in range.
 5. an exact literal only where one step of arithmetic gives it.
WHEN THE ANSWER IS NOT UNIQUE, ASSERT WHAT IS INVARIANT. Cluster labels permute, nearest
neighbours tie, random initialisation lands differently, argmax picks either of two equals.
So assert cluster sizes rather than which label they carry, assert each point is nearest its
own centroid, assert the majority when there is no tie. Build fixtures that remove the
ambiguity: well separated clusters, an odd k, no equidistant points, a fixed seed passed in.
Include one overflow case, such as logits of 1000, and one degenerate case. Every fixture
the asserts use is defined inside TEST.>
<one line naming the bug this code is most likely to have, and what would catch it>

COMPLEXITY
<time and memory in big-O over the named dimensions, and what dominates>
