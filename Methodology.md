# Topic Modeling Methodology for Bioinformatics Literature

## 1. Data Collection & Preparation

- **Source**: PubMed biomedical literature (2013–2024)
- **Search Strategy**:
  - Keywords: `Molecular Docking`, `NGS Analysis`, `Virtual Screening` + 25 bioinformatics terms
  - Filters:
    - Last 10 years (2014–2024)
    - Abstracts available
    - Open access articles
- **Metadata Extraction**:
  - Fields: PMID, Title, Abstract, Year, Journal, Country
  - Tools: Python regex scripts
  - Output: Structured CSV dataset

---

## 2. Data Preprocessing Pipeline

### Text Cleaning
- Remove duplicates & null entries
- Eliminate special characters/numbers
- Normalize publication dates → Year only
- Handle encoding issues (`\n`, `\xag`)

### Text Standardization
- Lowercasing (`Gene → gene`)
- Custom stopword removal:
  - NLTK default list + domain-specific terms
  - Retained critical terms (e.g., `"in_silico"`)
- Lemmatization (WordNetLemmatizer):
  - `analyses → analysis`, `genes → gene`

### Feature Engineering
- **Tokenization**: Split text into words/phrases
- **N-gram Detection**:
  - Bigrams: `virtual_screening`, `gene_expression`
  - Trigrams: `next_generation_sequencing`
- **TF-IDF Transformation**:
  - Weight terms by importance
  - `max_features=10,000`, `min_df=5`

---

## 3. Topic Modeling Implementation

### Latent Dirichlet Allocation (LDA)

```python
lda_model = gensim.models.LdaModel(
    corpus=corpus,
    id2word=id2word,
    num_topics=14,          # Grid search optimized
    alpha=0.91,             # Symmetric document-topic
    eta=0.91,               # Symmetric topic-word
    random_state=42
)
```
Validation:

Coherence Score: 0.48 (c_v metric)

Perplexity: -9.472

Visualization: pyLDAvis inter-topic distance maps

Non-negative Matrix Factorization (NMF)
```python
nmf_model = gensim.models.Nmf(
    corpus=corpus,
    id2word=id2word,
    num_topics=10,          # Coherence-optimized
    random_state=100,
    passes=100
)
```
Matrix Factorization:

TF-IDF → W (term-topic) + H (topic-document)

Non-negativity constraints

Output: Heatmaps + top-term visualizations

## 4. Model Comparison & Interpretation
Metric	LDA	NMF
Topics	14	10
Coherence	0.47	0.48
Strengths	Probabilistic framework	Additive part-based
Key Themes	Functional analysis	Technical methods

Dominant Research Trends:
Gene expression in cancer

NGS genomic applications

In silico drug discovery

miRNA–mRNA interactions

## 5. Tools & Libraries
mermaid
Copy
Edit
graph LR
    A[Python] --> B[Pandas]
    A --> C[NLTK]
    A --> D[Gensim]
    A --> E[pyLDAvis]
    A --> F[Matplotlib]
    D --> G[LDA]
    D --> H[NMF]
Key Functions:
Pandas: Data cleaning and manipulation

NLTK: Tokenization, lemmatization, stopword removal

Gensim: LDA/NMF implementation

pyLDAvis: Interactive topic visualization

Matplotlib: Static visualizations (word clouds, heatmaps)

## 6. Limitations & Future Work
Challenges:
Short-text complexity (abstracts)

Term disambiguation (e.g., "NGS")

Improvements:
Incorporate BERTopic / LLMs

Dynamic topic modeling (temporal trends)

Clinical correlation analysis (ICD-10 mapping)
