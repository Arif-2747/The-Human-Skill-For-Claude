# Structural Paraphrasing Reference (DIPPER Paradigm)

Operational guide for defeating modern AI detectors (GPTZero, Binoculars, DetectGPT, Turnitin, and inference watermarks) using deep structural paraphrasing, derived from research by Krishna et al. (2023: *Paraphrasing evades detectors of AI-generated text*).

---

## 1. Why Surface Edits Fail Against Modern Detectors

Most humanizing techniques focus on **surface-level edits**:
- Swapping vocabulary (e.g. replacing "delve" with "explore", "tapestry" with "blend").
- Deleting em dashes, adverbs, and transition markers.
- Removing common corporate clichés.

While these clean up prose style, **they fail to defeat modern AI detectors**:

1. **Neural Classifiers (GPTZero, Pangram, Turnitin)** evaluate entire sentence embedding spaces and syntactic transition probabilities. Swapping isolated words leaves the underlying sentence tree and token transition graph intact.
2. **Zero-Shot / Statistical Profilers (Binoculars, DetectGPT, Perplexity)** evaluate cross-entropy and probability distributions across the entire sequence. Surface swaps do not alter the low-perplexity, uniform-cadence signature.
3. **Statistical Watermarks (Kirchenbauer et al., SynthID-Text)** key on preceding token transitions ($hash(t_{i-1}, K)$). If word order and clause topology remain the same, green-list token concentrations survive.

### The DIPPER Finding (Krishna et al., 2023)

Krishna et al. demonstrated that AI detectors across all families can be systematically neutralized without semantic loss by controlling two orthogonal dimensions:

- **Lexical Diversity ($L$-scale)**: The degree to which words across *every* position in a sentence are replaced with semantically equivalent alternatives.
- **Order Diversity ($O$-scale)**: The degree to which clause order, sentence boundaries, and syntactic dependency trees are rearranged.

When both Lexical Diversity and Order Diversity are set to high levels, detection rates across state-of-the-art classifiers collapse to near-human false positive baselines.

---

## 2. The Two Control Levers

```
                      HIGH ORDER DIVERSITY (O)
                                 ▲
                                 │   [DIPPER Resynthesis]
                                 │   • Invert clauses
                                 │   • Rebuild sentence graphs
                                 │   • Split & merge sentences
                                 │   • High burstiness (CV ≥ 0.55)
    [Cosmetic Scrambling]        │   • Defeats all detector families
    • Jumbled phrasing           │
    • Loss of voice/flow         │
  ◄──────────────────────────────┼──────────────────────────────►
  LOW LEXICAL                    │                    HIGH LEXICAL
  DIVERSITY (L)                  │                    DIVERSITY (L)
                                 │   [Surface Humanizer]
                                 │   • Swaps "delve" / em dashes
                                 │   • Preserves sentence tree
    [Raw AI Text]                │   • Still caught by GPTZero
    • Predictable n-grams        │
    • Low perplexity             │
                                 ▼
                      LOW ORDER DIVERSITY (O)
```

### Lever 1: Order Diversity ($O$) — Syntactic Reordering

To break the source model's token dependency graph, you must rearrange *how* the thought is structured:

1. **Clause Inversion**:
   - *AI Default*: Main clause $\rightarrow$ subordinate clause with an -ing participle.
     *"The company launched the new infrastructure, demonstrating its commitment to scalability."*
   - *High-$O$ Rewrite*: Lead with the context, purpose, or consequence.
     *"To handle growing load, the team rebuilt their server architecture from scratch."*
2. **Voice and Actor Realignment**:
   - Shift from abstract agentless passive to direct personal action, or invert the topical subject.
     *"Significant cost reductions were achieved through pipeline optimization."* $\rightarrow$
     *"The engineers cut AWS expenses by half by refactoring database queries."*
3. **Sentence Boundary Mutation**:
   - Break a single 25-word periodic sentence into a punchy 5-word assertion followed by an explanation.
   - Combine two fragmented sentences into a complex conditional sentence.
   - Disallow uniform sentence lengths across consecutive lines.

### Lever 2: Lexical Diversity ($L$) — Position-Wide Diction

Rather than replacing only "forbidden" AI words, regenerate diction across the entire proposition:

1. **Specific Domain Nouns**: Replace generic abstract nouns ("solutions", "landscape", "paradigm") with concrete operational terms ("Postgres replicas", "cold-start latency", "margin compression").
2. **Dynamic Verbs**: Eliminate copulas ("is", "serves as", "acts as") and vague verbs ("facilitates", "enhances") in favor of active, high-impact verbs ("slashed", "routed", "pinpointed").
3. **Idiosyncratic Phrasing**: Introduce natural human idioms, conversational friction, or authorial perspective that language models rarely generate at high temperature.

---

## 3. The 3-Pass Resynthesis Protocol

When tasked with humanizing text to be fully detector-resilient, follow this 3-pass protocol:

### Pass 1: Semantic Extraction (Deconstruct)

Read the AI text and extract its factual content into a bare list of **semantic predicates**, stripping away the original sentence structure entirely:
- What are the concrete claims?
- What are the numbers, entities, or citations?
- What is the logical relation between them (cause, contrast, sequence)?

*Do not carry over the phrasing or the syntax tree.*

### Pass 2: Syntactic Reconstruction (High $O$ & $L$)

Rebuild the text from the bare predicates using the target **Style Profile** and asymmetric human cadence:
1. **Vary sentence lengths radically**: Target a sentence-length Coefficient of Variation ($CV \ge 0.55$). Juxtapose 4-to-6 word sentences against 25-to-30 word compound sentences.
2. **Vary sentence starters**: Never start consecutive sentences with the same part of speech. Ban "What makes this...", "By doing so...", and throat-clearing openers.
3. **Invert the thematic progression**: Start with the consequence or the concrete fact rather than the abstract setup.

### Pass 3: Entropy & Stylometry Audit (Polish)

Run an immediate quantitative self-audit:
1. **Burstiness Check**: Is there cadence variety, or do sentences hover around 15–20 words? Break any run of similar-length sentences.
2. **Lexical Diversity Check**: Is the local moving-average type-token ratio ($\text{MATTR}_{50}$) high ($\ge 0.75$)? Ensure repetitive connecting words are purged.
3. **Pattern Audit**: Walk the 42 patterns (colon reveals, metadiscourse, em dashes, negative parallelism) to confirm zero cosmetic tells remain.
4. **Watermark Disruption Check**: Ensure token sequence boundaries have been shuffled so that preceding-token green-list hashes are broken.

---

## 4. Preserving Meaning and Complexity (The Karpathy Guardrail)

The primary failure mode of aggressive paraphrasers (e.g. DIPPER without human oversight) is **semantic drift**: numbers get rounded, nuances get lost, and technical claims get distorted.

Under `human`'s Karpathy discipline:
- **Zero Fact Loss**: Every number, entity name, citation, and technical relationship in the input must survive verbatim.
- **Zero Scope Shrinkage**: The output matches the input's technical depth, nuances, and intellectual weight.
- **Style Fidelity**: If a writing sample was provided, the regenerated prose must adopt the sample's syntax and cadence—not a generic paraphraser tone.
