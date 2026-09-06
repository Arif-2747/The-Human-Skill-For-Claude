# Watermark & Provenance Mark Classes Reference

A comprehensive reference for multi-vendor AI provenance marks across text, files, and containers. This document details the technical layers that operate beneath visible prose patterns, derived from the research literature and [guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover).

---

## 1. Overview: The Multi-Layer AI Signal Hierarchy

AI outputs carry signals at multiple distinct architectural levels:

```
┌────────────────────────────────────────────────────────┐
│ Layer 4: Voice & Style (Style Profile, 12 dimensions)  │
├────────────────────────────────────────────────────────┤
│ Layer 3: Narrative Structure (StoryScope, 30 features) │
├────────────────────────────────────────────────────────┤
│ Layer 2: Syntax & Rhythm (Stop-slop, cadence, em dash) │
├────────────────────────────────────────────────────────┤
│ Layer 1: Vocabulary & Phrasing (42 humanizer patterns) │
├────────────────────────────────────────────────────────┤
│ Layer 0: Machine Provenance (watermarks & metadata)    │
│   ├─ Layer A: Invisible Unicode & steganography        │
│   ├─ Layer B: Statistical token-sampling watermarks    │
│   └─ Layer C: Container & C2PA provenance metadata     │
└────────────────────────────────────────────────────────┘
```

Standard editing tools only address Layer 1 and parts of Layer 2. Removing AI tells completely requires understanding Layer 0: the low-level provenance marks deliberately stamped into text, tokens, and file headers.

---

## 2. Layer A: Edit-Based & Invisible Unicode Marks

Edit-based watermarks inject imperceptible or near-invisible characters, directional formatting controls, unassigned codepoints, or typographic confusables directly into text streams. These survive copy-pasting between plain text fields.

### Codepoint Taxonomy

| Category | Typical Codepoints | Purpose / Carrier Mechanism |
|---|---|---|
| **`zwj_family`** | `U+200B` (Zero-Width Space / ZWSP)<br>`U+200C` (Zero-Width Non-Joiner / ZWNJ)<br>`U+200D` (Zero-Width Joiner / ZWJ)<br>`U+2060` (Word Joiner / WJ)<br>`U+FEFF` (Zero-Width No-Break Space / BOM)<br>`U+00AD` (Soft Hyphen / SHY) | Injected between syllables, word boundaries, or spaces to encode binary payloads (e.g. model ID, timestamp, user hash). |
| **`bidi`** | `U+202A`–`U+202E` (LRE, RLE, PDF, LRO, RLO)<br>`U+2066`–`U+2069` (LRI, RLI, FSI, PDI) | Directional embedding, override, and isolation markers. Placed invisibly in LTR text to alter parsing without affecting rendering. |
| **`tag_chars`** | `U+E0001` (Language Tag)<br>`U+E0020`–`U+E007E` (Tag ASCII range)<br>`U+E007F` (Cancel Tag) | Special Unicode tags designed for language tagging; abused as an invisible 7-bit ASCII transmission channel in text. |
| **`variation_selector`** | `U+FE00`–`U+FE0F` (VS1–VS16)<br>`U+E0100`–`U+E01EF` (VS17–VS256) | Select alternate glyph presentations. Injected after plain ASCII letters where they have no visible glyph effect. |
| **`reserved_ignorable`** | `U+2065`, `U+FFF0`–`U+FFF8`<br>`U+E0000`, `U+E0080`–`U+E00FF`<br>`U+E01F0`–`U+E0FFF` | Unassigned characters designated by the Unicode standard as Default_Ignorable_Code_Point (`DI`). |
| **`private_use`** | `U+E000`–`U+F8FF` (BMP PUA)<br>`U+F0000`–`U+FFFFD` (Plane 15 PUA)<br>`U+100000`–`U+10FFFD` (Plane 16 PUA) | Private use areas with no standardized glyph assignment, used for custom in-band signaling. |
| **`noncharacter`** | `U+FDD0`–`U+FDEF`<br>`U+FFFE`, `U+FFFF` (and end of planes 1–16) | Guaranteed never to be assigned to any character; used for internal marker abuse. |
| **`space`** | `U+00A0` (NBSP)<br>`U+2000`–`U+200A` (En/Em/Thin/Hair space)<br>`U+202F` (Narrow NBSP)<br>`U+205F` (Medium Math Space)<br>`U+3000` (Ideographic Space) | Non-standard spaces substituted for standard ASCII space (`U+0020`). Sequences of varying space widths encode data. |
| **`confusable`** | Cyrillic (`а`, `е`, `о`, `р`, `с`, `х`), Fullwidth Latin (`U+FF01`–`U+FF5E`), Math Alphanumeric | Homoglyphs visually identical to Latin characters in standard fonts, replacing Latin letters to watermark tokens. |

### Load-Bearing Preservations (Critical Rules)

A blind strip of all non-ASCII or invisible codepoints corrupts legitimate text. Cleaners and models must respect load-bearing contexts:

1. **Emoji Glue**: Do **not** strip `ZWJ` (`U+200D`) or Variation Selectors (`U+FE0F`) when they connect emoji base characters into composite emojis (e.g. `👨‍👩‍👧‍👦` family, `🏳️‍🌈` rainbow flag).
2. **Complex Script Orthography**:
   - **Persian / Farsi**: `ZWNJ` (`U+200C`) is linguistically required for prefixes (e.g. `می‌خواهم`) and plural suffixes (e.g. `خانه‌ها`). Stripping it creates spelling errors.
   - **Devanagari / Indic scripts**: `ZWJ` and `ZWNJ` dictate half-forms and conjunct consonants.
   - **Arabic**: Orthographic format marks (`U+0600`–`U+0605`) are required for correct Quranic and classical layout.
3. **Regional Flags**: Tag characters between `U+1F3F4` and `U+E007F` form standard subdivision flags (e.g. Scotland, Wales).
4. **Typographic Spaces**: French and certain continental European typographical conventions require non-breaking spaces before two-part punctuation (`;`, `:`, `!`, `?`) and inside guillemets (`« ... »`). Preserve these unless explicitly asked to normalize all spaces to ASCII `U+0020`.

---

## 3. Layer B: Generative & Statistical Token Sampling

Generative watermarks do not alter characters or inject metadata. Instead, they alter the **probability distribution** over the vocabulary during next-token generation (Nature, 2024; Kirchenbauer et al., 2023).

### How It Works

1. **Green-List / Red-List Partitioning (Kirchenbauer et al.)**:
   - At token step $i$, the generator takes a pseudo-random hash of the preceding token(s) (e.g. $hash(t_{i-1}, K)$ using secret key $K$).
   - The entire model vocabulary $V$ is partitioned into a "green list" $G$ (typically 50% of tokens) and a "red list" $R$.
   - A constant bias $\delta > 0$ is added to the logits of all tokens in $G$.
   - Detection: counts the fraction of green tokens in a passage. If the text has $N$ tokens and significantly more than $N/2$ belong to $G$, a statistical z-test confirms AI authorship with extreme confidence ($z > 4.0$).
2. **Tournament Sampling (Google SynthID-Text)**:
   - Evaluates candidate tokens across pairwise tournament brackets keyed on token contexts.
   - Preserves generation quality and text entropy better than basic green lists while leaving a persistent statistical signature.
3. **Keyed Gumbel / EXP (Aaronson)**:
   - Incorporates a secret pseudo-random seed into Gumbel-max sampling so the sequence of generated tokens correlates with the pseudo-random generator state.

### Why Surface Rewriting Fails Against Layer B

- **Synonym cycling fails**: Swapping a word for a synonym without altering sentence structure leaves the preceding token hashes and n-gram relationships largely intact.
- **Surface polishing fails**: Fixing grammar, removing adverbs, or swapping "delve" does not disrupt the mathematical token-frequency correlations.
- **What actually works**:
  - **Clause restructuring**: Invert dependent and independent clauses.
  - **Sentence splitting and merging**: Disrupt the exact sequence of token transitions.
  - **Cadence alteration**: Inject varied sentence lengths and break predictable n-gram pairings.
  - **Vocabulary diversity (MATTR)**: Introduce specific, high-information human nouns and verbs that break the model's preferred green-list paths.

---

## 4. Layer C: Document & Container Provenance Metadata

Even when the text body is completely sanitized, the enclosing file container frequently advertises AI origin or records cryptographically signed provenance.

### Major Container Signatures

| Format | Artifact / Provenance Carrier | Detection & Clean Path |
|---|---|---|
| **C2PA / Content Credentials** | JUMBF (`jumb`) boxes in JPEG, PNG (`caBX`), WebP, AVIF, HEIC, MP4/MOV; RIFF `C2PA` chunks in WAV; ID3v2 frames in MP3. | Cryptographic manifest declaring AI model, toolchain, and edit history. Must be stripped from container headers. |
| **PDF** | `/Creator`, `/Producer`, `/ModDate` info dictionary keys; XMP `xmp:CreatorTool`, `pdf:Producer`; metadata streams. | Structural rewrite required to remove persistent object streams. |
| **OOXML (DOCX, PPTX, XLSX)** | `docProps/core.xml` (`dc:creator`, `cp:lastModifiedBy`), `docProps/app.xml` (`Application`), `customXml/` injection. | Unpack ZIP container, scrub XML properties, repack archive. |
| **EPUB** | `content.opf` metadata tags (`dc:creator`, `meta name="generator"`), embedded XHTML headers. | Scrub OPF manifest, clean embedded media and HTML meta tags. |
| **HTML / Markdown** | `<meta name="generator" content="...">`, HTML comment signatures (`<!-- Generated by ... -->`), YAML frontmatter keys (`model:`, `ai_generated: true`, `prompt:`). | Delete metadata tags, strip generator headers, prune frontmatter keys. |
| **SVG** | `<metadata>` elements, RDF/XMP blocks, embedded base64 data URIs. | Strip `<metadata>` blocks; sanitize embedded raster data URIs. |

---

## 5. Vendor Landscape (Public & Class-Level Status)

- **Anthropic / Claude**:
  - Employs embedded statistical text watermarks at the model level (survives copy-paste).
  - Employs C2PA Content Credentials on image/document generation surfaces.
  - Mitigated via Layer A Unicode hygiene, Layer B structural rewrite, and Layer C container metadata removal.
- **Google / SynthID**:
  - SynthID-Text (Tournament sampling): documented in Nature (2024).
  - *API Status*: Google retired native SynthID text watermarking on current Gemini API endpoints (mid-2026), but SynthID-Image and enterprise deployments remain active.
- **OpenAI**:
  - Employs C2PA metadata manifests on DALL-E and image generation outputs.
  - Statistical sampling research active; container-level metadata present across enterprise exports.
- **Open-LLM / Self-Hosted**:
  - Kirchenbauer-style and keyed-Gumbel schemes implemented in engines such as MarkLLM, vLLM, and text-generation-webui.
