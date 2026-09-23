# The Unofficial Guide

Project 1 submission: campus_life corpus

> **This file is your submission.** Fill it in as you go — most sections get
> written during the milestone that produces them, not at the end.
>
> How the starter works, and every command you'll need, is in `RUNNING.md`.
> Leave that file alone.
>
> **Paste everything as text.** No screenshots, no video. A typed table gets
> full credit; a picture of the same table gets none.
>
> Delete these instruction blocks as you replace them. The `<!-- -->` comments
> are notes to you and don't show up when the page renders — you can leave them
> or remove them.

---

# Unit 1

## What This Does

This project answers practical questions about student life from the
campus_life corpus. The corpus contains 88 short posts about courses, housing,
dining, deadlines, transportation, and campus services. The system loads the
posts, creates searchable chunks, retrieves the closest evidence, and refuses
questions that fall outside the corpus. Answers should cite the source files
used as evidence.

## Chunking Strategy

**Chunk size:** 450 characters
**Overlap:** 0 characters

The documents average about 317 characters and the longest is 549 characters.
Only 12 documents exceed 400 characters, so 450 keeps the normal one-to-three
paragraph post together while limiting the longer posts. I chose zero overlap
because the custom strategy splits at paragraph and sentence boundaries; a
sliding character overlap would duplicate those boundaries without adding
useful context.

## Sample Chunks

The custom strategy produced 91 chunks from 88 documents.

**Chunk 1** — source: `admin_withdrawal_deadline.txt#0` — produced by: `chunker.py::split_documents`

```
Withdrawal is a different thing from dropping and has a different date. Dropping ends at week six. Withdrawal runs to week ten, requires an adviser signature, and puts a W on the transcript that doesn't affect GPA.
```

**Chunk 2** — source: `admin_pass_fail_option.txt#0` — produced by: `chunker.py::split_documents`

```
Any course outside your major can be taken pass/fail, and you can declare it as late as week eight, after you've seen your midterm. A pass needs a C- or better.
```

**Chunk 3** — source: `dining_pellew_dining_hall_followup.txt#0` — produced by: `chunker.py::split_documents`

```
The wait figure of 12 to 18 minutes at peak matches what I've seen. If you're trying to eat between classes, go before 11:45.
```

**Chunk 4** — source: `housing_innisfree_hall.txt#0` — produced by: `chunker.py::split_documents`

```
Laundry costs $1.75 wash, $1.75 dry, app-based. On noise: moderate; the building is L-shaped and the short wing is much quieter.
```

**Chunk 5** — source: `course_cs_340.txt#0` — produced by: `chunker.py::split_documents`

```
Expect 6 hours a week early, 15 in the last three weeks when the project lands.
```

## Sample Answer

**Question:** How late can I withdraw from a course, and does it affect my GPA?

**Answer:** Withdrawal runs to week ten, requires an adviser signature, and puts
a W on the transcript that does not affect GPA. Source:
`admin_withdrawal_deadline.txt`.

```
```

**My relevance cutoff:** 0.6 initially; this is the starter threshold and still
needs to be measured against the five covered and five out-of-scope questions.

<!-- The number you set in config.py, and how you got there.

     You ran five questions your corpus covers and the five in OUT_OF_SCOPE
     that it clearly doesn't, and wrote down the best distance for each. What
     did those two groups look like? Where was the gap? Put the actual numbers
     here — the table below wants all ten rows.

     Milestone 4. -->

| Question | In corpus? | Best distance |
|---|---|---|
| Withdrawal deadline and GPA | yes | not measured yet |
| Pass/fail declaration deadline | yes | not measured yet |
| Pellew peak wait | yes | not measured yet |
| Innisfree laundry | yes | not measured yet |
| CS 340 workload | yes | not measured yet |

## How I Used AI

<!-- Two specific moments. For each: what you asked for, what came back, and
     what you changed about it.

     "I asked Claude to write the chunking function from my notes. It ignored
     the overlap, so I added that myself" is the level of detail we're after.
     "I used AI to help me code" is not.

     Milestone 5. -->

**1.**

I used AI to inspect the starter chunker and compare it with the corpus shape.
The useful observation was that the default produced 88 chunks for 88 short
documents, but a fixed window could split a longer post mid-word. I used that
observation to choose paragraph and sentence boundaries instead of copying a
generic sliding window.

**2.**

I used AI to draft the first five evaluation questions from facts in the
corpus. I checked each expected phrase against its source document and changed
the questions to include exact topics and facts, such as "week ten" and
"12 to 18 minutes," rather than accepting broad questions with no right answer.

<!-- ── Stretch features ─────────────────────────────────────────────────────
     Doing one? Say so here BEFORE you start. A feature this README never
     claims earns nothing.
     ───────────────────────────────────────────────────────────────────────── -->

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

<!-- Your five criteria, three runs each. `python run_eval.py --label before`
     runs the questions, puts the OUT_OF_SCOPE ones through the gate, and
     writes it all into results/ for you. Targets come from criteria.md; the
     verdict column is your call.

     Criterion 3 is measured in one deterministic pass rather than three, so
     the same number goes in all three run columns. That's correct, not lazy.

     Milestone 1. -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. Chunk boundaries preserve complete thoughts | 4 of 5 | pending | pending | pending | pending |
| 5. Retrieved source matches answer evidence | 5 of 5 | pending | pending | pending | pending |

<!-- Underneath, paste the REAL output for each criterion from one of your
     runs — the actual text your system produced, not a description of it.
     Name the file and function that produced it. -->

## Verdicts

<!-- MET or MISSED for each of the five, against the target you wrote last
     unit — not a new one. Plus a sentence on how you decided. That sentence
     matters most where it was close.

     If your target said 4 of 5 and your runs came out 4, 3, 4, that's a MISS.
     The target has to hold, not show up occasionally.

     Milestone 2. -->

| # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 |  |  |  |
| 2 |  |  |  |
| 3 |  |  |  |
| 4 | Chunk boundaries preserve complete thoughts | pending | Await chunk sample review. |
| 5 | Retrieved source matches answer evidence | pending | Await retrieval and answer runs. |

## Diagnoses

<!-- For each miss: which stage caused it, and how. The stage alone isn't
     enough — you need the mechanism.

     Not a diagnosis: "Question 3 didn't work."
     A diagnosis:     "Question 3 asks about laundry costs. The answer is in
                       one sentence that got split across two chunks, so
                       neither chunk on its own contains it."

     The five stages: loading → chunking → embedding → retrieval → generation.

     Look for a pattern. If three misses all ask about numbers, that's one
     problem, not three.

     Missed nothing? Say so, then say honestly whether your targets were set
     low, and which one you'd tighten and to what.

     Milestone 3. -->

## The Improvement

**What I changed:**

I replaced fixed-size fallback windows with `chunker.py::split_documents`,
which groups paragraphs up to 450 characters and splits oversized paragraphs at
sentence boundaries.

**Why I picked it:**

The baseline output showed that this corpus is mostly short posts, so preserving
whole thoughts matters more than adding overlap. The new run has 91 chunks
instead of 88 while keeping every chunk at or below 450 characters.

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 |  |  |  |  |
| 2. Every answer names a source | 5 of 5 |  |  |  |  |
| 3. Gate stops out-of-corpus questions | 4 of 5 |  |  |  |  |
| 4. Chunk boundaries preserve complete thoughts | 4 of 5 | pending | pending | pending | pending |
| 5. Retrieved source matches answer evidence | 5 of 5 | pending | pending | pending | pending |

**Did it help?**

The chunking check passed locally: all 91 chunks are at most 450 characters and
the sample chunks end at readable paragraph or sentence boundaries. Retrieval
and generation still need an indexed run before criteria 1, 2, 3, and 5 can be
judged.

<!-- Say plainly whether it did, and how you know. If it made things worse,
     say that — a change that backfired, honestly reported, earns full credit
     and is more interesting than one that worked. What matters is that you can
     tell.

     Milestone 4. -->

## What's Still Broken

<!-- For each criterion still missed after your fix: what you'd do about it,
     and why you stopped where you did.

     "I ran out of time" is fine if it's true. Pretending nothing is left is
     not.

     Milestone 5. -->

## What I'd Do Differently

<!-- Knowing what you know now — which of your five criteria would you write
     differently, and why?

     Milestone 5. -->
