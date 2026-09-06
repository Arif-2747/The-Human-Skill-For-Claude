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
- **Target**: $CV \ge 0.45$.

### 2. Moving-Average Type-Token Ratio (MATTR)

Standard Type-Token Ratio ($TTR = \frac{\text{unique tokens}}{\text{total tokens}}$) degrades severely as document length grows. MATTR calculates TTR across sliding windows of fixed size $W$ (typically $W = 50$ words):
$$\text{MATTR}_W = \frac{1}{N - W + 1} \sum_{i=1}^{N - W + 1} \frac{|\text{unique}(w_i, \dots, w_{i+W-1})|}{W}$$

- **AI Text Baseline**: Drops rapidly due to synonym cycling and repetition penalties.
- **Human Prose Baseline**: High local lexical density.
- **Target**: $\text{MATTR}_{50} \ge 0.72$.

### 3. Weighted AI-Cadence Phrase Density

Measures the frequency of formulaic AI transition n-grams and throat-clearing signposts per 100 words:
$$\text{Cadence Density} = \frac{\sum \text{weight}(p) \times \text{count}(p)}{\text{Word Count} / 100}$$

Where phrases like *"in conclusion"*, *"serves as a testament to"*, *"delve into"*, *"it is important to note"*, and *"not only... but also"* carry calibrated weights.
- **High AI Tier**: Density $> 0.60$ per 100 words.
- **Clean Human Target**: Density $< 0.15$ per 100 words.

---

## 4. Legitimate Claims vs. Evidentiary Honesty

When discussing detection or evaluating text:

1. **Never promise 100% detection evasion**: Commercial classifiers frequently shift internal thresholds, retrain on new data, and produce false positives on human prose (especially non-native speakers).
2. **Report measurable movements**: It is accurate to state: *"Removed 8 zero-width codepoints, raised sentence burstiness CV from 0.28 to 0.52, and eliminated all 42 documented AI phrasing patterns."*
3. **Detect mode provides evidence, not verdicts**: Flagging patterns and measuring burstiness gives the user verifiable evidence to inspect, not a binary or probabilistic judgment of human vs. machine authorship.
4. **Preserve ethical boundaries**: This skill is built for authorial hygiene, personal privacy, and voice restoration on text the user owns or is authorized to edit.
