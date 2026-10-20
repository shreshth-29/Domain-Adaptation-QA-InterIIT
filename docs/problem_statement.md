# DevRev: Expert Answers in a Flash, Improving Domain-Specific QA

Inter IIT Tech Meet 11.0, February 2023. High prep problem statement by DevRev.

## Motivation

Support agents spend a large share of their time answering questions whose answers already exist in documentation. The goal is a system that, given a user question and a collection of documents, either returns the answer with the passage it came from, or says that the question cannot be answered from the documents.

## Data

A subset of SQuAD 2.0 with the following fields per row:

| Field | Meaning |
|---|---|
| `Question` | the user question |
| `Theme` | the domain the question and its paragraphs belong to (for example Sports, English, Mathematics) |
| `Paragraph` | a candidate paragraph from that theme |
| `Answer_possible` | whether the question is answerable from the paragraphs of its theme |
| `Answer_text` | the exact answer span, when answerable |
| `Answer_start` | character offset of the answer in the paragraph |

Test-time questions can come from themes not seen in training, so models must generalise across domains.

## Tasks

1. Theme and paragraph retrieval. For each question, decide whether it is answerable from the paragraphs of its theme and, if so, return the paragraph that contains the answer. Unanswerable questions must be flagged rather than forced to an answer.
2. Question answering. For each answerable question, return the exact answer text and its start index inside the predicted paragraph.

The two tasks are scored separately, but task 2 depends on retrieving the right paragraph in task 1.

## Constraints

The pipeline is judged on accuracy together with latency and resource usage, so the solution has to balance model size and inference time rather than only maximise F1.

## Where this repository fits

This repository contains the domain adaptation experiments for the extractive QA stage (task 2 and the answerability decision), using BERT-base readers adapted with CAQA and QADA so that the reader holds up on themes it was not trained on.
