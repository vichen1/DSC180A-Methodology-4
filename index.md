---
layout: default
---

# Victor Chen

**Email:** vic006@ucsd.edu

**Section:** D37 — Autonomous, Multimodal Data Ecosystem for Enterprise Intelligence

**Mentors:** Renu Raman and Jack Brzeznski

---

**What is the most interesting topic covered in your domain this quarter?**

Using LLM agents to infer what a dataset's columns mean from metadata alone, and figuring out where that knowledge actually comes from. Our semantic agent described 27 of 31 columns correctly, but on the heart dataset it stated code meanings (like chest pain type 1 = typical angina) that the column names and value ranges couldn't support, because the model recognized the well-known UCI Heart Disease dataset. On the cardio dataset, where the same concepts have different column names, it flagged 7 of 15 columns as uncertain compared with 3 of 16 on heart. Separating what a model infers from evidence and what it remembers from training turned out to be the central question in evaluating these agents.

**Describe a potential investigation you would like to pursue for your Quarter 2 Project.**

Automatically aligning columns across datasets so they can be queried together. Our current relationship finder only matches identical column names, so it found 3 of the 14 equivalent pairs between the heart and cardio datasets and missed pairs like `sex`/`gender`, `trestbps`/`restingBP` and `thalach`/`maxheartrate`. I'd like to build a matching step that combines the semantic agent's descriptions with evidence from the data itself, such as value ranges and code sets, to propose column pairs and detect when matched columns are coded differently (chest pain is 1–4 in one dataset and 0–3 in the other). I would evaluate it against our 14-pair crosswalk, then on a less familiar pair of datasets to see how much accuracy drops when the model can't rely on memory. If that goes well, the aligned schema could support asking one question across both datasets at once.

**What is a potential change you'd make to the approach taken in your current Quarter 1 Project?**

I'd change the metadata agent to count values for every low-cardinality column regardless of data type, and to report zeros and other placeholder values. Most of the data-quality problems it missed (3 of 7) come from categorical columns stored as numbers, so rare or undocumented codes like `slope = 0` and the 53 cholesterol readings of 0 never show up. I'd also run the semantic agent several times per column instead of once, since it doesn't fix its randomness, and report how consistent its answers are.

**What other techniques would you be interested in using in your project?**

- Embedding similarity between column descriptions to propose candidate matches across datasets.
- Distribution comparisons (overlap of value sets, range similarity, statistical tests) to confirm or reject those matches.
- Structured output with a fixed vocabulary for `semantic_type` and a `{code: label}` field for value codes, so downstream code can rely on the agent's output.
- Retrieval of data dictionaries and cleaning notes as context, so the agent grounds its answers in documentation instead of memory.
- A memorization test that renames or anonymizes columns to measure how much the agent depends on recognizing familiar datasets.
