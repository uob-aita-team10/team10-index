# EMATM0067 Team 10 — Task 5: Build a Neural Network to Describe Simple Pictures

University of Bristol | Spring 2026  
Introduction to AI and Text Analytics (EMATM0067)

This is the central index repository for Team 10's Task 5 submission. Individual 
experimental work is maintained in separate repositories linked below, each with 
its own commit history and results.

## Team Members and Repositories

| Member | Repository | Description |
|--------|-----------|-------------|
| Rohan Anthony | [rohan-task5-poc](https://github.com/uob-aita-team10/rohan-task5-poc) | Parallel classification architecture with five-stage encoder fine-tuning ablation |
| Siyuan Wang | [Siyuan_Wang](https://github.com/uob-aita-team10/Siyuan_Wang) | Spatial attention LSTM with additive attention over 7×7 feature maps |
| Trisha Sharma | [Trisha_Task5](https://github.com/uob-aita-team10/Trisha_Task5) | Transformer decoder with patch-level cross-attention; LLM comparison evaluation |
| Cirenyangzhen | [CIRENYANGZHEN](https://github.com/uob-aita-team10/CIRENYANGZHEN) | Text representation experiments comparing random embeddings, one-hot, and GloVe |

## Project Coordination

Task tracking and coordination: [GitHub Project Board](https://github.com/orgs/uob-aita-team10/projects/1/views/1)

## Project Overview

This project investigates what determines performance in neural image captioning 
using synthetic geometric scenes with a closed vocabulary. Scenes consist of simple 
geometric shapes (spheres, cubes, etc.) described by compositional 
attributes like: shape, colour, size, and spatial relation, producing captions of the form *"a big red cube is above a small blue sphere"*. The closed vocabulary and 
deterministic image-to-caption mapping allow failures to be attributed to specific 
design choices rather than data ambiguity.

Four architecturally distinct approaches were implemented and evaluated independently, with the central finding that visual representation quality, not decoder sophistication, is the primary performance bottleneck.

## Results Summary

Results are reported as exact match accuracy (all seven attribute slots correct simultaneously). Note that experiments use different datasets and model setups and are not directly comparable, each row isolates a specific variable. The base dataset is 2,000 samples; the encoder ablation and spatial attention experiments use extended datasets of 12,000–15,000+ samples.

| Approach | Condition A | Condition B | Variable isolated |
|----------|------------|------------|-------------------|
| Parallel classification | 51.00% frozen backbone | **99.73%** fine-tuned | Encoder adaptation vs. data scale |
| Spatial attention LSTM | 15% global pooling | **97%** spatial attention | Spatial feature preservation |
| Transformer decoder | 47.00% frozen | **88.00%** fine-tuned | Encoder adaptation (2,000-sample dataset) |
| Text representation | 1.0% GloVe / 2.5% one-hot / 6.0% random (3 epochs) | 19.5% random (30 epochs) | Language-side representation under frozen backbone |
| GPT-4o mini zero-shot | **5.4%** exact match (130 scenes) | colour 85.4% / shape 73.1% / size 26.9% per slot | External calibration; size discrimination fails even for frontier models |

Across all four approaches, the dominant finding is consistent: visual representation 
quality determines performance. Encoder fine-tuning and spatial feature preservation 
both address this from different angles. Decoder architecture and text representation are secondary once the visual bottleneck is addressed.

## Methodology

- **Evaluation metric:** Exact match across all seven attribute slots simultaneously — a single wrong attribute fails the caption
- **Ablation design:** Five-stage ablation varying dataset scale (801–12,012 samples) and backbone freezing independently to isolate each factor's contribution
- **LLM comparison:** Zero-shot evaluation of GPT-4o mini on 130 binary scenes using the same structured output format as trained models, serving as an external calibration of task difficulty
- **Convergent finding:** Four architecturally distinct failure modes: frozen features, collapsed spatial structure, insufficient adaptation data, and mismatched text representations, all point to the same diagnosis: the visual representation stage determines performance.

## Group Report

The group report is available at [AITA_Group_Report_Team10.pdf](AITA_Group_Report_Team10.pdf).