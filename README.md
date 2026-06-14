# Propaganda-Detection-and-Text-Analytics

This project investigates the automatic detection and classification of propaganda techniques in textual content extracted from internet memes. The original dataset consisted of character-level span annotations containing overlapping spans, nested spans, and multiple propaganda techniques, creating significant challenges for traditional machine learning workflows.

A key contribution of this project was the development of a novel data engineering pipeline that automatically transformed complex annotation structures into machine-learning-ready datasets. This approach enabled the systematic handling of overlapping and nested propaganda spans while preserving annotation integrity throughout the modelling process.

To investigate effective solutions for propaganda span detection and technique classification, two modelling approaches were evaluated and compared.

The first approach treated the task as a traditional Named Entity Recognition (NER) problem, where token-level labels were used to identify propaganda spans and associated techniques directly from the text.

The second approach proposed a novel two-stage framework. The first model was trained to identify propaganda span boundaries by predicting start and end labels for each token, allowing propaganda spans to be extracted from the text. The extracted spans were then passed to a second model that performed propaganda technique classification using a multi-label classification framework. This decomposition separated span identification from technique classification, enabling independent optimisation of each task.

This repository demonstrates the complete NLP workflow, from exploratory analysis and data engineering through to model development and evaluation, with particular emphasis on solving the challenges associated with complex span-level propaganda annotations.
## Content
1. [Exploratory Data Analysis](./data-exploration/README.md)
2. [Data Engineering and Preprocessing](./data-engineering-and-preprocessing/README.md)
3. [Model Development and Evaluation](./modelling-and-evaluation/README.md)

## Build Configuration

### Data Exploration and Data Engineering

The environment used for data exploration and preprocessing can be recreated using the provided `environment.yml` file:

```bash
conda env create -f environment.yml
conda activate propaganda-analysis
jupyter lab
```

### Model Development and Evaluation

Model training and evaluation were performed using Google Colab.

Run:

`./modelling-and-evaluation/modelling_and_evaluation.ipynb`

within a Google Colab environment.


## Repository Structure

```text
Propaganda-Detection-and-Text-Analytics/
│
├── data/                              # Raw datasets
├── data-exploration/                  # Exploratory data analysis
├── data-engineering-and-preprocessing/# Data transformation pipelines
├── modelling-and-evaluation/          # Model training and evaluation
├── README.md
└── environment.yml
```
## Project Highlights

- Developed a custom data engineering pipeline for handling overlapping and nested propaganda span annotations.
- Transformed character-level annotations into machine-learning-ready token-level datasets.
- Performed exploratory analysis of propaganda technique distributions and class imbalance.
- Implemented and compared a traditional NER-based approach against a novel two-stage modelling framework.
- Built a token-level span identification model for propaganda boundary detection.
- Developed a multi-label classification model for propaganda technique prediction.
- Evaluated model performance across highly imbalanced propaganda technique categories.

