# Data Engineering & Preprocessing

The original dataset consisted of internet memes annotated with propaganda techniques. Annotations were provided as **character-level spans**, where each propaganda fragment was represented by its start and end character positions within the raw text.

Before modelling could take place, the dataset required extensive preprocessing and transformation to convert the annotations into formats suitable for transformer-based NLP models.

---

## Character-Level to Word-Level Span Conversion

The original annotations were defined using character positions. However, transformer models operate on tokens rather than characters. To bridge this gap, a custom **character-to-word mapping pipeline** was developed.

A numerical masking strategy was implemented to assign each character position to its corresponding word index while preserving spaces and line breaks. This enabled character-level annotations to be accurately converted into word-level spans without losing alignment information.

### Conversion Pipeline

```text
Raw Text
    ↓
Character-Level Spans
    ↓
Character-to-Word Mapping
    ↓
Word-Level Spans
    ↓
Tokenisation
    ↓
Model-Ready Data
```

The conversion pipeline consisted of:

* Character-to-word mapping generation
* Character-level span conversion
* Tokenisation of meme text
* Span alignment validation

This process ensured that all annotations remained correctly aligned after tokenisation.

---

## Handling Noisy Multi-Line Meme Text

The dataset contained highly inconsistent text formatting, including:

* Multiple consecutive spaces
* Multi-line text
* Irregular punctuation
* Varying text layouts

Custom preprocessing functions were developed to handle these cases while preserving annotation boundaries. Special handling was implemented for spaces and newline characters to prevent annotation misalignment during span conversion.

---

# Approach 1: BIO Tagging

The first modelling approach reformulated propaganda span identification as a token classification task using a BIO-style labelling scheme.

Each token was assigned one of three labels:

| Label | Description                    |
| ----- | ------------------------------ |
| 0     | Outside a propaganda span      |
| 1     | Beginning of a propaganda span |
| 2     | Inside a propaganda span       |

### Example

```text
THERE ARE ONLY TWO GENDERS
```

```text
0 0 1 2 2
```

This representation allowed propaganda spans to be converted into token-level training labels suitable for sequence-labelling models.

### Limitation

While effective for standard span identification, BIO tagging assumes that each token belongs to only one span.

As a result, overlapping propaganda annotations could not be represented without overwriting existing labels and losing information.

---

# Approach 2: Overlap-Aware Span Detection (Novel Approach)

To address the limitations of BIO tagging, a second approach was developed that reformulated span identification as a **boundary detection problem**.

Rather than assigning a single label to each token, two independent label sequences were generated:

* Span Start Labels
* Span End Labels

This allowed overlapping propaganda spans to be represented without forcing tokens into a single annotation sequence.

---

## Boundary Label Engineering

For each document, binary start and end vectors were generated:

* Start labels indicate where a propaganda span begins.
* End labels indicate where a propaganda span ends.

### Example

| Token | THE | EVIL | MEDIA | LIES |
| ----- | --- | ---- | ----- | ---- |
| Start | 0   | 1    | 1     | 0    |
| End   | 0   | 0    | 1     | 1    |

This representation preserves overlapping annotations and avoids the information loss associated with BIO tagging.

---

## Tokenizer Alignment

Transformer tokenizers often split words into multiple subword tokens.

### Example

```text
unbelievable
```

↓

```text
un ##bel ##ievable
```

To ensure label consistency, a custom alignment pipeline was implemented to synchronise span boundary labels with tokenized model inputs.

Special tokens introduced by the tokenizer were ignored during training, while span labels were propagated across subword tokens when required.

---

## Candidate Span Extraction and Classification

After span boundaries were identified, candidate spans could be reconstructed from predicted start and end positions.

These extracted spans formed the input to a secondary classification stage responsible for determining:

* Whether the extracted span represented propaganda.
* Which propaganda technique was present.

### Two-Stage Pipeline

```text
Stage 1
Span Boundary Detection
        ↓
Stage 2
Propaganda Technique Classification
```

---

## Key Contributions

* Developed a custom character-to-word span alignment system.
* Engineered preprocessing pipelines for noisy multi-line meme text.
* Converted character-level annotations into token-level representations.
* Designed BIO-based span identification labels.
* Developed an overlap-aware boundary detection approach for handling overlapping annotations.
* Implemented tokenizer-label alignment for transformer models.
* Constructed a two-stage span detection and classification pipeline.

---

The preprocessing and data engineering pipeline formed the foundation of the modelling process and enabled both traditional BIO tagging and the proposed overlap-aware span detection approach.
