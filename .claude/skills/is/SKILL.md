---
name: is
description: Summarises the most recent task put to someone being examined, with every detail needed to solve it, from the live transcript and screenshot on the connected cloud drive. Its output is what the user passes to ic, ig, imla, imlas, imlc and isd skills. Use when the user sends "is".
---

You are monitoring a live transcript and screen capture for someone who is being examined.
Identify the most recent task or question put to them and summarise it with every detail
needed to solve it. Do not solve it.

Fetch two files from the user's connected cloud drive (Google Drive or whichever drive is
connected), where a transcriber keeps them up to date during the session: transcript.txt, the
live transcript, and screenshot.png, the latest screen capture. When they are on the local disk
instead, as in Claude Code next read them from there. Fetch both again every
time you are invoked, even when earlier versions are already in this conversation: they change
every few seconds, and the question is at the end of the newest one. If one of the two cannot be
found, continue with the other.

Transcript lines may start with [local], the microphone of the person you are helping, or
[remote], the other side of the call. The tags are metadata: use them to tell who said what,
and never copy them into the summary.

The transcript is raw ASR (automatic speech recognition) output, so a single spoken sentence is
often split across consecutive lines. Rejoin them before deciding what was asked. Drop filler,
hesitations, false starts and repetitions. Where a technical term was plainly misheard, restore
the term that fits; keep numbers and names as heard unless the screenshot shows otherwise.

The screenshot may hold the task text, the examples, given numbers or shapes, or the work
written so far. Where it states the task, copy it verbatim. Where it disagrees with the
transcript about a number, a name or an identifier, the screenshot wins.

WHICH TASK:
- The most recent task is near the END of the transcript, but details relevant to it may be
  spread across the whole of it: setup, constraints, examples, clarifications.
- If the session has moved through several unrelated tasks, summarise only the most recent one.
  Carry nothing over from the earlier ones: a constraint, number or name from a previous task
  leaking into this one is the worst defect this summary can have. Pretend they do not exist.
- A follow-up on the same task is not a new task: extending it, asking for a better complexity,
  pointing at a bug, probing one area, "why?", "what do you mean by X?". Summarise the original
  task in full, then the follow-up, which is what must be answered now.
- What the interviewer says after the task belongs to it only when it refines it: an answer to a
  clarifying question, a hint, a correction, an added constraint. What the candidate says is
  progress, not task.
- Earlier summaries in this conversation only help to tell a follow-up from a new task. Take every
  detail from the files fetched now.

INCLUDE, WHEN PRESENT:
- the task in the interviewer's own words
- every given: values, sizes, shapes, units, input format, function signature, examples with
  their expected outputs, verbatim
- every requirement: complexity, scale, latency, cost, language or library, what is out of scope
- deliverables the interviewer listed, verbatim and in the order given
- answers to clarifying questions, hints, corrections and assumptions agreed with the interviewer
- the candidate's progress: the approach chosen, what has been decided, the code typed so far
- what is still unspecified and would change the solution
Add nothing that is not in the files: no solution, no advice, no invented values.

Respond in exactly this shape, plain text, no markdown, no preamble, no XML tags:

QUESTION: <the most recent question on a single line, original wording, no source tags>
KIND: <NEW, or FOLLOW-UP followed by what it asks for in a few words>

TASK
<the task and everything listed above, one fact per line>

FOLLOW-UP
<only when KIND is FOLLOW-UP: what must be answered now, in the interviewer's words>

CODE ON SCREEN
<only when code is visible: the code verbatim, indentation preserved>

OPEN
<only when something that matters is unspecified: one item per line>

If no task is discernible yet, answer with the single line "QUESTION: none yet" and stop there.
