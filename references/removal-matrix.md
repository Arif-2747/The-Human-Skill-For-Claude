# Watermark & Provenance Removal Matrix

A practical operational matrix mapping every category of AI provenance mark to its detection method, mitigation action, potential side effects, and verification criteria. Derived from [guillaumemeyer/watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover).

---

## 1. Provenance Removal Matrix

| Target Layer | Specific Signal | Detection Method | Removal / Mitigation Action | Potential Side Effects | Verifiability |
|---|---|---|---|---|---|
| **Layer A**<br>(Edit-based) | Zero-width spaces (`U+200B`), Word Joiners (`U+2060`), BOM (`U+FEFF`), Soft Hyphens | Codepoint regex audit (`[\u200B-\u200D\u2060\uFEFF\u00AD]`), character code inspection | Strip non-load-bearing zero-width characters; normalize to ASCII space | None on standard prose. Must preserve emoji glue & Persian/Indic orthography | **100% Verifiable** (codepoint audit returns clean) |
| **Layer A**<br>(Edit-based) | Bidi directional overrides (`U+202A`–`U+202E`, `U+2066`–`U+2069`) | Inspect text for directional format controls in LTR text blocks | Strip directional markers unless document is genuine bidirectional/RTL text | None on English/monolingual text; layout break if stripped from genuine Arabic/Hebrew | **100% Verifiable** (zero directional controls remaining) |
| **Layer A**<br>(Edit-based) | Tag characters (`U+E0001`–`U+E007F`), unassigned Default_Ignorables | Grep/inspect codepoint ranges `0xE0000`–`0xE007F`, `U+2065` | Strip tag codepoints (preserve subdivision flags `U+1F3F4`..`U+E007F`) | None | **100% Verifiable** (no tag characters present) |
| **Layer A**<br>(Edit-based) | Exotic space substitutions (`U+00A0`, `U+2000`–`U+200A`, `U+202F`, `U+3000`) | Scan for non-ASCII space codepoints | Normalize to ASCII space `U+0020` (preserve non-breaking spaces in French typography if requested) | May affect strict French typesetting if `--no-normalize-spaces` not applied | **100% Verifiable** (clean ASCII space mapping) |
| **Layer A**<br>(Edit-based) | Homoglyphs & confusable substitutions (Cyrillic `а`/`е`, fullwidth Latin) | Unicode decomposition (NFKC) or confusable character mapping | Map confusables to Latin ASCII equivalents (`--nfkc` / `--aggressive-homoglyphs`) | Accidental translation of genuine multilingual words if applied indiscriminately | **100% Verifiable** (all characters in target Latin alphabet) |
| **Layer B**<br>(Statistical) | Kirchenbauer green-list token distribution bias | Token-level green-list z-score evaluation under reference key | **Layer B Rewrite**: Invert clauses, vary sentence boundaries, disrupt n-gram transitions, redistribute vocabulary entropy | Slight drift in phrasing; meaning and technical facts preserved | **Partially verifiable** with specific detector key; unverifiable without private vendor key |
| **Layer B**<br>(Statistical) | Google SynthID-Text tournament sampling marks | Proprietary Google scoring function (retired from public API mid-2026) | **Layer B Structural Rewrite**: Reorganize sentence logic, vary rhythm and sentence lengths, introduce high-information domain diction | Requires thorough rewrite rather than surface synonym swapping | **Empirical**: Diluted below statistical significance via structural changes |
| **Layer B**<br>(Statistical) | Stylometric AI cadence & low burstiness (uniform sentence lengths, high cadence n-grams) | Zero-LLM stylometry estimator: sentence-length CV, MATTR lexical diversity, weighted cadence density | Break rhythmic monotony: mix 4-word punches with 25-word multi-clause sentences; eliminate throat-clearing transitions | Sharper, more direct reading experience | **Verifiable via stylometry**: sentence-length CV > 0.45, cadence density < 0.20 |
| **Layer C**<br>(Container) | C2PA Content Credentials manifests in JPEG, PNG, WebP, AVIF, HEIC, MP4, WAV | Binary inspection for `jumb` boxes, `caBX` chunks, RIFF `C2PA` chunks | Strip C2PA box/chunk from binary stream (or call external HTTP service `POST /clean`) | Erases cryptographically signed provenance history | **100% Verifiable** (re-inspection confirms no `jumb`/C2PA chunks) |
| **Layer C**<br>(Container) | PDF `/Creator`, `/Producer`, `/ModDate`, persistent object streams | PDF metadata inspection (exiftool, qpdf) | Linearize and rebuild PDF object graph without metadata streams | Loses PDF generation history | **100% Verifiable** (metadata inspection returns empty) |
| **Layer C**<br>(Container) | OOXML doc properties (DOCX, PPTX, XLSX `docProps/core.xml`) | Unzip archive, inspect `core.xml` and `app.xml` | Scrub creator/editor metadata, repackage ZIP | Loses revision history and author initials | **100% Verifiable** (XML properties sanitized) |
| **Layer C**<br>(Container) | Markdown YAML frontmatter & HTML meta tags | Text scan for `model:`, `ai_generated:`, `<meta name="generator">` | Delete generator tags, purge AI-identifying YAML keys, strip HTML comments | Clean publication frontmatter | **100% Verifiable** (clean header inspection) |

---

## 2. Decision Tree for Processing Text & Files

```
[Input Received]
       │
       ├── Is it a binary file or container (PDF, DOCX, PNG, JPEG)?
       │      └── Route to Container/C2PA Strip (Layer C)
       │          (Strip metadata headers, JUMBF boxes, docProps)
       │
       └── Is it text or Markdown prose?
              │
              ├── Step 1: Layer A Pre-Flight Check (Deterministic)
              │     ├── Scan for invisible Unicode (ZWSP, WJ, BOM, tag chars)
              │     ├── Scan for exotic spaces or confusable homoglyphs
              │     └── Strip carriers while strictly preserving emoji glue & script joiners
              │
              ├── Step 2: Form Check (General vs Narrative)
              │     ├── Narrative: Run Structure Sheet (10 decisions) & 30-feature audit
              │     └── General: Proceed to Step 3
              │
              ├── Step 3: Layer B Structural & Stylistic Rewrite
              │     ├── Apply 42 humanizer patterns (vocabulary, copula avoidance, em dashes)
              │     ├── Apply Karpathy discipline & stop-slop rules (rhythm, sentence variety)
              │     └── Disperse token-sampling entropy (clause inversion, varied sentence lengths)
              │
              └── Step 4: Layer A Post-Flight Hygiene Verification
                    └── Confirm no em dashes or invisible formatting introduced during drafting
```

---

## 3. Tool & Service Integration Modes

1. **Model-Native Mode (Default)**:
   - The language model directly identifies and eliminates Layer A Unicode artifacts, applies the 42 vocabulary/syntax patterns, performs the Layer B structural rewrite, and purges Markdown/HTML provenance tags.
   - Requires zero external dependencies.

2. **HTTP Service Mode (Optional External Infrastructure)**:
   - When the user runs the `watermarks-remover` HTTP service (via Docker or local Python server) and exposes `WATERMARKS_SERVICE_URL`:
     ```bash
     WM="${WATERMARKS_SERVICE_URL:-http://127.0.0.1:8765}"
     # Check health & capabilities
     curl -sf "$WM/health"
     curl -sf "$WM/capabilities"
     # Inspect file
     curl -s -X POST "$WM/inspect" -H "Content-Type: application/json" \
       -d "{\"file\": \"$(base64 < document.docx | tr -d '\n')\", \"name\": \"document.docx\"}"
     # Clean file
     curl -s -X POST "$WM/clean" -H "Content-Type: application/json" \
       -d "{\"file\": \"$(base64 < document.docx | tr -d '\n')\", \"name\": \"document.docx\"}"
     ```
   - Automatically handles deep container stripping across PDFs, DOCX, images, and audio/video files.
