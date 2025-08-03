# BioNLP-Topic-Modeling

## LDA/NMF Approach for Bioinformatics Topic Modeling

### **1. Data Collection & Preparation**
Source: PubMed biomedical literature database (2013-2024).

Keywords: Bioinformatics-specific terms (e.g., "Molecular Docking," "NGS Analysis," "Virtual Screening").

Filters:

Time range: Last 3 years.

Document type: Abstracts available & freely accessible.

Metadata Extraction: PMID, Title, Abstract, Publication Year, Journal, Country via Python regex script.

### **2. Data Preprocessing Pipeline**
**Cleaning:**

Removed duplicates, null values, and special characters/numbers.

Normalized dates to publication year only.

**Text Standardization:**

Lowercasing ("Gene" → "gene").

Custom stopword removal (NLTK defaults + domain-specific adjustments).

**Advanced NLP Processing:**

**Tokenization:** Split text into words/phrases.

**Lemmatization:** Reduced words to root forms (e.g., "analyses" → "analysis").

**N-grams:** Identified bigrams/trigrams (e.g., "virtual_screening") using Gensim’s Phrases.

**Feature Engineering:**

TF-IDF Transformation: Weighted terms by importance (frequent in doc, rare in corpus).

### **3. Topic Modeling Implementation**
LDA (Latent Dirichlet Allocation)
Algorithm: Probabilistic model (documents = mixture of topics; topics = distribution of words).

**Hyperparameter Tuning:**

Grid search for optimal num_topics, alpha, beta.

Evaluated via coherence score (maximized at 0.48 for 14 topics).

**Optimal Setup:**

num_topics=14, alpha=0.91, beta=0.91.

**Validation:**

Perplexity: -9.472 (indicates strong generalization).

Visualization: Interactive pyLDAvis inter-topic distance maps.

NMF (Non-negative Matrix Factorization)
Algorithm: Linear-algebraic matrix decomposition (TF-IDF matrix → term-topic + topic-document matrices).

**Optimization:**

Coherence-driven topic count selection (num_topics=10).

**Output:**

Topic-word heatmaps + top-term visualizations.

Coherence Score: 0.48 (comparable to LDA).

### **4. Model Comparison & Topic Interpretation**
LDA Topics: Focused on functional themes (e.g., Topic 5: "functional gene analysis in cardiac disorders").

NMF Topics: Emphasized technical methods (e.g., Topic 2: "virtual_screening, molecular_docking").

**Key Insights:**

Dominant research trends: Gene Expression Analysis, NGS Applications, Drug Discovery via Molecular Docking.

Interdisciplinary links: Bioinformatics × AI, clinical diagnostics, genomic medicine.

### **5. Tools & Libraries**
Python Stack: Gensim (LDA/NMF), NLTK (text preprocessing), pandas (data cleaning), pyLDAvis (visualization).

Validation Metrics: Coherence score (c_v), perplexity, manual topic labeling.

### **6. Strengths & Limitations**
**Strengths:**

Captured evolving trends (e.g., rise of NGS and in silico drug screening).

Handled large-scale corpus (37M+ PubMed citations).

**Limitations:**

Short-text complexity (abstracts lack full-document context).

Dependency on preprocessing (e.g., lemmatization accuracy).
