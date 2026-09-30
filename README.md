# The Unofficial Guide

**Edmine** — corpus: `city_guides`

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

The `city_guides` corpus is 14 travel guides to a fabricated coastal
region: nine town guides and five guides on topics such as eating,
seasons and accessibility. The system answers questions about the
region, like restaurant hours and the types of transportation
available, using only those guides. You can ask when to visit, where to
eat, how accessible a town is, or how to get there. Answers come from
the source material and name the guide they used, and the system
refuses questions the guides don't cover.



## Chunking Strategy

**Chunk size:** one section per chunk (175–712 characters in this corpus)
**Overlap:** 0

the guides are split into labelled sections that each cover one topic, and the starter's 800-character windows cut across them and left a 24-character title on its own. Save
<!-- What about YOUR documents made you pick these numbers? Short posts and
     long sectioned guides don't want the same chunking, and "800 seemed
     reasonable" earns nothing. Point at something you noticed when you read
     the documents in Milestone 1.

     If you changed your mind partway through, say so and say why. That's worth
     more than pretending you got it right first time.

     Milestone 3. -->

## Sample Chunks

<!-- Five chunks, pasted as text. Label each one and name the file it came from
     AND the function that produced it — the grader checks your code against
     what you claim here.

     `python app.py chunks -n 5` prints all three for you. Copy them straight
     across.

     Milestone 3. -->

**Chunk 1** — source: `guide_accessibility.md#0` — produced by: `chunker.py::split_documents`

```
# Getting around the region with limited mobility

An honest assessment rather than a promotional one. Some of these places are
difficult and it is better to know in advance.
```

**Chunk 2** — source: ` guide_corry_vale.md#5` — produced by: `chuker.py ::split_documents`

``` Where to stay

Perhaps thirty beds in the entire valley, spread across two pubs and a handful of farmhouse rooms. In summer these are booked months ahead. Camping is permitted on two marked fields and nowhere else.
```

**Chunk 3** — source: `guide_givens_mill.md#2` — produced by: `chunker.py::split_documents`

```
 Getting around

Everything is on one street along the river. The mill is at one end and the church at the other, eight minutes apart. The riverside path continues in both directions for as far as you want to walk.
```

**Chunk 4** — source: `guide_kestrelford.md#4` — produced by: `chunker.py::split_documents`

```What to see

The market square on a Saturday morning is the main event and has run continuously since the 1400s. The parish church has a 13th-century tower you can climb for £2. The old trackbed walk runs six miles to the next village along an easy gradient and is the best half-day here.
```

**Chunk 5** — source: `guide_pellew_sands.md#6` — produced by: `chunker.py::split_documents`

```When to go

June and September for the beach without the crowds. July and August are busy and the town is at its most itself, for better and worse. Winter is bleak, largely closed, and has a following among people who like that sort of thing.
```

## Sample Answer

<!-- One complete question and answer, pasted as text, with the source line
     visible. Milestone 4. -->

**Question:** A friend of mine requires accessibility, is Marchwood step-free?

**Answer:**

```
(best distance 0.509, cutoff 0.6)

Marchwood has level boarding on all four lines of its modern tram network, and both its city museum and covered market are step-free (guide_accessibility.md).

Sources retrieved: guide_accessibility.md, guide_kestrelford.md, guide_marchwood.md, guide_thornby_wells.md
```

**My relevance cutoff:**  My five real questions scored between
0.438 and 0.510, and my five off-topic questions scored between 0.734
and 0.903, so the gap is between 0.510 and 0.734. I chose 0.6, below the
middle of that gap, because I want the RAG to be strict and not let
random questions poke through. It still leaves room above my weakest
real question (0.510).

| Question | In corpus? | Best distance |
|---|---|---|
| When should I plan to visit Kestrelford? | yes | 0.438 |
| A few of my friends require dietary accommodations, what restaurants cater to pescatarians? | yes | 0.510 |
| When planning this vacation should I be concerned about restaurants kitchen closing early or days where restaurants aren't available? | yes | 0.453 |
| A friend of mine requires accessibility, is Marchwood step-free? | yes | 0.509 |
| Does Marchwood have the closest airport? | yes | 0.488 |
| What is the capital of Mongolia? | no | 0.803 |
| Is there an anime clothing store that sells sailor moon shirts? | no | 0.734 |
| How do I change oil in my toyota camry? | no | 0.879 |
| Where will Beyonce announce her next album? | no | 0.750 |
| Do you carry any Beyonce's merch? | no | 0.903 |


## How I Used AI

**1.** I asked Claude for step-by-step instructions so I'd know what I
was working on and what was done. It gave me checklists of what to
complete, which file to edit, and where to paste my output. But the
commit steps never said to save my files first, and I noticed my commits
could leave out edits still open in VS Code. I started saving before
every commit, checked with git status, and turned on Auto Save.

**2.** I asked Claude for grep commands to find the answer to each of my
test questions, so I could write the expects phrases. For "When should
I plan to visit Kestrelford?" it gave me a search for the word
"kestrelford" in guide_seasons.md, which only found lines about the
market, walkers and snow, not the best time to go. I told Claude I
didn't think that search was robust, since it only finds lines that name
the town. I searched further and found that the Kestrelford guide has
its own "When to go" section, which says late spring and early autumn.
I changed my expects phrase from "After May, June, September" to "late
spring" and my source to guide_kestrelford.md.

---

# Unit 2

<!-- These sections get ADDED to what's already above. Don't delete or rewrite
     unit 1 — the point is that someone can see what you said before you knew
     how it went. -->

## Run Log — Before

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 3/5 | 3/5 | 3/5 | MISSED |
| 2. Every answer names a source | 5 of 5 | 3/5 | 3/5 | 3/5 | MISSED |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Chunks between 150 and 750 characters, none heading-only | all chunks | 174–711 | 174–711 | 174–711 | MET |
| 5. Answers contain the expects phrase | 4 of 5 | 1/5 | 1/5 | 2/5 | MISSED |

All output below is from `results/run_2026-09-29_2145_before.md`, produced
by `run_eval.py::main`, with retrieval from `store.py::search` and chunks
from `chunker.py::split_documents`.

**Criterion 1** — "When should I plan to visit Kestrelford?", run 1. The
answer is in the Kestrelford guide's "When to go" section, which was not
retrieved:

```
Best distance: 0.4378 (passed the gate)
Sources retrieved: guide_kestrelford.md, guide_seasons.md, guide_thornby_wells.md, guide_walking.md
```

**Criterion 2** — "A few of my friends require dietary accommodations, what
restaurants cater to pescatarians?", run 1. No guide is named:

```
I don't have enough information to answer your question.
```

**Criterion 3** — produced by `run_eval.py::check_out_of_scope`, cutoff 0.6:

```
refused  (best distance 0.803)  What is the capital of Mongolia?
refused  (best distance 0.734)  Is there an anime clothing store that sells sailor moon shirts?
refused  (best distance 0.879)  How do I change oil in my toyota camry?
refused  (best distance 0.750)  Where will Beyonce announce her next album?
refused  (best distance 0.903)  Do you carry any Beyonce's merch?
-> gate refused 5 of 5
```

**Criterion 4** — the summary line from `python app.py index`, before the
improvement:

```
chunked  94 chunks, 306 characters on average (shortest 174, longest 711), produced by chunker.py::split_documents
```

**Criterion 5** — "When planning this vacation should I be concerned about
restaurants kitchen closing early...", run 1. The answer is correct but
writes "9 pm", so it does not contain the expects phrase "9pm":

```
Yes, you should be concerned. In Elder Ness, the single pub serves food only from 12 to 2 and 6 to 8 and is closed on Mondays (guide_elder_ness.md). In Kestrelford, the pubs serve food only between 12 and 2 and again between 6 and 8:30, with nowhere else to eat outside those windows (guide_kestrelford.md). Additionally, in Halden Bay, everything closes by 9 pm and much of it closes entirely from November to February (guide_halden_bay.md).
```

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

| | # | Criterion | Verdict | How I decided |
|---|---|---|---|
| 1 | Retrieved chunk contains the answer | MISSED | 3/5 in every run. Questions 1 and 5 never had their expects phrase in the retrieved chunks, and retrieval is deterministic, so all three runs were identical. |
| 2 | Every answer names a source | MISSED | 3/5 in every run. The two misses were refusals ("I don't have enough information"), which name no guide. I counted them as misses because my criterion says every answer must name a source. |
| 3 | Gate stops out-of-corpus questions | MET | 5/5 refused. The closest off-topic question scored 0.734, well above the 0.6 cutoff. |
| 4 | Chunks between 150 and 750 characters, none heading-only | MET | The index line reported shortest 174 and longest 711, and every title-only heading was joined to the section below. |
| 5 | Answers contain the expects phrase | MISSED | 1/5, 1/5, 2/5. Question 3's answer was correct but wrote "9 pm" instead of "9pm". I counted that as a miss because the phrase isn't in the answer; even counting it, the best would be 2/5. |

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

**What I changed:** added the guide's place name (e.g. "[Marchwood]") to the start of every chunk.

**Why I picked it:**

<!-- Connect it to a specific diagnosis above in one sentence. If you can't,
     you picked a fix because it sounded impressive. -->

### Run Log — After

<!-- Same format, same five criteria, three runs each.
     `python run_eval.py --label after` -->

| Criterion | Target | Run 1 | Run 2 | Run 3 | Verdict |
|---|---|---|---|---|---|
| 1. Retrieved chunk contains the answer | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 2. Every answer names a source | 5 of 5 | 4/5 | 4/5 | 4/5 | MISSED |
| 3. Gate stops out-of-corpus questions | 4 of 5 | 5/5 | 5/5 | 5/5 | MET |
| 4. Chunks between 150 and 750 characters, none heading-only | all chunks | 190–727 | 190–727 | 190–727 | MET |
| 5. Answers contain the expects phrase | 4 of 5 | 4/5 | 3/5 | 3/5 | MISSED |

**Did it help?**

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
