# Acceptance criteria — The Unofficial Guide

Five criteria that say what "working" means for this system, written in unit 1
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. *"Retrieval works"* is an opinion. *"For at
least 4 of my 5 test questions, the top results include a chunk containing the
answer"* is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter or looser one. A reason that says something about your corpus or your
pipeline earns credit; *"80% seemed reasonable"* does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

## Criterion 1 — Retrieved chunk contains the answer

For at least **4 of 5** test questions, the retrieved chunks include one
that contains the question's `expects` phrase.
matches ignores capital letters before the full stop.

**Why this target:** city_guides is organised under headings, but
`fallback_split` cuts by length — it produced a 24-character chunk, so a
heading can be separated from the paragraph under it. One miss is allowed
because question 3 asks about closing times across every town, so its
answer is spread across guides.

## Criterion 2 — Every answer names a source

**5 of 5** answers name atleast one guide file in the answer text itself (the "sources received line doesn't count).

**Why this target:** the towns are fictional, so a reader can only trust
an answer they can check against a guide. The starter already cites
sources (my first Kestrelford answer ended with `guide_kestrelford.md`),
so anything less than every answer would be a regression.

## Criterion 3 — Gate stops out-of-corpus questions

At least **4 of 5** `OUT_OF_SCOPE` questions are refused without
reaching the model.

**Why this target:** the guides cover one small coastal region, so sport,
medicine and programming questions should sit far from every chunk. One
miss is allowed because the diesel-engine question shares driving and
road vocabulary with the transport and access sections.

*Revised before testing:* I replaced four of the original out-of-scope
questions (including the diesel-engine one) with my own, to test
questions closer to everyday topics like shopping and music.


## Criterion 4 — Chunks are sized to hold one section

Every chunk is between **150 and 750 characters**, and **0** chunks
consist of a heading alone, and change it to "checked with the shortest/longest figures printed by `python app.py index`.

**Why this target:** city_guides is organised into labelled sections,
and I measured them: the real sections run from 175 to 712 characters,
most between 200 and 400. Four guides open with a title-only heading of
24–28 characters, and `fallback_split` turned one of these into a
24-character chunk that can't answer anything. 150 sits just below the
shortest real section, and 750 just above the longest, so a chunker that
keeps one section per chunk passes, and one that cuts headings off or
merges several sections fails.

## Criterion 5 — Answers contain the expected fact

At least **4 of 5** answers contain their `expects` phrase.

**Why this target:** criterion 1 checks that retrieval finds the fact;
this checks that the model actually uses it. Questions 1 and 3 rely on
general statements that don't name the town, the hardest case in this
corpus, so I allow one miss rather than requiring all five.
---

## 1. Retrieved chunks contain the answer

For at least 4 of my 5 test questions, the retrieved chunks include one that
contains the answer.

**Why this target:**
<!-- e.g. "One of my questions is about a topic only two documents mention, so
     I expect that one to be hard." -->

---

## 2. Every answer names a source

Every answer the system produces names at least one source document.

**Why this target:**
<!-- Why all five and not four? What about your setup makes that achievable —
     or what would have to go wrong for it not to be? -->

---

## 3. The relevance gate stops out-of-corpus questions

When I ask a question my documents clearly don't cover, the relevance gate
stops it and the system returns "I don't have enough information about that" —
in at least 4 of 5 tries.

<!-- The five questions are the ones in `OUT_OF_SCOPE` at the bottom of
     `questions.py`, and `run_eval.py` puts them through the gate and writes
     what happened into your run log. Swap them for your own if you'd rather —
     just keep five of them, or the "4 of 5" above has nothing to be 4 of. -->

**Why this target:**
<!-- What did your distances look like when you set the cutoff in Milestone 4?
     Was there a clean gap, or did the two groups overlap? -->

---

## 4. Something about your chunks

<!-- YOU WRITE THIS ONE.

     How would you know if your chunks were the right size? Name something
     countable or observable.

     Examples of the right shape — don't copy these, they should come from
     what you actually saw in Milestone 3:
       - "At least 4 of 5 sampled chunks read as a complete thought, with no
          sentence cut in half at either end."
       - "No chunk is shorter than 200 characters, since anything below that
          in my corpus turned out to be a heading with no content under it." -->



**Why this target:**



---

## 5. Your choice

<!-- YOU WRITE THIS ONE TOO.

     Pick something you actually care about getting right. It could be about
     speed, about refusals, about a particular kind of question your corpus
     handles badly, about source attribution being correct rather than merely
     present — anything, as long as it names a number or an observable
     outcome. -->



**Why this target:**



---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 2 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 1. Retrieved chunks contain the answer

         For at least 4 of my 5 test questions, the retrieved chunks include
         one that contains the answer.

         **Why this target:** ...

         > **Revised in unit 2:** For at least 4 of 5 questions, the top three
         > results contain the answer.
         >
         > **Why revised:** I couldn't judge "the chunks include one that
         > contains the answer" the same way twice — I scored two questions
         > differently on Monday than on Wednesday. The new version is
         > something I can actually check.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "I said 4 of 5 but got 2 of 5, so 2 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.

     The whole reason the originals stay visible is so someone can see what you
     said before you knew the answer.
     ───────────────────────────────────────────────────────────────────────── -->
