# AI Detectors and Stylometry Reference

A technical analysis of how modern AI detection and watermarking systems operate, what zero-LLM stylometry can measure, and what can legitimately be claimed. Derived from [guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover) and contemporary detection research.

---

## 1. Two Mechanically Different Detector Families

AI detection systems fall into two distinct engineering paradigms. A technique that lowers probability under one family is often ineffective against the other.

```
┌─────────────────────────────────────────────────────────────┐
│                    AI DETECTION SYSTEMS                     │
├──────────────────────────────┬──────────────────────────────┤
│  Family A: Statistical       │  Family B: Trained Neural    │
│  Zero-Shot Profilers         │  Classifiers                 │
├──────────────────────────────┼──────────────────────────────┤
│ • Perplexity metrics         │ • Fine-tuned transformers    │
│ • Burstiness (length & PPL)  │ • Hard-negative mining       │
│ • Binoculars (cross-PPL)     │ • GPTZero, Pangram, Turnitin │
│ • DetectGPT (perturbations)  │ • Sentence-level classifiers │
└──────────────────────────────┴──────────────────────────────┘
```

### Family A: Statistical / Zero-Shot Detectors

These models evaluate the statistical properties of a text under a reference language model without being trained on human-vs-AI classification labels:

1. **Perplexity (PPL)**:
   - Evaluates how "surprised" a language model is by the sequence of words.
   - Because standard LLMs emit high-probability tokens during greedy or top-$p$ decoding, raw AI text exhibits consistently low perplexity.
   - *Mitigation*: Introduce precise, domain-specific human diction and idiomatic expressions that raise perplexity naturally without sacrificing clarity.
2. **Burstiness**:
   - Measures the variation in sentence length and perplexity across a document.
   - Human prose is "bursty": short, punchy 3-word assertions are juxtaposed against complex, 30-word compound sentences. AI text tends toward metronomic regularity (15–20 words per sentence).
   - *Mitigation*: Break rhythmic monotony intentionally (stop-slop rule #35).
3. **DetectGPT (Mitchell et al., 2023)**:
   - Assumes AI text sits at a local maximum of the model's log-probability function. It creates minor perturbations of the text and checks whether the original text had strictly higher log-probability than its neighbors.
   - *Mitigation*: Reconstruct the argument logic and syntactic dependencies rather than merely swapping isolated words.
4. **Binoculars (Hans et al., 2024, arXiv:2401.12070)**:
   - Uses the ratio between an observer model's log-perplexity and the cross-perplexity between two distinct model families:
     $$\text{Binoculars}(x) = \frac{\log \text{PPL}_{M_1}(x)}{\text{CrossPPL}_{M_1, M_2}(x)}$$
   - Demonstrates high discrimination accuracy and robustness to simple surface paraphrasers.
   - *Mitigation*: Coherent, opinionated human voice choices (Style Mirroring) that alter the cross-model probability landscape.

### Family B: Trained Neural Classifiers

These systems (e.g. GPTZero, Pangram, Turnitin, Copyleaks) train deep neural networks on millions of human-authored and AI-generated texts:

- They learn latent features, embedding geometries, and n-gram associations directly from massive datasets.
- Advanced classifiers employ **hard-negative mining**: training against commercial paraphrasers and humanizer tools so that generic "paraphrase-style" edits are classified as AI.
- *What works against Family B*:
  - Matching the idiosyncratic, messy, authentic distribution of a genuine human writer (via a 12-dimension Style Profile extracted from a verified sample).
  - Removing formulaic transitions, corporate jargon, and metadiscourse.
  - Allowing genuine opinions, emotional texture, and first-person perspectives.

---

## 2. Watermarking is an Orthogonal Axis

Watermarks are **not** detectors of generic "AI-ness." They are mathematical signatures stamped at inference time by the model provider:

1. **Kirchenbauer Green/Red List (2023)**:
   - The generator hashes the previous token to bias sampling toward a pseudo-random green list. Detection checks for a statistically anomalous surplus of green-list tokens.
2. **Google SynthID-Text (2024)**:
   - Tournament sampling from the same red/green family, designed to be less perceptible and preserve generation quality.
   - *Status*: Google retired native SynthID text watermarking from current Gemini Generative Language API endpoints (mid-2026), but it remains deployed in specialized enterprise models.
3. **Claude Embedded Watermarks**:
   - Anthropic embeds statistical text watermarks at the model level across models launched on or after August 2026.
   - Detection relies on internal Anthropic verification services.

**Key Insight**: Because statistical watermarks depend on preceding-token hash transitions, **any substantial rewrite that alters token order, clause order, and sentence boundaries dilutes the watermark**. However, absence cannot be mathematically proven without the private vendor key.

---

## 3. Zero-LLM Stylometry Metrics

The mathematical signals that can be measured deterministically without calling an external language model include:

### 1. Sentence-Length Burstiness (Coefficient of Variation)

Calculates the standard deviation of sentence lengths divided by the mean:
$$CV = \frac{\sigma_{\text{length}}}{\mu_{\text{length}}} = \frac{\sqrt{\frac{1}{N}\sum_{i=1}^N (L_i - \bar{L})^2}}{\bar{L}}$$

- **AI Text Baseline**: $CV \approx 0.25 - 0.35$ (uniform, monotonous cadence).
- **Human Prose Baseline**: $CV \ge 0.45 - 0.65$ (dynamic cadence, varied lengths).
- **Target**: $CV \ge 0.55$ (for resilient human cadence).

### 2. Moving-Average Type-Token Ratio (MATTR)

Standard Type-Token Ratio ($TTR = \frac{\text{unique tokens}}{\text{total tokens}}$) degrades severely as document length grows. MATTR calculates TTR across sliding windows of fixed size $W$ (typically $W = 50$ words):
$$\text{MATTR}_W = \frac{1}{N - W + 1} \sum_{i=1}^{N - W + 1} \frac{|\text{unique}(w_i, \dots, w_{i+W-1})|}{W}$$

- **AI Text Baseline**: Drops rapidly due to synonym cycling and repetition penalties.
- **Human Prose Baseline**: High local lexical density.
- **Target**: $\text{MATTR}_{50} \ge 0.75$.

### 3. Weighted AI-Cadence Phrase Density

Measures the frequency of formulaic AI transition n-grams and throat-clearing signposts per 100 words:
$$\text{Cadence Density} = \frac{\sum \text{weight}(p) \times \text{count}(p)}{\text{Word Count} / 100}$$

Where phrases like *"in conclusion"*, *"serves as a testament to"*, *"delve into"*, *"it is important to note"*, and *"not only... but also"* carry calibrated weights.
- **High AI Tier**: Density $> 0.60$ per 100 words.
- **Clean Human Target**: Density $< 0.12$ per 100 words.

---

## 4. What Research Proves Actually Defeats Detectors

### The DIPPER Benchmark (Krishna et al., 2023)

Research on paraphrasing and AI detection (Krishna, Song, Raghavan, Wieting, Iyyer; 2023) systematically evaluated detection evasion across GPTZero, DetectGPT, and watermarking schemes. The core findings:

1. **Surface word swaps are ineffective**: Removing individual tells like "delve" or em dashes produces almost zero movement against deep neural classifiers because the sentence embedding geometry remains in the AI distribution.
2. **Adversarial perturbations are brittle**: Micro-optimizing specific tokens against a known model's gradient or decision boundary is fragile. The moment the detector retrains or the text is scanned by a different vendor (e.g. GPTZero vs Turnitin), the evasion fails.
3. **Deep Structural Paraphrasing is durable**: Rebuilding sentences with high **Order Diversity ($O$)** and high **Lexical Diversity ($L$)** breaks detection across *all* model families simultaneously. Changing the syntactic dependency tree and clause order while diversifying vocabulary across every token position neutralizes:
   - **GPTZero / Pangram**: Sentence-level transformer embeddings no longer match training clusters.
   - **DetectGPT / Binoculars**: Log-probability local curvature and cross-perplexity ratios flatten.
   - **SynthID / Kirchenbauer**: Preceding-token green-list dependencies ($hash(t_{i-1})$) are scrambled.

### Cross-Family Evasion Matrix

| Detector System | Core Signal Measured | What Fails Against It | What Successfully Defeats It | Target Metric |
|---|---|---|---|---|
| **GPTZero / Pangram / Turnitin**<br>(Neural Classifiers) | Sentence embedding clusters, AI syntactic templates, transition n-grams | Synonym swaps, removing adverbs, changing punctuation | **DIPPER Structural Resynthesis**: Invert main/subordinate clauses, eliminate formulaic openers, inject authentic human voice signals | Sentence embedding distance from AI centroid; 0/42 pattern tells |
| **Binoculars**<br>(Cross-Perplexity Ratio) | Ratio of observer model PPL to cross-PPL between two architectures | Random thesaurus swaps (creates ungrammatical perplexity spikes) | **High Lexical Diversity ($L$)**: Natural, high-information human domain terminology and coherent idioms | Balanced human cross-perplexity ratio |
| **DetectGPT / PPL Profilers**<br>(Probability Curvature) | Uniform low perplexity; local probability maximums under perturbation | Superficial grammar smoothing, re-phrasing single lines | **High Order Diversity ($O$)**: Asymmetrical sentence structures, varied sentence lengths, split/merge clauses | Burstiness $CV \ge 0.55$ |
| **SynthID-Text / Kirchenbauer**<br>(Inference Watermarks) | Pseudo-random green-list token surplus keyed on preceding tokens | Replacing adjectives or nouns within identical clause structures | **Clause & Sequence Inversion**: Reordering clauses scrambles preceding-token hashes ($t_{i-1}$) completely | Dilution of green-list token concentration to baseline chance ($\approx 50\%$) |
| **Layer A Steganography**<br>(Invisible Codepoints) | Zero-width spaces (`U+200B`), bidi overrides, tag characters, homoglyphs | Text paraphrasing (invisible characters survive copy-paste) | **Deterministic Unicode Scrub**: Strip invisible carriers while preserving emoji glue and orthography | 0 non-load-bearing invisible codepoints |

---

## 5. Legitimate Claims vs. Evidentiary Honesty

When evaluating text or discussing detector resilience:

1. **The Arms Race Reality**: AI detection is an evolving arms race. A paraphrasing pattern that fools a classifier today may eventually be incorporated into its training set. Durable resilience requires genuine human structural variety, not templated "anti-detection tricks."
2. **Preserve Exact Meaning**: Heavy structural paraphrasing must never sacrifice accuracy. The Karpathy guardrail guarantees that every fact, number, citation, and nuance survives intact.
3. **Evidence Over Guarantees**: Report measurable structural movements: *"Raised burstiness $CV$ from 0.28 to 0.58, elevated $\text{MATTR}_{50}$ to 0.78, eliminated all 42 patterns, and restructured clause dependencies across all sentences."*
4. **Responsible Use**: Built for personal privacy, technical hygiene, and restoring authentic authorial voice on content the user owns.

