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

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->

This project classifies discussions from r/edmproduction (specifically threads centered on electronic and house music production, mixing, and sound design). It sorts user comments and text posts into three operational categories: actionable technical analysis, dogmatic hot takes, and personal/conversational reactions. The goal is to separate substantive engineering and sound-design advice from casual social commentary and prescriptive opinion.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `label_one`
`analysis`
**Definition:**
Provides verifiable, step-by-step technical guidance, cites specific numerical parameters (dB, Hz, kHz, specs), or breaks down an objective workflow method.
**Example 1:**
> "First off, you’re not alone on this confusion. Good for you to take the time to learn! Regarding your question, the sample rate must be doubled in order to prevent aliasing which sounds bad. This is based off of the Nyquist Theory which essentially is a mathematical rule for converting analog audio to digital signals or vise versa."

**Example 2:**
> "I got a 16” 2021 MacBook M1 Pro for about $1100 after tax from eBay. 1TB hard drive, 32 GB RAM, M1 Pro chip. They have a lot of good trusted sellers that come with a one year warranty."

### `label_two`
`hot_take`
**Definition:**
Delivers a blunt, prescriptive opinion, universal rule, or strong workflow dogma without empirical breakdown or nuanced context.
**Example 1:**
> "Clip drums, limit or clip mid bass, leave sub alone. If it’s not loud enough turn sub down and mid bass up. Leave master alone except for maybe clipper or limiter just for volume."

**Example 2:**
> "Stop buying third party plugins. Ableton stock plugins are objectively better than 90% of Waves or FabFilter tools if you actually know what the knobs do."

### `label_three`
`reaction`
**Definition:**
Expresses subjective emotion, personal learning struggles, conversational agreement/acknowledgment, or casual praise.
**Example 1:**
> "Ahhh i miss the days of spending hours on the most basic things...I think most people start the same way you are. My advice when you start is dont try to recreate exactly what's in your mind, my best work comes from experimenting and finding things that just happen to sound good."

**Example 2:**
> "I tried to start with Ableton and had the same problem figuring out basic operations. I hadn't realized there were better tutorials available. I found FL Studio much easier to get started with and didn't need tutorials in the beginning. Some people have exactly the opposite experience, just depends on how they're wired."


### The hardest boundary

**Which two labels:** `hot_take` vs. `analysis`

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->
If a post gives prescriptive advice or asserts a rule of thumb, it is classified as `analysis` only if it provides a specific, verifiable parameter or explains *why/how* the DSP/physics works (e.g., citing exact dB/Hz targets, filter slopes, or Nyquist/aliasing principles). If it delivers imperative rules or commands without verifiable numbers or explanatory mechanics (e.g., "always clip your drums", "never put a limiter on master"), it is classified as `hot_take`, regardless of how confident or accurate the advice sounds.


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
Public top-level comments and self-posts collected from Reddit's `r/edmproduction` community, focused on discussion threads concerning house music production, mixing workflows, sound design, and DAW setups.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
I labeled the first 20 posts completely cold (manually without tools or assistance) to establish ground truth and verify taxonomy boundaries. The remaining 180 posts were pre-labeled using an AI prompt containing the exact label definitions and decision rules, followed by manual review where I read and verified or corrected every individual classification. Rows are marked `cold` and `pre-labelled; checked` in the `note` column.

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| `analysis` | 67 | 33.5% |
| `hot_take` | 67 | 33.5% |
| `reaction` | 66 | 33.0% |
| **Total** | 200 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* "Small amounts of clipping at multiple stages. Clip your Snare track, clip your drum bus, clip your master. True peak limiter can help - many of us use FabFilter's Pro L2. Saturate low end instruments and lower the resulting harsh brightness with a high shelf. EQ out some of the muddy 250-500hz range. Check your low end with a spectrum analyzer and make tame any wild bass frequencies with a multiband compressor. That's all I got for now."

> *Could have been:* `analysis` or `hot_take`

> *I chose, because:* I chose `hot_take`. While it mentions specific frequency ranges (250–500 Hz) and plugins, the post reads as a rapid-fire list of prescriptive workflow rules ("clip this, clip that, saturate this") without explaining the acoustic mechanics, why those specific stages need clipping, or how to set the parameters. Under the decision rule, rapid prescriptive dogma without empirical grounding falls under `hot_take`.

**2.**
> *The post:* "This has nothing to with depth, this is specifically what makes mixes flat. It's easy as shit to get loud, depth is not."

> *Could have been:* `hot_take` or `reaction`

> *I chose, because:* I chose `reaction`. While the claim borders on a hot take regarding loudness versus depth, the tone is an immediate, frustrated conversational counter-response to another commenter rather than an unprompted standalone thesis or broad workflow rule.

**3.**
> *The post:* "Soothe 2 has the most insane algorithm for cleaning up sounds. As a heavy dubstep producer I use this in a lot of places, including a soft one on the master. It really is that good, and it's nearly impossible to replicate it properly"

> *Could have been:* `reaction` or `hot_take`

> *I chose, because:* I chose `hot_take`. Although it describes personal usage ("as a heavy dubstep producer I use this"), the central premise makes an assertive, unqualified claim about a commercial tool's algorithm and indispensability ("nearly impossible to replicate it properly") rather than merely expressing personal emotion or beginner confusion.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** `distilbert-base-uncased`

**Settings:** <!-- epochs, learning rate, batch size, seed -->
* Epochs: 3
* Learning Rate: 2e-5
* Batch Size: 16
* Max Token Length: 128
* Seed: 42

**Anything I changed from the defaults, and why:** None. Kept the starter default hyperparameters to establish an unperturbed baseline for comparing against Unit 6 multi-seed runs.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->

* Train: 140 posts (70%)
* Validation: 30 posts (15%)
* Test: 30 posts (15%)
* Per-label counts in test split:
  * `analysis`: 10
  * `hot_take`: 10
  * `reaction`: 10
* All labels have 10 examples in the test split, which is above the 8-example threshold, minimizing extreme single-sample variance during evaluation.

---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* Generated 8 synthetic edge-case posts sitting directly on the boundary between `hot_take` and `analysis` to stress-test whether our taxonomy definitions were deterministic.
- *What came back:* Examples mixing prescriptive recommendations with technical terminology (e.g., sidechain compression advice, FabFilter recommendations, and monitoring guidelines).
- *What I changed:* Refined the decision rule to strictly state that imperative workflow commands belong to `hot_take` unless they cite an explicit checkable parameter (dB, Hz, kHz, latency samples) or explain the underlying acoustic/DSP theory.

**Moment 2**

- *What I asked for:* Detected and resolved parsing errors in `labels.csv` where unquoted commas inside raw Reddit comments broke the 3-column CSV schema (`text,label,note`).
- *What came back:* An RFC 4180 compliant CSV formatting fix wrapping all multiline and comma-separated post texts in standard double quotes.
- *What I changed:* Re-exported the cleaned dataset, verifying that all 200 posts parse cleanly into Pandas without column misalignment.

**Pre-labelling disclosure:** The first 20 rows were labeled entirely cold without any AI assistance to establish ground truth. The remaining 180 rows were pre-classified by an LLM prompted with the three label definitions and decision rules. Following automated pre-labeling, every single item was individually reviewed, audited, and corrected by hand to maintain strict boundary adherence. Rows are explicitly marked as `cold` or `pre-labelled; checked` in the `note` column.

<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.500 | 0.800 | +0.300 |
| Macro F1 | 0.423 | 0.795 | +0.373 |
| F1 — `analysis` | 0.706 | 0.818 | +0.112 |
| F1 — `hot_take` | 0.000 | 0.818 | +0.818 |
| F1 — `reaction` | 0.563 | 0.750 | +0.187 |


**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
     I predicted that the trained model would beat the zero-shot baseline most substantially on `hot_take` and `analysis` F1 scores, because distinguishing between dogmatic workflow imperatives ("always clip the master") and evidence-backed technical guidance ("cut 250 Hz with a Q of 1.2") relies heavily on recognizing domain-specific community idioms and technical thresholds that a general zero-shot model without fine-tuning often conflates.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->
     The massive +0.300 gain in overall accuracy (0.500 to 0.800) and +0.373 gain in Macro F1 (0.423 to 0.795) are almost entirely driven by the baseline completely collapsing on `hot_take` (F1 score of 0.000 vs. 0.818 trained). 

     Without fine-tuning on community data, the general baseline model could recognize obvious technical explanations (`analysis`, 0.706 F1) and emotional venting (`reaction`, 0.563 F1), but it was incapable of identifying the intermediate boundary where unbacked, prescriptive advice is delivered with high confidence. The baseline consistently sorted dogmatic assertions into either `analysis` (because they talked about production gear/plugins) or `reaction` (because of aggressive, opinionated tone). Fine-tuning on 140 training examples gave the classifier the precise boundary signal needed to operationalize workflow dogma as `hot_take`, turning a completely unlearned category into a high-performing label (0.818 F1).


---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1. Overall accuracy | ≥ 0.65 | 0.800 | 0.833 | 0.767 | MET |
| 2. Macro F1 | ≥ 0.60 | 0.795 | 0.828 | 0.743 | MET |
| 3. F1 — `analysis` | ≥ 0.60 | 0.818 | 0.952 | 0.800 | MET |
| 4. F1 — `reaction` | ≥ 0.60 | 0.750 | 0.750 | 0.857 | MET |
| 5. F1 — `hot_take` | ≥ 0.50 | 0.818 | 0.783 | 0.571 | MET |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted | analysis | hot_take | reaction |
|---|---|---|---|
| **analysis** | 9 | 1 | 0 |
| **hot_take** | 1 | 9 | 0 |
| **reaction** | 2 | 2 | 6 |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->

     The largest off-diagonal value is tied at 2, where real `reaction` posts were misclassified as `analysis` (2 posts) and as `hot_take` (2 posts), while zero real `analysis` or `hot_take` posts were predicted as `reaction`. This asymmetric leakage shows that when users share personal setup stories or studio anecdotes containing equipment names, the model frequently misreads those domain keywords as technical evidence (`analysis`) or opinionated rules (`hot_take`), whereas it never mistakenly dilutes true technical advice into casual reactions.

---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** 27 / 30 = 90%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:* "Times are down about 3% since the shoe rules changed, so obviously the shoes are the whole story and the sport has a problem."
>
> *Staff said / I said:* Staff said `hot_take` / I said `analysis`
>
> *My call, and why:* Staff was right. I saw "3%" and "shoe rules changed" and immediately tagged it as `analysis`. But Decision Rule 1 explicitly states that the fact must carry the argument rather than decorate it. The leap to "obviously the shoes are the whole story and the sport has a problem" is an unargued, sweeping conclusion that uses the percentage merely as rhetorical decoration. I applied the rule loosely by relying on keyword recognition instead of assessing whether the fact was truly load-bearing.
>
> *Which pile:* (a) the rule covered it and I applied it loosely

**2.**
> *The post:* "Wind was so bad on the out section I was leaning at about 20 degrees. Coming back felt like cheating."
>
> *Staff said / I said:* Staff said `reaction` / I said `hot_take`
>
> *My call, and why:* The taxonomy rules have an unaddressed gap when figurative exaggeration describes physical conditions. Staff labeled it `reaction` because it describes the writer's immediate feeling during the run. I labeled it `hot_take` because "Coming back felt like cheating" asserts an unsupported claim about the course conditions, and "leaning at about 20 degrees" presents a pseudo-quantitative assertion. The rules do not specify whether figurative numbers used to convey mood default to emotional reporting or unsupported factual claims.
>
> *Which pile:* (b) the rule genuinely doesn't say

**3.**
> *The post:* "Carbon plates aren't the whole story. My times dropped 2% in the shoes and another 2% the following year in the same pair, when the only thing that changed was going from 35 to 50 miles a week."
>
> *Staff said / I said:* Staff said `analysis` / I said `hot_take`
>
> *My call, and why:* My reading is defensible. While the author provides explicit numbers (2% drop, 35 to 50 miles a week), the thesis is a broad generalized conclusion about athletic shoe technology ("Carbon plates aren't the whole story") inferred entirely from an $n=1$ personal anecdote over two years with unisolated variables. Under the core definition, a single runner's retrospective mileage change is subjective self-reporting rather than an objective, checkable comparative test. Treating an unverified individual testimonial as definitive analysis blurs the line between personal hot takes and rigorous technical breakdown.
>
> *Which pile:* (c) the rule is ambiguous here and my reading is defensible

**What the pattern in my disagreements tells me:**
My disagreements reveal two clear patterns:
1. I have an initial bias toward treating numerical indicators (percentages, degrees) as `analysis` on first pass, even when those numbers function as narrative hyperbole or decorative flourishes rather than rigorous proof (as shown in post 1).
2. The boundary between an unproven personal narrative and an objective technical case study (post 3) is inherently ambiguous when an author uses precise numbers to argue a general premise. This explains why my own model had trouble distinguishing between `reaction` and `analysis` in the confusion matrix when community members discussed their studio gear and audio setups.



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



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
