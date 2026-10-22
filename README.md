# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

TakeMeter sorts opinion comments from r/television, a general TV discussion
subreddit, into two categories: `analysis` (a claim backed by a specific,
checkable detail that's explicitly tied to the verdict) and `take` (a
confident claim with no such backing). The goal is to separate posts that
argue something from posts that just assert something, since both show up
constantly in the same threads and regulars can usually tell the difference
within a sentence without being able to say exactly how.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `analysis`

**Definition:** An evaluative claim about a show backed by a specific,
checkable detail (a scene, episode, stat, or named fact) that is explicitly
connected to the verdict by reasoning language.

**Example 1:**
> "It seems to me that they don't want Muppets to do well, they want them to
> do astronomically well. Like, the 2011 movie did well at the box office, but
> the sequel and the attempts at tv shows don't find a massive audience like
> other IPs."

**Example 2:**
> "I had to stop when the Walking Dead they got to that junkyard town with the
> lady with the bad haircut. I think they were called the Scavengers. By that
> point, all my favorite characters were gone and I didn't see the point to
> keep on going."

### `take`

**Definition:** A confident evaluative claim about a show with no specific
support offered, or a specific detail mentioned without being used to argue
the verdict.

**Example 1:**
> "Malcolm in the Middle has aged really well, though i think my name is earl
> has a plenty of charm too."

**Example 2:**
> "Just going to suggest Peter Jackson makes Muppets LOTR. He has experience
> directing with Muppets and it would be absolutely legendary!"

### The hardest boundary

**Which two labels:** `analysis` vs. `take`

**The decision rule I used every time:** A specific, checkable detail only
makes a post `analysis` if it's explicitly connected to the verdict by
reasoning language — "because," "that's why," "and it shows," "by that point,"
etc. If the detail and the opinion sit side by side with no stated causal
link, it's `take`.

The case that found this rule: "I love when Dewey tells the bullies he called
their moms and while they scoff, their mothers show up and they get dragged
off by their mothers." This names an exact, checkable scene — the reason
`analysis` exists as a label — but naming a scene isn't the same as arguing
something with it. "I love when X happens" is a feeling with a detail
attached, not a claim built from that detail. Applying the rule: this is
`take`, since there's no causal link between the scene and any broader verdict
about quality.

---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** r/television, a general TV discussion
subreddit (not built around a single show). Posts were collected from a wide
variety of threads: episode-discussion and "aged well/poorly" threads,
"deserves more love"/hidden-gem threads, character-development discussion,
franchise-news and casting threads, and "does it get better" threads. Posts
had to meet an inclusion rule decided during collection: a post is in scope
if it states a preference, recommendation, or evaluative judgment about a
show/episode; posts that only report an action ("just started watching X")
with no judgment attached were excluded, as were posts reacting to
real-world news about a person rather than the show's quality, and posts that
were really about fandom/reception (reddit vs. general audiences) rather than
the show's content itself.

**How I labelled them:** The first 20 were labelled cold — no help, no
tools, just the taxonomy above — marked `cold` in the notes column. The
remaining ~143 were collected in batches and pre-labelled with AI assistance
using the taxonomy and decision rule, then every single one was read and
corrected by hand before being added; those rows are marked
`pre-labeled, checked` in the notes column.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `analysis` | 84 | 51.5% |
| `take` | 79 | 48.5% |
| **Total** | 163 | 100% |

**Three hard cases**

**1.**
> *The post:* "I listened to the dramatized audiobooks at work so I was
> pretty entertained if only for the dragons."
>
> *Could have been:* `analysis` or `take`
>
> *I chose, because:* `take` — "if only for the dragons" is a soft
> attribution, not an explicit causal link like "because" or "that's why." It
> names a detail next to the verdict without building an argument from it,
> the same shape as the Dewey boundary case above, just with weaker phrasing.

**2.**
> *The post:* "No, it actually gets worse. That's why it's such a popular,
> beloved and rewatched show."
>
> *Could have been:* `analysis` or `take`
>
> *I chose, because:* `take` — this is sarcasm (the literal words say
> "worse," the intended meaning is the opposite). Even reading past the irony
> to the real claim, there's no specific, checkable detail behind it, just a
> general appeal to popularity.

**3.**
> *The post:* "Bojack Horseman did a great job developing characters."
>
> *Could have been:* `analysis` or `take`
>
> *I chose, because:* `take` — it sits right next to comments in the same
> thread that were labelled `analysis` for describing an actual character
> trajectory (e.g. "Rick becomes a little more understanding and selfless").
> Naming a show and asserting a verdict isn't enough; `analysis` requires
> describing what actually changed, not just a conclusory label like "did a
> great job."

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** 3 epochs, learning rate 2e-5, batch size 16, max length 128
tokens, seed 42 — all the notebook's defaults.

**Anything I changed from the defaults, and why:** Nothing. Used the
defaults as-is for this first run.

**Split sizes:** 70/15/15 stratified split of 163 rows — test split has 25
rows (13 `analysis` / 12 `take`), both comfortably above the ~8-example
threshold where F1 gets jumpy. Train/val are approximately 114/24.

**Device:** Apple Silicon GPU (MPS), torch 2.14.1 — recorded in `results.json`.
Per the notebook's own note, this is part of the result, not just a detail
about the laptop: the same data and seed on different hardware can produce
different numbers because of how the arithmetic is ordered.

**Headline result (not yet evaluated against criteria — that's next unit):**
overall accuracy 0.60, F1 `analysis` 0.72 / `take` 0.29. The confusion matrix
shows the model caught all 13 true `analysis` posts but only 2 of 12 true
`take` posts — a real asymmetry worth digging into next unit.

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Stress-tested the `analysis`/`take` taxonomy by asking
  for 8 posts written to sit right on the boundary between the two labels.
- *What came back:* 8 TV-opinion-style sentences. Classifying them against
  the taxonomy, 7 resolved cleanly but one ("I end up more impressed,
  especially with the lighting in episode 4") stayed genuinely ambiguous — it
  named a detail next to a verdict with no stated link between them.
- *What I changed:* Tightened the decision rule to require explicit
  reasoning language ("because," "that's why," "and it shows") connecting the
  detail to the verdict, not just the detail sitting near an opinion. That
  tightened rule is the one in the taxonomy above.

**Moment 2**

- *What I asked for:* For most of the ~143 non-cold rows, pasted batches of
  raw post text and asked for a pre-label (`analysis`/`take`, or excluded if
  out of scope) with a one-line reason per post, against the taxonomy and
  inclusion rule.
- *What came back:* A labeled table per batch, including some out-of-scope
  flags for posts that read as pure trivia, off-topic fandom discussion, or
  status updates rather than opinions.
- *What I changed:* Read and corrected every single row before it was added
  to `labels.csv` — several pre-labels were adjusted or excluded outright
  (e.g. posts that were really about franchise lore rather than the specific
  show, or about real-world events rather than the show's quality).

**Pre-labelling disclosure:** 142 of 163 rows (marked `pre-labeled, checked`
in the notes column) were pre-labelled with AI assistance in batches and then
individually read and corrected by hand. The first 21 rows (marked `cold`)
were labelled without any AI assistance.

**Moment 3 (Unit 6)**

- *What I asked for:* For each of the two disagreements against the staff
  set, asked for the strongest possible case for each label using only the
  staff's written definitions, without being told which one staff actually
  picked.
- *What came back:* A two-sided argument for each post (sp_010, sp_020) —
  neither side was told to me as "correct."
- *What I changed:* Nothing was changed for me — I made the actual call and
  wrote the adjudication myself, using the two-sided arguments as a sanity
  check rather than an answer.

**Moment 4 (Unit 6)**

- *What I asked for:* After the first training run missed several criteria,
  asked for the 11 `analysis`-labeled rows out of 163 that showed the same
  "named-but-not-argued" pattern my agreement-check disagreements had
  surfaced (a detail mentioned decoratively, or a vague attribute with no
  concrete instance).
- *What came back:* A list of 11 candidate rows with a one-line reason each,
  for me to review before anything changed.
- *What I changed:* Confirmed all 11 flips to `take` after reading the
  reasoning myself — this is the one improvement made for this unit, and it's
  what moved `take` F1 from an average of 0.369 to 0.742 across the three
  seeds.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.52 | 0.60 | +0.08 |
| Macro F1 | 0.342 | 0.504 | +0.162 |
| F1 — `analysis` | 0.684 | 0.722 | +0.038 |
| F1 — `take` | 0.000 | 0.286 | +0.286 |

**What I predicted before I looked:** "I predict the trained model will
narrowly beat the baseline on overall accuracy, but the real gap will be on
`take`: the baseline's zero-shot guess collapses almost entirely into
`analysis` (0% precision/recall on `take`), and while the trained model is
also `analysis`-biased, it at least catches a couple of `take` examples — so
fine-tuning helped it partially learn the boundary even if neither model has
really learned it." (Note: by this point in the process the standalone
trained-model numbers were already known from Unit 5, so this was a
prediction about where the *comparison* would land rather than a fully blind
guess at the raw numbers.)

**What the gap actually means:** The prediction held. Fine-tuning barely
moved overall accuracy (+0.08) but it's the only thing that gave the model
any ability to recognize `take` at all — the baseline, working from the bare
label definitions with no examples, never once predicted `take` correctly.
Both models still default heavily toward guessing `analysis`, though, which
suggests the real limitation isn't the lack of training so much as the task
itself: recognizing the *absence* of causal reasoning is a harder pattern to
learn than recognizing its presence, and 163 examples may not be enough
signal for that negative case either way.

---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.60 | 0.56 | 0.76 | MISSED |
| 2. Per-label F1 (`take` / `analysis`) | `take` ≥ 0.65, `analysis` ≥ 0.55 | 0.286 / 0.722 | 0.154 / 0.703 | 0.667 / 0.813 | MISSED |
| 3. Balance | no label > 60% of `labels.csv` | 51.5% / 48.5% (dataset-level, same every seed) | — | — | MET |
| 4. Consistency | ≥ 80% agreement | 93% (one-time measure, not seed-dependent) | — | — | MET |
| 5. Confidence *(revised)* | most-confident-third accuracy ≥ 0.85 | 0.75 | 0.75 | 0.75 | MISSED |

See "Verdicts and Diagnoses" below for how each of these was decided and the
cause behind every miss.

### Confusion matrix

(Seed 42 — the matrix Section 7 reports.)

| true \ predicted | analysis | take |
|---|---|---|
| **analysis** | 13 | 0 |
| **take** | 10 | 2 |

**My biggest off-diagonal number, and what it means:** 10 real `take` posts
were called `analysis`, and 0 real `analysis` posts were called `take` — a
completely one-sided confusion. The model never misses an `analysis` post in
any of the three seeds, but it's unreliable at recognizing `take`, and this
direction matches the baseline's bias too (it never predicted `take`
correctly either). That points at the boundary itself — recognizing the
*absence* of argued reasoning — being harder to learn than recognizing its
presence, rather than a random/symmetric error.

---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 | Overall accuracy | ≥ 0.65 | MISSED | 2 of 3 seeds (0.60, 0.56) fall below target; only seed 2024 (0.76) clears it. Target must hold across all three. |
| 2 | Per-label F1 | `take` ≥ 0.65, `analysis` ≥ 0.55 | MISSED | `analysis` clears 0.55 in all three seeds (0.722/0.703/0.813). `take` only clears 0.65 in one of three (0.667), missing badly in the other two (0.286, 0.154). |
| 3 | Balance | no label > 60% | MET | `labels.csv` is 51.5% `analysis` / 48.5% `take` — well under the cap, and this is dataset-level so it doesn't vary by seed. |
| 4 | Consistency | ≥ 80% agreement | MET | 28/30 = 93% agreement with staff on the staff set. |
| 5 | Confidence *(revised)* | most-confident-third accuracy ≥ 0.85 | MISSED | Exactly 0.75 in all three seeds — consistent, not a fluke of one split. |

**Diagnoses**

**Criteria 1 and 2 (overall accuracy, `take` F1) — one underlying cause, two
pieces of evidence pointing at different parts of it.**

Evidence for a data/task limitation: the baseline model — zero-shot, never
trained on any of my labels — made the *exact same kind* of error as my
trained model: 100% recall on `analysis`, 0% correct `take` predictions. A
model that never saw my training data can't be reproducing my labeling
habits, so some of this is just that "this post doesn't argue anything" is a
harder pattern to learn than "this post does argue something" — an absence
is harder to pin down than a presence, especially from 163 examples.

Evidence for a labeling-consistency problem: my agreement check against the
staff set found both of my disagreements on the exact same boundary
(`analysis` vs. `hot_take`-equivalent), and they went in *opposite*
directions — I was less willing to call `analysis` on sp_010 and more
willing on sp_020. That's not a one-way bias; it's an inconsistent threshold
for "how much evidence counts as load-bearing," applied even when reading
someone else's carefully written rules. If I'm inconsistent there, the same
threshold almost certainly wobbled somewhere across my own 200-label
`analysis`/`take` boundary, giving the model a blurrier line to learn from
than it needed.

Also relevant: `take` has only 79 examples total and just 12 in any given
test split, so its F1 is inherently noisy by seed (0.154 to 0.667) on top of
whatever the model actually learned — a second, compounding reason `take`
specifically is the unstable number.

**Criterion 5 (confidence) — the model is never very sure, in either
direction.** `mean_when_right` (0.57) and `mean_when_wrong` (0.55) are nearly
identical and both close to 0.5 — the floor for a 2-class problem. The model
isn't confidently right or confidently wrong; it's mostly uncertain
everywhere, which tracks with the same root cause as 1 and 2: a boundary
(`take`) it hasn't learned cleanly produces predictions clustered near
chance rather than sharp, trustworthy confidence in either direction.

---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 28 / 30 = 93%

**Disagreements**

**1.**
> *The post:* "Three of us ran the same session on the same day, different
> shoes, within 10 seconds of each other over 5 miles. I'm not saying shoes
> do nothing, but whatever they do was smaller than the noise between three
> people."
>
> *Staff said / I said:* `analysis` / `hot_take`
>
> *My call, and why:* Sticking with `hot_take`. The sample is three people on
> one session — too small for the "10 seconds" figure to really be load-
> bearing evidence. It reads like an anecdote with a number attached rather
> than an argument built from the number: the conclusion ("smaller than the
> noise between three people") is a hedge, not a claim the number rigorously
> supports.
>
> *Which pile:* (c) — the rule is ambiguous here and my reading is
> defensible. Rule 1 asks whether the fact "carries" the argument, but
> doesn't say how large the sample needs to be before a comparison counts as
> load-bearing rather than anecdotal.

**2.**
> *The post:* "Every injury thread on this forum is the same injury and the
> same three replies."
>
> *Staff said / I said:* `hot_take` / `analysis`
>
> *My call, and why:* Sticking with `analysis`. The claim names a specific,
> checkable pattern — "the same injury," "the same three replies" — not a
> vague feeling. That specificity is functioning as the writer's evidence,
> even without a formal tally attached to it.
>
> *Which pile:* (c) — the rule is ambiguous here and my reading is
> defensible. Rule 1 talks about checkable facts "doing work," but doesn't
> say whether a specific-but-uncounted pattern claim qualifies, or whether it
> needs an actual number to count as load-bearing.

**What the pattern in my disagreements tells me:** Both disagreements sit on
the same boundary — `analysis` vs. `hot_take`, not `reaction` — and both
hinge on the same gap in Rule 1: how much rigor a checkable fact needs before
it counts as load-bearing rather than decorative. The two disagreements don't
point the same direction, though: on sp_010 I was *less* willing than staff
to call something `analysis` (small sample felt too thin to me), and on
sp_020 I was *more* willing (a specific-but-uncounted pattern felt like
enough to me). That's not a one-way bias — it looks more like a threshold I
apply inconsistently in both directions depending on the post. That's useful
evidence for Milestone 4: if I'm inconsistent on exactly this kind of
judgment call even when reading someone else's carefully written rules, the
same inconsistency plausibly shows up somewhere in my own 200-label
`analysis`/`take` boundary too.

---

## The Improvement

**What I changed:** Relabeled 11 of 163 rows in `labels.csv` from `analysis`
to `take` — specifically, `analysis`-labeled posts whose "evidence" was
decorative or unquantified: a named detail with no causal link to the
verdict (e.g. "I love how Elisha Cuthbert was all about housing ribs"), or a
vague attribute claim with no concrete instance behind it (e.g. "my biggest
issue has to be the pacing," "it brought everyone full circle"). Nothing
else changed — same taxonomy, same criteria, same training settings.

**Which diagnosis pointed at it:** The `take` F1 miss and overall accuracy
miss, diagnosed partly as a labeling-consistency problem. My agreement check
against the staff set found I apply the "how much evidence counts as
load-bearing" threshold inconsistently, in both directions, on the exact
`analysis`/`take`-equivalent boundary — direct evidence the same wobble
likely existed somewhere across my own 200 labels.

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.76 | 0.76 | 0.60 | MISSED (one seed below) |
| 2. Per-label F1 (`take`/`analysis`) | `take` ≥ 0.65, `analysis` ≥ 0.55 | 0.80 / 0.70 | 0.8125 / 0.667 | 0.615 / 0.583 | MISSED (`take` misses by 0.035 in one seed; `analysis` MET in all three) |
| 3. Balance | no label > 60% | 55.2% `take` / 44.8% `analysis` (dataset-level) | — | — | MET |
| 4. Consistency | ≥ 80% agreement | 93% (unaffected by this change) | — | — | MET |
| 5. Confidence *(revised)* | most-confident-third accuracy ≥ 0.85 | 0.875 | 1.0 | 0.875 | MET |

**Did it help, and how do I know:** Yes, clearly, even though two criteria
are still technically MISSED. `take` F1 nearly doubled on average (0.369 →
0.742), the confidence criterion flipped from a flat miss (0.75 every seed)
to a clean MET (0.875–1.0), and overall accuracy improved in 2 of 3 seeds.
Criteria 1 and 2 are still missed, but only because of seed 2024, which was
oddly the *best* seed before the fix and the *worst* seed after — a flip
that's itself evidence these numbers are genuinely noisy with only 25 test
examples, not a sign the fix backfired. Seeds 42 and 7 both clear every
criterion except accuracy's margin by a hair. `analysis` F1 dropped
(0.746 → 0.650), which makes sense: the model is no longer defaulting to
`analysis` as its safe guess, so it loses a little accuracy there in
exchange for a much more balanced, genuinely learned boundary.

---

## What's Still Broken

**Criterion 1 (overall accuracy ≥ 0.65)** and **Criterion 2 (`take` F1 ≥
0.65)** are both still MISSED, though narrowly — only seed 2024 falls short
now (accuracy 0.60, `take` F1 0.615, just 0.035 under target), while seeds 42
and 7 clear both. What I'd do next: add fresh `take` examples specifically
(rather than relabeling existing ones again), since the remaining gap looks
like it's mostly about test-set size (25 examples, 11-14 per label) making
any single seed's numbers jumpy, not a deeper labeling problem — the
one-improvement rule for this unit means I'm stopping here rather than
stacking a second change on top of the relabel.

**The gap between what I meant my labels to capture and what the model
learned:** I meant `analysis` and `take` to mark whether a post's reasoning
was genuinely load-bearing or just decorative. Before the fix, the confusion
matrix showed the model hadn't learned that distinction at all — it defaulted
to `analysis` almost every time, the same failure mode the untrained baseline
showed. After relabeling the clearest decorative-evidence cases, the model
learned a real boundary (`take` F1 nearly doubled), but it's still a seed-
sensitive approximation of the rule rather than a settled one — which fits
the pattern my own agreement-check disagreements for sp_010/sp_020 point to:
the "how much evidence is enough" line is borderline-ambiguous.



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
