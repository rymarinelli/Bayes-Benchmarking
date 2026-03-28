# Bayes-Benchmarking

A proof-of-concept demonstrating how Bayesian sequential testing can be used to 
benchmark LLMs more efficiently — reaching confident model rankings while evaluating 
a fraction of the usual dataset.

## Overview

Standard benchmarking uses a pass@K approach, running each prompt 5–10 times across 
thousands of questions. This is computationally expensive and energy intensive. This 
project applies a Beta-Bernoulli sequential testing framework to stop evaluation early 
once sufficient confidence has been reached, without sacrificing ranking accuracy.

Two variants of [NorMistral](https://huggingface.co/norallm) are compared — the 7B and 
11B models — evaluated on the [NorEval](https://github.com/ltgoslo/noreval) `nob_world` 
benchmark suite in a zero-shot setting. The Bayesian approach reached a confident 
decision after evaluating only **410 out of 31,800 problems** — a reduction of **98.7%**.

## Results

| Task | Stopped at | Total N | Savings | P(11B > 7B) |
|---|---|---|---|---|
| NorBeleBele | 4 | 4,500 | 99.9% | 0.976 |
| NorCommonsenseQA | 24 | 4,990 | 99.5% | 0.960 |
| NorOpenBookQA | 65 | 1,870 | 96.5% | 0.960 |
| NorTruthfulQA | 119 | 2,440 | 95.1% | 0.954 |
| NRK Quiz QA | 198 | 18,000 | 98.9% | 0.950 |
| **Total** | **410** | **31,800** | **98.7%** | |

## Notebook

The analysis is available as a Google Colab notebook:  
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/rymarinelli/Bayes-Benchmarking/blob/main/Bayesian_Benchmarking_Analysis.ipynb)

To reproduce the results, upload `7b_results.zip` and `noreval_results_11B.zip` when 
prompted and run all cells in order.

## Method

- **Model:** Beta-Bernoulli with a uniform Beta(1,1) prior, updated via conjugate updating
- **Stopping rule:** halt when P(11B > 7B) ≥ 0.95 or ≤ 0.05
- **Skip rule:** skip non-discriminating problems where both models are clearly too easy 
  or too hard (τ = 0.85)
- **Inference:** zero-shot, evaluated with 
  [lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness) and vLLM

## Submission

This work was submitted to 40th International Workshop on Statistical Modelling 
(IWSM), Oslo, Norway, 28 June – 3 July 2026.
