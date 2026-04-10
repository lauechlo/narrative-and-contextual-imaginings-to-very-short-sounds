# Narrative and Contextual Imagination to Very Short Sounds

## Analysis Code

Lau, C., Turnbull, C., Loui, P., & Margulis, E. H.

This repository contains the analysis code for "Narrative and Contextual Imagination to Very Short Sounds." All analyses were conducted in R (via Quarto) and Python (via Google Colab).

### Repository Structure

```
├── R/
│   ├── story_clean.qmd          # Story group analyses
│   └── context_clean.qmd        # Context group analyses
├── Python/
│   ├── Single_Sounds_Story_Group_Analysis.ipynb    # Story group text cleaning & word clouds
│   ├── Single_Sounds_Context_Group_Analysis.ipynb  # Context group text cleaning & word clouds
│   └── Single_Sounds_Between_Group.ipynb           # Cross-group TF-IDF cosine similarity & t-tests
├── data/
│   └── [de-identified response data]
├── stimuli/
│   └── [8 synthesized audio stimuli]
└── README.md
```

### Execution Order

The analysis pipeline should be run in the following order:

**Step 1: R preprocessing and statistical models**

Run `story_clean.qmd` and `context_clean.qmd` in either order. These perform:

- Data cleaning and headphone check filtering
- Exclusion criteria application (retained sample: 148 Story + 156 Context = 304 participants)
- Descriptive statistics (familiarity, enjoyment, NES ratings)
- Story group logistic GLMM: SRQ_yes ~ Duration + Timbre + Warble + AttackTime + (1|ID)
- Story group NES LMM: Average ~ Duration + Timbre + Warble + AttackTime + (1|ID)
- Context group ease-of-context LMM: Rating ~ Duration + Timbre + Warble + AttackTime + (1|ID)
- Between-group OLS regression: story proportion ~ mean context ease (audio-level)
- CSV exports for the Python NLP pipeline

**Step 2: Python text analysis (run in order)**

1. `Single_Sounds_Story_Group_Analysis.ipynb`: Text cleaning for the Story group (punctuation/case stripping, spell correction, lemmatization, stopword removal), TF-IDF word cloud generation by stimulus (Figure 6) and by audio feature.

2. `Single_Sounds_Context_Group_Analysis.ipynb`: Text cleaning for the Context group (same pipeline as Story), TF-IDF word cloud generation by stimulus and by audio feature.

3. `Single_Sounds_Between_Group.ipynb`: Cross-group TF-IDF cosine similarity analysis. Loads cleaned text from the Story and Context notebooks, projects into shared TF-IDF space, and computes pairwise cosine similarity. Runs Welch's t-tests comparing same- vs. different-feature similarity (timbre, duration, warble, attack time) within each group and between groups.

### Software and Dependencies

**R (v4.x)**
- lme4 (GLMMs and LMMs)
- lmerTest (p-values via Satterthwaite)
- MuMIn (R² for mixed models)
- broom.mixed (tidy model output)
- dplyr, tidyr, ggplot2, patchwork
- readxl

**Python (3.x, Google Colab)**
- scikit-learn (TfidfVectorizer, cosine_similarity)
- nltk (WordNetLemmatizer, stopwords, pos_tag)
- spacy (stopwords)
- gensim (stopwords)
- wordcloud
- pandas, numpy, scipy, matplotlib, seaborn

### AI Disclosure

The first author used Claude (Anthropic) and ChatGPT (OpenAI) for checking, debugging, and generating R and Python code in the statistical analysis pipeline. All AI-generated code outputs were independently verified against raw data by the first author. No AI tool was used to design the study, collect data, interpret results, or draw conclusions.

### Contact

Corresponding author: Elizabeth H. Margulis (ehm@princeton.edu)
