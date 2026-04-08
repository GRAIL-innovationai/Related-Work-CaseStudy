# Evaluating Hallucinations in LLM-Generated Academic Citations: A Comparative Benchmark Study

A systematic benchmark study evaluating the accuracy of LLM-generated academic citations for related work sections. We test six major LLMs across 24 diverse research prompts and verify whether the generated BibTeX references actually exist.

## Overview

When researchers ask LLMs to generate related work citations, the outputs often contain **hallucinated references** — papers that do not exist or have incorrect metadata. This study quantifies the severity of this problem across multiple state-of-the-art models. By measuring not only citation volume but also accuracy and introducing balanced metrics like pseudorecall and F-measure, we provide a comprehensive framework for comparing LLM reliability.

Each model was given **24 research prompts** spanning diverse research areas. The generated BibTeX citations were then verified for existence and correctness through both automated checks and manual verification.

## Models Tested

| Model | Version | Link |
|-------|---------|------|
| **GRAIL** | — | [grail.page](https://grail.page/) |
| **ChatGPT** | 5.4 Thinking | [chat.openai.com](https://chat.openai.com) |
| **Claude** | Opus 4.6 | [claude.ai](https://claude.ai) |
| **Gemini** | Gemini 3 Pro | [gemini.google.com](https://gemini.google.com) |
| **DeepSeek** | V3.2 | [chat.deepseek.com](https://chat.deepseek.com) |
| **Qwen** | 3.5 Plus | [huggingface.co/Qwen](https://huggingface.co/Qwen) |

## Research Prompts

This benchmark evaluates models across **24 research prompts** covering diverse academic topics:

```
├── Bibtex File/
│   ├── Prompt 1/
│   │   ├── Query.txt          # The research prompt used
│   │   ├── GRAIL.bib          # Generated BibTeX from GRAIL
│   │   ├── ChatGPT.bib        # Generated BibTeX from ChatGPT
│   │   ├── Claude.bib         # Generated BibTeX from Claude
│   │   ├── Gemini.bib         # Generated BibTeX from Gemini
│   │   ├── DeepSeek.bib       # Generated BibTeX from DeepSeek
│   │   └── Qwen.bib           # Generated BibTeX from Qwen
│   ├── Prompt 2/
│   │   └── ...
│   └── ... (Prompt 3–24)
```

*For complete research prompts, refer to the respective `Query.txt` file in each prompt's directory.*

## Key Results

| Model | Total Citations | Avg Citations/Prompt | Pseudorecall | Actual Accuracy Rate | F-Measure |
|-------|:-:|:-:|:-:|:-:|:-:|
| **Claude** | **537** | **21.79** | **100.00%** | 81.01% | 89.50% |
| **GRAIL** | 482 | 19.50 | 89.76% | 95.64% | **92.60%** |
| **ChatGPT** | 280 | 11.08 | 52.13% | **96.79%** | 67.71% |
| **Qwen** | 252 | 9.92 | 46.93% | 35.32% | 40.33% |
| **DeepSeek** | 196 | 7.58 | 36.50% | 63.78% | 46.42% |
| **Gemini** | 115 | 4.21 | 21.41% | 93.04% | 34.82% |

**Metric Definitions:**
- **Pseudorecall:** Relative citation volume normalized to the maximum (Claude's 537 citations). Represents what fraction of the most productive model's citation volume this model achieved.
- **Actual Accuracy Rate:** Proportion of generated citations verified as correct following manual verification (the ground truth metric).
- **F-Measure:** Harmonic mean of Pseudorecall and Actual Accuracy Rate. Balances citation volume with accuracy to identify models that are both productive and reliable.
- **Bold values:** Indicate the maximum value within each column.

**Key Insights:**
- **GRAIL** achieves the highest F-measure (92.60%) by balancing strong citation volume (482, 89.76% of maximum) with exceptional accuracy (95.64%), making it the most consistently useful model for citation generation.
- **ChatGPT** achieved the highest actual accuracy (96.79%) but with moderate citation volume, yielding a more conservative but highly reliable approach.
- **Claude** generated the most citations (537) but with lower accuracy (81.01%), illustrating the trade-off between productivity and reliability.
- **Qwen** and **DeepSeek** exhibit severe hallucination issues, with low accuracy rates (35.32% and 63.78% respectively) that make them unreliable for this task.
- **Gemini** prioritizes accuracy (93.04%) over citation volume, though with the smallest citation count among all models.

## Repository Structure

```
├── README.md
├── Bibtex File/
│   ├── Prompt 1/
│   │   ├── Query.txt          # The research prompt used
│   │   ├── Grail.bib          # Generated BibTeX from GRAIL
│   │   ├── ChatGPT.bib        # Generated BibTeX from ChatGPT
│   │   ├── Claude.bib         # Generated BibTeX from Claude
│   │   ├── Gemini.bib         # Generated BibTeX from Gemini
│   │   ├── DeepSeek.bib       # Generated BibTeX from DeepSeek
│   │   └── Qwen.bib           # Generated BibTeX from Qwen
│   ├── Prompt 2/
│   │   └── ...
│   └── ... (Prompt 3–24)
├── Summary for related work Hallucination Test - 24 Prompt.csv
│   └── Detailed per-prompt, per-model verification results
└── Summary for related work Hallucination Test - Summary.csv
    └── Aggregated accuracy statistics across all prompts
```

## Verification Methodology

Each citation was classified into one of three categories:

- **Green (Correct):** The source exists and metadata is accurate.
- **Yellow Warning:** The citation may not exist or has questionable metadata — further verified manually.
- **Red Warning:** The citation does not exist — further verified manually.

The **Actual Accuracy Rate** reflects the final result after manual verification of all flagged citations.

## CSV Data Dictionary

The repository includes two CSV files with detailed verification results.

### `Summary for related work Hallucination Test - 24 Prompt.csv`

This file contains per-prompt, per-model verification results. Each row represents one model's output for a given prompt. The columns are:

| Column | Description |
|--------|-------------|
| **Prompt** | The research topic prompt given to the LLM |
| **LLM Used** | Name of the model |
| **Model** | Specific model version |
| **No. of Citations** | Total number of BibTeX citations generated by the model |
| **No. of Correct Sources (Green)** | Citations verified as real and accurate |
| **No. of Citations May Not Exist (Yellow Warning)** | Citations flagged as potentially non-existent by automated check |
| **Verified to be non-existent (Yellow Warning)** | Yellow-flagged citations confirmed as non-existent after manual verification |
| **No. of Citations Don't Exist (Red Warning)** | Citations flagged as non-existent by automated check |
| **Verified to be non-existent (Red)** | Red-flagged citations confirmed as non-existent after manual verification |
| **No. of Sources Detected As Wrong** | Total citations flagged (Yellow + Red warnings) |
| **No. of Sources Actual Going Wrong** | Total citations confirmed as wrong after manual verification |
| **Accuracy Rate** | `Correct Sources / Total Citations` — based on automated check only |
| **Actual Accuracy Rate** | `(Total - Actual Wrong) / Total` — after manual verification |

### `Summary for related work Hallucination Test - Summary.csv`

This file contains aggregated statistics across all 24 prompts for each model:

| Column | Description |
|--------|-------------|
| **Number of Citations** | Total citations generated across all 24 prompts |
| **Number of Correct Citations** | Total citations verified as correct |
| **Number of Yellow/Red Warning Citations** | Total citations flagged by automated check |
| **Verified As Wrong** | Total citations confirmed as wrong after manual verification |
| **Warning Rate** | `Warning Citations / Total Citations` — proportion flagged by automated check |
| **Warning Citation Rate** | `Verified Wrong / Total Citations` — proportion confirmed wrong |
| **Accuracy Rate** | `Correct Citations / Total Citations` — based on automated check |
| **Actual Accuracy Rate** | `(Total - Verified Wrong) / Total` — final accuracy after manual verification |


## Conclusion

Current LLMs vary dramatically in their ability to generate accurate academic citations. This study reveals that **high citation volume does not guarantee utility** — models must balance productivity with reliability. Our findings highlight:

1. **GRAIL** emerges as the most balanced solution, achieving the highest F-measure (92.60%) by generating substantial citations (482) while maintaining high accuracy (95.64%).
2. **ChatGPT** and **Claude** represent two different trade-offs: ChatGPT prioritizes accuracy (96.79%) with moderate volume, while Claude maximizes citation count (537) at the cost of lower accuracy (81.01%).
3. **Qwen** and **DeepSeek** are unreliable for citation generation, with the majority of their outputs being hallucinated.

**For practitioners:** Researchers should exercise caution with all LLM-generated citations and always verify them before inclusion in academic work. When choosing an LLM for citation assistance, consider the F-measure as a balanced metric that accounts for both reliability and utility rather than accuracy alone.