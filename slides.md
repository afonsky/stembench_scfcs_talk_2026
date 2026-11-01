---
# You can also start simply with 'default'
theme: seriph
addons:
  - "@twitwi/slidev-addon-ultracharger"
addonsConfig:
  ultracharger:
    inlineSvg:
      markersWorkaround: false
    disable:
      - metaFooter
      - tocFooter

background: /nn-mj-wjcpq7Ov5x.jpg

# some information about your slides (markdown enabled)
title: LLM Benchmark for Postdoc-level Natural Science Problems
subtitle: some subtitle
date: 07/11/2026
venue: HSE
author: Alexey Boldyrev
info: |
  ## LLM Benchmark for Postdoc-level Natural Science Problems
  30-minute talk, FCS Scientific Conference, HSE Voronovo, 7 November 2026.
# charts are drawn for a light background
colorSchema: light
duration: 30min
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
# transition: slide-down
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

<br>

## LLM Benchmark<br> for Postdoc-level Natural Science Problems<br><br>
### FCS Scientific Conference<br><br>
### Alina Avanesyan, <u>Alexey Boldyrev</u>, Denis Derkach, Mary Godunova, Mikhail Hushchyn, Dmitry Ilvovsky, Irina Knyazeva, Fedor Ratnikov, Ekaterina Trofimova<br><br>

#### HSE Voronovo November, 7th, 2026

<!--
Good morning. I will talk about how we test whether large language models can do real scientific work, not textbook exercises, and why we put so much effort into making those tests reproducible.
-->

---

# LLMs are acing science exams

<div class="grid grid-cols-[1.3fr_1fr] gap-10 mt-4">
<div>

<v-clicks>

- **Olympiad level.** An agentic system scored 23.5 / 30 on the IPhO 2025 theory problems, placing 14th of 406 contestants
- **Research calculations.** A Harvard theorist guided an LLM through a real calculation from start to finish, without touching a file himself
- **"AI scientists".** Autonomous agents run literature → analysis → hypothesis loops for hours. Experts judged 79% of one system's report statements accurate

</v-clicks>

</div>
<div class="flex flex-col justify-center gap-6">
  <div class="q">Can an LLM do the work of a <b>postdoc</b>?</div>
  <div v-click class="q">How do we <b>measure</b> it, so that anyone can <b>re-measure</b> it?</div>
</div>
</div>

<span class="refs">Qiu et al. 2025 (Physics Supernova) · M. Schwartz, "Vibe physics: the AI grad student", Mar 2026 · Mitchener et al. 2025 (Kosmos)</span>

<!--
The progress is real. Olympiad problems, guided research calculations, autonomous "AI scientist" loops.

So the natural question is whether an LLM can do what a postdoc does. As physicists we immediately ask a second question: how do we measure that, in a way someone else can repeat and get the same number? That second question runs through the whole talk.
-->

---

# From tool to scientist

<div class="text-base mt-2">

| Level | LLM's role | Human's role | Task scope |
|---|---|---|---|
| **1 · LLM as tool** | automates a task | allocates tasks | explicitly defined |
| **2 · LLM as analyst** | models and analyses data | defines the problem, validates the output | goal-oriented |
| **3 · LLM as scientist** | explores, discovers | minimal intervention | open-ended |

</div>

<div v-click class="callout mt-6">

**Postdoc level** sits between 2 and 3. Given a well-posed research problem and real data, the agent delivers a method the field would accept, with nobody checking each step.

</div>

<span class="refs">Levels from Zheng et al., "From Automation to Autonomy: A Survey on LLMs in Scientific Discovery", EMNLP 2025</span>

<!--
A useful vocabulary: three levels of autonomy. Literature search and idea generation are fairly mature. Closed-loop analysis on real data is not.

By "postdoc level" we mean: someone hands you a well-posed research problem and the data, and you come back with a working method that your field would accept. That is what we want to measure.
-->

---

# How do today's benchmarks decide who is right?

<div class="grid grid-cols-4 gap-3 mt-4 text-base">
  <div class="card">
    <div class="ic"><carbon-checkmark-outline /></div>
    <b>Answer key</b>
    <div class="ex">GPQA, PHYBench, UGPhysics, olympiads</div>
    <i>Is the final number right?</i>
  </div>
  <div class="card">
    <div class="ic"><carbon-terminal /></div>
    <b>Tests &amp; checks</b>
    <div class="ex">SciCode, ScienceAgentBench, Terminal-Bench-Science</div>
    <i>Does the code pass?</i>
  </div>
  <div class="card">
    <div class="ic"><carbon-chart-line /></div>
    <b>Leaderboard rank</b>
    <div class="ex">MLE-bench (Kaggle competitions)</div>
    <i>Where would you place?</i>
  </div>
  <div class="card">
    <div class="ic"><carbon-user-multiple /></div>
    <b>Rubric or LLM judge</b>
    <div class="ex">PaperBench, COMPOSITE-STEM, LifeSciBench</div>
    <i>Does it look right to a reviewer?</i>
  </div>
</div>

<div v-click class="callout mt-8">

None of these is how a scientific field decides that one method beats another. A check **accepts or rejects**. A field **orders** competing methods on a continuous figure of merit, measured on data nobody has seen.

</div>

<!--
Existing benchmarks replace the hard question with an easier one: an answer key, a unit test, a leaderboard position, or a rubric applied by a human or another LLM.

All four are useful, but none of them is how physicists actually decide that a new method is better. Two solutions can both "pass" and still differ by exactly the quantity the field cares about. And a rubric graded by an LLM cannot be re-executed bit for bit.
-->

---

# An exercise vs. a research problem

<div class="tbl mt-2">

| | Exercise | Research problem |
|---|---|---|
| **Solutions** | one correct answer | many admissible methods, *ranked* by quality |
| **Deliverable** | a number or a formula | working code and a model that generalise to unseen data |
| **Data** | fits in the prompt | 0.1–12 GB; deciding what to look at is part of the job |
| **Scoring** | right / wrong | the field's figure of merit: a sharp interval that misses loses to a wider one that covers |
| **Failures** | visible in the answer | silent: data leakage, unphysical models; only withheld data exposes them |
| **Ceiling** | 100% | empirical: ≈ 0.75 AUC is the published limit for cross-scanner autism diagnosis |

</div>

<div v-click class="mt-4 text-lg">

<carbon-idea class="inline text-[#5d8392]" /> In every one of our tasks, the gap to the expert is closed by a **domain insight**, not by more compute.

</div>

<!--
This table is the core of our problem formulation. Research problems have many admissible solutions that are ranked, not graded. The deliverable is working code, not a number. Data does not fit in a context window. The metric is the one the community uses. Failures are silent: a model can reproduce a spectrum while violating the physics, and only withheld data reveals it. And the ceiling is empirical, not 100%.
-->

---

# Key idea: let each field judge by its own rules

<div class="text-lg mt-1">

<v-clicks>

- Each problem is scored by the **figure of merit its community already uses** to compare published methods
- Two **anchors**, both obtained by *running code we ship*, never copied from a paper
- Every score is read against the interval $[b, e]$, not against a perfect 1.0

</v-clicks>

</div>

<div v-click class="mt-4 px-4">
  <AnchorAxis />
</div>

<div v-click class="text-base muted -mt-1 text-center">The expert anchor is a reference, not an upper bound. Agents can and do exceed it.</div>

<!--
Our answer: adopt the field's own criterion. Each task is scored by the metric its community uses to rank published methods.

And each task ships two anchors, measured by running code we include: a baseline that a capable engineer would write without domain knowledge, and an expert solution that reproduces a published method end to end.

The interval between them, the headroom, is the part of the problem crossed by scientific insight rather than engineering. That is what we read agent scores against.
-->

---

# Twelve research problems, three sciences

<div class="grid grid-cols-3 gap-4 mt-1 tasks">
<div class="card">
  <div class="dom">Particle physics <span>×4</span></div>
  <div class="why">big data, hypothesis-free search</div>
  <ul>
    <li>New-physics search at the LHC, no labels <em>0.33→0.91</em></li>
    <li>Higgs signal strength with honest error bars <em>0.40→0.69</em></li>
    <li>Top-quark jet tagging <em>0.06→0.88</em></li>
    <li>Anomaly detection, signal never shown <em>0.00→0.22</em></li>
  </ul>
</div>
<div class="card">
  <div class="dom">Astrophysics <span>×3</span></div>
  <div class="why">ill-posed inversion with uncertainty</div>
  <ul>
    <li>Solar magnetic fields from polarised spectra, simulated <em>0.01→0.62</em></li>
    <li>… from real Hinode telescope data <em>0.00→0.60</em></li>
    <li>Type Ia supernovae from sparse light curves <em>0.42→0.85</em></li>
  </ul>
</div>
<div class="card">
  <div class="dom">Neuroscience <span>×5</span></div>
  <div class="why">forward models, cross-site generalisation</div>
  <ul>
    <li>Autism from fMRI, on unseen scanners <em>0.00→0.31</em></li>
    <li>Identify the neural-mass model family <em>0.00→0.90</em></li>
    <li>Digital twin: predict an intervention's effect <em>0.10→0.72</em></li>
    <li>Recover hidden brain-model parameters <em>0.13→0.52</em></li>
    <li>Recover neural drive from BOLD signal <em>0.01→0.35</em></li>
  </ul>
</div>
</div>

<div class="text-sm muted mt-3"><em class="anch">b→e</em> measured baseline → expert anchor; scores in&nbsp;&#91;0,&nbsp;1&#93;, higher is better. All datasets openly licensed (Zenodo, public challenges, ABIDE data-use agreement, open solar data, in-project simulation).</div>

<!--
Twelve problems in three domains, chosen because they stress different abilities. Particle physics brings huge datasets and searches where you do not know what you are looking for. Astrophysics brings inverse problems with uncertainty. Neuroscience brings forward models and generalisation across clinical sites.

The grey numbers are the two anchors. Look at the autism task: the expert reaches only 0.31. That is not a weak expert, it is the real ceiling of cross-scanner clinical data.
-->

---

# Close-up: reading the Sun's magnetic field

<div class="grid grid-cols-[1.15fr_1fr] gap-8 mt-1">
<div class="text-[17px] leading-snug closeup">

**Task.** From a polarised spectrum (4 Stokes profiles × 56 wavelengths), recover 11 physical parameters with 80% intervals, for 10,000 pixels.

**Physics traps, by design**
- azimuth is defined only modulo 180°
- some parameters are *exactly* unobservable at singular points
- a weak field filling the pixel ≈ a strong field filling part of it

**The agent also submits its own physics model.** The grader re-synthesises spectra from the predicted parameters in an isolated container.

</div>
<div class="flex flex-col gap-4">
  <div class="card text-center py-4">

  $$\text{Score} = 0.8\,R + 0.2\sqrt{R\cdot F}$$

  </div>
  <div class="text-base">

  - **R**: calibrated parameter recovery, relative to a no-skill predictor
  - **F**: does *your* physics reproduce the observed spectrum? Scaled by physical-validity probes
  - The geometric gate means a perfect spectral fit with nonsense parameters earns nothing

  </div>
</div>
</div>

<!--
One task in detail, to show what "research problem" means in practice. Solar physicists infer the magnetic field from polarised spectra. There are built-in traps: a 180-degree ambiguity, parameters that are physically unobservable in some pixels, and degeneracies.

The agent must also submit its own forward model. Note the square root in the score: you cannot win by fitting the spectrum while getting the physics wrong. This is how we defend against gaming the metric.
-->

---

# How one evaluation run works

<div class="flex items-stretch gap-2 mt-3 flow text-base">
  <div class="card flex-1 b-task">
    <div class="hd"><carbon-document /> Task</div>
    research question in plain language<br>
    public data only<br>
    required submission format
  </div>
  <div class="arr">→</div>
  <div class="card flex-[1.4] b-agent">
    <div class="hd"><carbon-bot /> Agent in an offline sandbox</div>
    LLM in a reason → act loop<br>
    tools: <code>bash</code> · <code>python</code> · data explorer · format validator <span class="muted">(never reveals the score)</span><br>
    budget: 1 h · 10⁶ tokens · 200 messages · <b>no internet</b>
  </div>
  <div class="arr">→</div>
  <div class="card flex-1 b-grade">
    <div class="hd"><carbon-scales /> Grader, outside</div>
    withheld test split<br>
    deterministic metric <span class="whitespace-nowrap">μ ∈ &#91;0,&nbsp;1&#93;</span><br>
    + reproducibility verdict
  </div>
</div>

<div v-click class="logstrip mt-5">
  <carbon-recording class="inline" /> <b>Every run is recorded:</b> each message, tool call, token, dollar, run setting and score, in the open log format of the <i>Inspect</i> framework (UK AI Security Institute). Any run can be replayed and audited.
</div>

<div v-click class="text-base mt-4 muted">

Same system prompt, same six tools and same budget for every model and every task. Ground truth never enters the sandbox. Without internet, a problem with a published solution can't turn into a search exercise.

</div>

<!--
The mechanics. The agent reads the task and the public data, works in an offline container with a few generic tools, and submits files. The grader runs outside the container on data the agent never saw.

Two design choices matter for reproducibility. First, everything is fixed across models: prompt, tools, budget. Second, every run leaves a complete log we can replay.
-->

---
layout: statement
---

# A benchmark is a measuring instrument

<div class="text-2xl mt-6 muted">If others cannot re-measure our number, it is an anecdote, not a result.</div>

<!--
Now the part that matters most to us. We come from experimental physics, where a number without a protocol and an uncertainty is not a result. A benchmark is a measuring instrument for models, and it deserves the same standards.
-->

---

# Why LLM evaluations are hard to reproduce

<div class="grid grid-cols-2 gap-x-10 gap-y-4 mt-4 text-lg threats">
  <div><carbon-shuffle class="ti" /> <b>Stochastic models.</b> For reasoning models, our API route doesn't even expose the sampling temperature</div>
  <div><carbon-renew class="ti" /> <b>Moving targets.</b> Providers update models and routing behind the same name</div>
  <div><carbon-cloud-offline class="ti" /> <b>Fragile infrastructure.</b> Network drops, timeouts, a GPU silently missing</div>
  <div><carbon-version class="ti" /> <b>Drifting setups.</b> Package versions, data versions, task revisions, run limits</div>
  <div><carbon-user-multiple class="ti" /> <b>Subjective judges.</b> Rubric or LLM-judge scores can't be re-executed</div>
  <div><carbon-warning class="ti" /> <b>Single runs.</b> One lucky or unlucky run gets reported as "the" score</div>
</div>

<div v-click class="callout mt-8">

Each of these can move a headline number more than the difference between two models.

</div>

<!--
Why is this hard for LLMs specifically? The model itself is stochastic and, for reasoning models through our gateway, we cannot even set the temperature. Providers change models under the same name. Infrastructure fails silently. Setups drift. Judges are subjective. And people report one run.

Each of these effects can be larger than the gap between two models on a leaderboard.
-->

---

# Reproducibility by design

<div class="tbl rep mt-1">

| Threat | Our answer |
|---|---|
| <carbon-data-base class="inline"/> data drift | SHA-256 checksum for every data file; **any mismatch blocks the run** |
| <carbon-cube class="inline"/> environment drift | one container image per domain, identical for all models; network off; public data read-only |
| <carbon-certificate-check class="inline"/> numbers quoted from papers | baseline and expert **ship as runnable code**; smoke tests re-grade them and reproduce the anchors |
| <carbon-scales class="inline"/> subjective judging | **deterministic graders**, no LLM judge; written independently of the expert solution |
| <carbon-recording class="inline"/> lost evidence | full log of every run: dialogue, tool calls, tokens, cost, **run config**, score |
| <carbon-script class="inline"/> irreproducible agent output | the agent must ship a script + README that regenerates its outputs. The **validity flag is reported separately** from the score |

</div>

<!--
Here is how we answer each threat. Data are frozen by checksums, and a mismatch refuses to start the run. The environment is a container per domain, the same for every model. Anchors are not quoted from papers; we ship the code and re-run it. Grading is deterministic, with no LLM judge. Every run is logged, including the configuration it actually ran with.

The last row is easy to miss: we also require the agent's own solution to be reproducible. It must leave a script that regenerates its predictions. We report that flag separately from the score.
-->

---

# Statistics, not anecdotes

<div class="grid grid-cols-3 gap-4 mt-3">
  <div class="stat"><div class="n">288</div><div class="l">retained runs: 8 models × 12 tasks × 3 repeats</div></div>
  <div class="stat"><div class="n">±0.06 <span>/ ±0.02</span></div><div class="l">single-run spread / error of the mean, 10 valid runs of one configuration</div></div>
  <div class="stat"><div class="n">0.933 <span>vs 0.92–0.94</span></div><div class="l">same task and model on the HSE HPC cluster (Apptainer) vs a cloud VM (Docker)</div></div>
</div>

<div class="mt-6 text-lg">

<v-clicks>

- **Runs are often bimodal**: they land near the best score or at zero. 14 identical runs on top tagging ranged from 0.06 to above 0.9
- **Infrastructure failures** (model API unreachable) are flagged automatically and excluded, *but stay visible* in the report
- **Genuine zeros stay in every mean.** A failed attempt is a result too

</v-clicks>

</div>

<!--
We never report a single run. Every model–task cell has three retained repeats, 288 runs in total. For one configuration we ran ten repeats: one run scatters by about 0.06, the mean of ten by about 0.02.

We also checked that the platform doesn't matter: the same task and model on our university cluster and on a cloud machine give the same score within the spread.

Runs are often bimodal, so a single run can mislead in either direction. Infrastructure failures are separated automatically from genuine failures, and genuine zeros count.
-->

---

# Reproducibility caught us, four times

<div class="grid grid-cols-2 gap-4 mt-2 text-base lessons">
  <div class="card"><b>1 · Launch notes vs. logged config.</b> A batch described as "extended limits + GPU" had actually run with the default limits. Only the config recorded in each run revealed it.</div>
  <div class="card"><b>2 · Confounded ablation.</b> Extended budgets lifted top tagging from 0.84 to 0.94, but the provider route changed at the same time. We report it as <i>not attributable</i> to the budget.</div>
  <div class="card"><b>3 · Stale pilot.</b> An early effort sweep predates task revisions and a GPU-access fix in the containers. It is reported, but kept out of the headline comparison.</div>
  <div class="card"><b>4 · Grader audit.</b> A forward model with the wrong azimuth law (5χ instead of χ) passed the physics checks. Checks hardened, regression tests added. Released scores were <i>not</i> silently changed.</div>
</div>

<div v-click class="callout mt-5">

Each would have produced a confident, wrong conclusion. Logs and versioned task contracts turned them into footnotes.

</div>

<!--
This slide is my favourite, because it shows why the machinery is worth it. Four times our own infrastructure caught us.

A batch we believed had larger limits had actually run with the defaults; the logged configuration showed it. An ablation changed two things at once, so we refuse to claim a causal effect. A pilot predates fixes, so it stays out of the headline. And an audit of our own physics checks found a loophole, which we closed with regression tests rather than quietly rewriting published numbers.
-->

---

# Seven of twelve: an agent beats the reproduced expert

<div class="text-base muted -mt-3 mb-1">8 frontier models · high reasoning effort · mean of 3 runs · identical prompt, tools and budget</div>

<HeadroomChart />

<!--
Now the results. Each row is a task. The shaded band runs from baseline to expert; the dark tick is the expert. Grey dots are the eight models, the blue dot is the best one.

On seven of twelve tasks, at least one model's mean exceeds the reproduced published method: new-physics search, top tagging, Dark Machines, both solar inversions, supernova classification and neural-mass family identification. On five tasks every model stays below.
-->

---

# Where agents still fall short

<div class="tbl short mt-2">

| Task | Best mean | Expert | What it requires |
|---|---|---|---|
| Higgs signal strength with error bars | 0.48 | 0.69 | **calibrated uncertainty** |
| Brain-model digital twins | 0.31 | 0.72 | **causal** prediction of an intervention |
| Brain-model parameter recovery | 0.42 | 0.52 | **latent** inference through a noisy channel |
| BOLD deconvolution | 0.19 | 0.35 | **latent** inference, no stimulus timing |
| Autism from fMRI, unseen scanners | 0.29 | 0.31 | **cross-site** generalisation |

</div>

<v-clicks>

- The hard part is exactly what **only withheld data can reveal**: honest uncertainty, causal response, hidden parameters, unseen sites
- On digital twins the best agent is still closer to the baseline (0.10) than to the expert
- Single runs do cross the expert on two of these (autism 0.314, parameters 0.583), so here **reliability** is the gap, not ability

</v-clicks>

<!--
The five remaining gaps share a pattern. They are about uncertainty estimation, causal and latent inference, and generalisation to new clinical sites. These are precisely the failures you cannot see by looking at the output; only held-out data exposes them, and that is why an LLM judge reading a report would miss them.

On two of these, individual runs do cross the expert line, so the problem is consistency.
-->

---

# There is no "best model for science"

<div class="grid grid-cols-[1.35fr_1fr] gap-8 mt-1">
<div>
  <div class="text-sm muted mb-1">Domain mean score (outlined: domain leader)</div>
  <DomainHeatmap />
</div>
<div class="text-base flex flex-col gap-3 pt-5">

- **DeepSeek V4 Pro** leads particle physics
- **Gemini 3.1 Pro** leads astro and neuro
- **Claude Opus 5** leads 5 tasks, yet no domain
- Gemini's own domains span 0.32–0.84

</div>
</div>

<div class="mt-5">
  <div class="text-sm muted mb-1">Share of variation across the 96 task × model means</div>
  <VarianceBar />
  <div v-click class="text-lg mt-3"><b>Which task</b> explains 5× more than <b>which model</b>. One aggregate leaderboard number hides this.</div>
</div>

<!--
Task wins and domain leadership come apart. DeepSeek leads particle physics, Gemini leads astrophysics and neuroscience, and Claude wins five individual tasks but leads no domain.

A simple variance decomposition makes the point quantitatively: 60% of the variation is explained by the task, only 12% by the model. So a single-number leaderboard for "science" is mostly measuring which tasks it happens to contain.
-->

---

# Ability vs. reliability

<div class="grid grid-cols-[1.1fr_1fr] gap-8 mt-2">
<div>
  <div class="text-sm muted mb-1">Particle-physics domain score per model</div>
  <ReliabilityDumbbell />
</div>
<div class="text-base flex flex-col gap-3">

<v-clicks>

- On their **best** run, 6 of 8 models reach 0.63–0.71, the level of the expert anchors
- The spread between models comes mostly from **how often they fail**, not whether they can solve the task
- **Score is not validity.** 15 of 88 zero-score runs still delivered a valid, reproducible package. We report both
- Time cap hit: 42% of DeepSeek runs, 39% of MiMo, 6% of Gemini Flash. We measure capability **under a fixed budget**

</v-clicks>

</div>
</div>

<!--
Separating the mean from the best of three runs is revealing. Six of eight models can reach roughly the expert level in particle physics on a good run. What separates them is reliability.

And because we record validity separately, we know that a zero is not automatically a crash: some zero-score runs produced a perfectly valid, reproducible package that simply did not beat the baseline. Budgets also bind differently for different models, which is why we state that we measure capability under a fixed wall-clock budget.
-->

---

# Conclusions

<v-clicks>

1. **Science has no answer key.** Judge agents the way a field judges methods: its own figure of merit, plus two measured anchors
2. Frontier agents already **beat reproduced published methods on 7 of 12** research problems
3. They still fall short on **uncertainty, causal and latent inference, cross-site generalisation**
4. There is **no single "best model for science"**: the task explains 60% of the variation, the model 12%
5. **Reproducibility is the backbone**: frozen data, fixed containers, deterministic graders, full logs, repeated runs, and an honest record of what went wrong

</v-clicks>

<!--
To summarise. Scientific problems need scientific criteria, anchored at both ends. With those criteria, frontier agents already beat reproduced published methods on most of our tasks, but they fail where science is hardest to fake: uncertainty, causality, hidden variables and generalisation. Rankings are task-dependent. And none of these statements would mean anything without the reproducibility machinery behind them.
-->

---

# Limitations and next steps

<div class="grid grid-cols-2 gap-10 mt-3 text-base">
<div>

### Honest limitations

- 3 runs per cell: stable rankings need more repeats and confidence intervals
- Agreement of the metrics with **blinded expert rankings** is not yet measured (target: Kendall τ ≥ 0.85)
- One agent design evaluated; the effort sweep covers one model only
- Container images are fixed per domain, but exact package digests are not yet frozen

</div>
<div>

### Next

- **Open release** of tasks, graders, anchors and harness with the paper
- An agent with a curated literature corpus per task
- Matched-route budget ablations
- New domains through the same task contract
- **Student projects (FCS):** checking agents' claims against their own logs · context budgets · red-teaming the metrics

</div>
</div>

<!--
Limitations we state openly: three runs per cell is not enough for confident rankings; we still have to show that our automatic metrics agree with blinded expert rankings; and we want to freeze the container digests.

Next: the open release with the paper, the literature-enabled agent, cleaner ablations, more domains. And for FCS students: we have well-defined thesis projects on top of the run logs and the metrics.
-->

---
layout: cover
background: /photo_2025-04-09_09-28-52.jpg
class: text-center
---

# Thank you!

### Questions?

<div class="mt-10 text-lg opacity-80">

Every number in this talk can be traced to a logged, re-runnable experiment.

</div>

---

# Backup: per-task scores for all models

<div class="flex gap-6 items-start">
  <img src="./images/scores_per_task.png" class="h-[420px]" alt="Per-task raw scores of eight models with baseline and expert anchors" />
  <div class="text-sm muted mt-8 max-w-[260px]">

  Marker: mean of 3 retained runs (zeros included). Bars: min–max of the 3 runs, not confidence intervals. Dashed line: baseline *b*; solid line: expert *e*. Basic agent, high reasoning effort.

  </div>
</div>
