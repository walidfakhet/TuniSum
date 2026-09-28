# TuniSum: A Multi-Domain Dataset for Tunisian Arabic News Summarization

**TuniSum** is a multi-domain dataset and benchmark for **Tunisian news summarization in Modern Standard Arabic (MSA)**. It was created to address the lack of publicly available resources dedicated to Tunisian news summarization and to support research on low-resource Arabic NLP.

> **Important:** Although the dataset focuses on Tunisian news, the articles are written in **Modern Standard Arabic (MSA)** rather than Tunisian spoken dialect. The dataset combines MSA with Tunisian-specific news topics, entities, institutions, and contextual information.

---

## Dataset Overview

TuniSum was constructed from **22,650 raw news articles** collected from **40 Tunisian news sources** through RSS feeds. After preprocessing, filtering, and deduplication, the dataset provides two complementary summarization tasks:

| Task                                   | Input → Output          |      Pairs | Avg. Input | Avg. Output | Compression |
| -------------------------------------- | ----------------------- | ---------: | ---------: | ----------: | ----------: |
| **Task 1 — Title Generation**          | Description → Title     | **13,963** |  132 words |    10 words |        7.6% |
| **Task 2 — Abstractive Summarization** | Full Text → Description |  **8,030** |  604 words |    40 words |       17.3% |

The dataset covers **seven thematic domains**:

* Politics
* Sport
* Economy
* Culture
* Religion
* Technology
* Other

TuniSum is released under the **CC BY-NC 4.0** license for non-commercial academic research.

---

## Motivation

Arabic NLP has made substantial progress through pretrained models such as AraBERT, AraT5, ARBERT, MARBERT, and AraBART. However, publicly available summarization resources remain concentrated on Modern Standard Arabic or other regional varieties.

Tunisian news content remains underrepresented despite the large number of online Tunisian news outlets.

TuniSum was created to provide a dedicated benchmark for:

* Tunisian news summarization
* Low-resource Arabic NLP
* Multi-domain Arabic text generation
* Short-form title generation
* Abstractive news summarization
* Evaluation of Arabic pretrained sequence-to-sequence models

---

## Dataset Construction

The collection pipeline starts with **22,650 raw articles** obtained from 40 Tunisian news sources through RSS feeds.

A custom **Java-based RSS extractor** was used with incremental updates to reduce duplicate collection.

```text
RSS Feeds
    ↓
40 Tunisian News Sources
    ↓
Java RSS Parser
    ↓
22,650 Raw Articles
    ↓
Deduplication & Filtering
    ↓
Domain Classification
    ↓
TuniSum
 ┌───────────────┴───────────────┐
 ↓                               ↓
Task 1                          Task 2
Description → Title             Full Text → Description
13,963 pairs                    8,030 pairs
```

---

## Domain Classification

Articles are assigned to seven thematic domains using a keyword-based classifier containing more than **450 Arabic and French keywords**.

| Domain     |     Task 1 |    Task 2 |
| ---------- | ---------: | --------: |
| Politics   |      5,034 |     3,218 |
| Sport      |      3,244 |     1,434 |
| Economy    |      2,644 |     1,397 |
| Culture    |      1,088 |       616 |
| Religion   |        955 |       624 |
| Technology |        630 |       208 |
| Other      |        368 |       533 |
| **Total**  | **13,963** | **8,030** |

---

## Tasks

### Task 1 — Title Generation

The objective is to generate a concise news title from the article description.

```text
Input:  News description
Output: News title
```

**Statistics:**

* Dataset size: **13,963 pairs**
* Average source length: **132 words**
* Average target length: **10 words**
* Compression ratio: **7.6%**

### Task 2 — Abstractive Summarization

The objective is to generate a short description from the complete news article.

```text
Input:  Full news article
Output: News description
```

**Statistics:**

* Dataset size: **8,030 pairs**
* Average source length: **604 words**
* Average target length: **40 words**
* Compression ratio: **17.3%**

Task 2 is a subset of Task 1 for which the complete article text was available and sufficiently different from the RSS description.

---

## Dataset Splits

### Task 1

| Split      | Number of pairs |
| ---------- | --------------: |
| Train      |          11,170 |
| Validation |           1,396 |
| Test       |           1,397 |
| **Total**  |      **13,963** |

### Task 2

| Split      | Number of pairs |
| ---------- | --------------: |
| Train      |           6,424 |
| Validation |             803 |
| Test       |             803 |
| **Total**  |       **8,030** |

---

## Preprocessing

### Task 1

The original 22,650 articles were processed using:

1. Removal of null or empty entries
2. URL-based deduplication
3. Removal of cases where title equals description
4. Length filtering
5. Arabic character ratio filtering

This produced **13,963 description-title pairs**.

### Task 2

Task 2 applies the Task 1 filtering steps and additionally requires a valid full article text.

Additional filtering includes:

1. Removal of articles without full text
2. URL deduplication
3. Removal of cases where full text equals description
4. Removal of descriptions contained entirely within the full text
5. Length filtering
6. Compression-ratio filtering
7. Arabic character ratio filtering

This resulted in **8,030 full-text/description pairs**.

### Arabic Normalization

The preprocessing pipeline includes:

* Diacritics removal
* Alef normalization
* Alef Maqsura normalization
* Tatweel removal
* URL removal
* HTML artifact removal

Some residual RSS artifacts may remain in a small number of articles, such as navigation elements or source-attribution text.

---

## Dataset Statistics

| Statistic              | Task 1 | Task 2 |
| ---------------------- | -----: | -----: |
| Total pairs            | 13,963 |  8,030 |
| Train                  | 11,170 |  6,424 |
| Validation             |  1,396 |    803 |
| Test                   |  1,397 |    803 |
| Avg. source characters |    798 |  3,871 |
| Avg. source words      |    132 |    604 |
| Avg. target characters |     62 |    246 |
| Avg. target words      |     10 |     40 |
| Compression ratio      |   7.6% |  17.3% |
| Arabic character ratio |  83.7% |  81.2% |

---

## Benchmark Models

The paper benchmarks three pretrained sequence-to-sequence models.

### AraT5

**AraT5-base-title-generation**

* 220M parameters
* 512-token context
* Pretrained for Arabic text generation and title generation

### AraT5v2

* 220M parameters
* 1,024-token context
* Extended context compared with AraT5
* Particularly relevant to Task 2 because the average source contains 604 words

### mT5

* 300M parameters
* Multilingual T5 model
* Pretrained on 101 languages
* Used as a multilingual baseline

### AraBART

AraBART was also investigated but was not included in the main comparison because its long-form summarization objective is poorly aligned with the short title-generation objective of Task 1.

---

## Benchmark Results

### Task 1 — Description → Title

Results on the **1,397-example test set**:

| Model   |   ROUGE-1 |  ROUGE-2 |   ROUGE-L | BERTScore-F1 |
| ------- | --------: | -------: | --------: | -----------: |
| Lead-10 |      6.15 |     1.01 |      6.15 |        50.27 |
| mT5     |     13.87 |     3.16 |     13.77 |        83.72 |
| AraT5   |     16.11 | **3.51** |     16.00 |        87.07 |
| AraT5v2 | **16.37** |     3.32 | **16.37** |    **87.19** |

### Task 2 — Full Text → Description

Results on the **803-example test set**:

| Model   |   ROUGE-1 |   ROUGE-2 |   ROUGE-L | BERTScore-F1 |
| ------- | --------: | --------: | --------: | -----------: |
| Lead-10 |     30.66 |     19.77 |     30.25 |        60.35 |
| mT5     |     38.52 |     23.82 |     38.36 |        88.29 |
| AraT5   |     49.32 |     34.07 |     49.32 |    **93.77** |
| AraT5v2 | **50.23** | **34.68** | **50.19** |        93.67 |

---

## Evaluation Metrics

TuniSum experiments report:

* **ROUGE-1**
* **ROUGE-2**
* **ROUGE-L**
* **BERTScore-F1**

ROUGE measures lexical overlap between generated and reference summaries, while BERTScore evaluates semantic similarity using contextual embeddings.

Both metrics are reported because lexical variation can be particularly important in Arabic text generation.

---

## Main Findings

The benchmark experiments reported in the paper highlight several observations:

### Arabic-specific pretraining

AraT5 and AraT5v2 consistently outperform the multilingual mT5 baseline across the two tasks.

### Context length

AraT5v2 benefits from its **1,024-token context window**, particularly for Task 2 where the average source contains approximately 604 words.

### ROUGE vs. semantic similarity

On Task 2, AraT5v2 obtains a higher ROUGE-1 score than AraT5, while AraT5 obtains a slightly higher BERTScore-F1.

This illustrates the importance of reporting both lexical and semantic evaluation metrics.

### Extractive behavior

Qualitative analysis indicates that mT5 tends to copy portions of the beginning of the source article for Task 2, whereas AraT5 and AraT5v2 produce more abstractive descriptions.

### Domain imbalance

Politics represents the largest domain in the dataset, while Technology is substantially smaller. This imbalance should be considered when interpreting domain-specific results.

---

## Intended Use

TuniSum is intended primarily for **academic and non-commercial research** in areas such as:

* Arabic NLP
* Tunisian news processing
* Text summarization
* Abstractive summarization
* Title generation
* Low-resource NLP
* Sequence-to-sequence learning
* Multilingual and Arabic pretrained models
* Domain adaptation

Researchers can use the dataset to develop and compare new summarization architectures against the baseline results reported in the accompanying paper.

---

## Limitations

Several limitations should be considered when using TuniSum:

* **Domain imbalance:** Technology is underrepresented compared with Politics.
* **Keyword-based domain classification:** Some domain labels may contain noise.
* **Semi-extractive tendency:** RSS descriptions can correspond to lead sentences of articles.
* **Residual RSS artifacts:** Some articles may retain navigation or source-attribution artifacts.
* **Model coverage:** The benchmark focuses on AraT5, AraT5v2, and mT5.
* **Automatic evaluation:** No human evaluation is currently included; human evaluation is planned as future work.
* **AraBART comparison:** AraBART was excluded from the main comparison because its long-form generation objective does not match the short title-generation task.

---

## License

TuniSum is released under the:

**Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)**

The dataset is intended for non-commercial academic research.


---

## Acknowledgements

We thank the Tunisian news sources whose publicly available RSS feeds made the construction of TuniSum possible.

---

## Contact

**Walid Fakhet**

Institut Supérieur des Sciences Appliquées et de Technologie de Gafsa
Gafsa, Tunisia

ORCID: [0000-0002-4682-6938](https://orcid.org/0000-0002-4682-6938)

---

## Related Resources

TuniSum is designed to complement existing Arabic summarization resources such as:

* XL-Sum
* Essex Arabic Summaries Corpus (EASC)
* WikiLingua Arabic
* SANAD

Unlike these resources, TuniSum focuses specifically on **Tunisian news content** and provides two complementary generation tasks: title generation and full-text abstractive summarization.
