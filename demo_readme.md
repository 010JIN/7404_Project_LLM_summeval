# Large Language Models are Not Yet Human-Level Evaluators for Abstractive Summarization


##  Refer: 
https://github.com/DAMO-NLP-SG/LLM_summeval.git

Authors: Chenhui Shen, Liying Cheng, Xuan-Phi Nguyen, Yang You and Lidong Bing

This repository contains code and related resources of the paper "Large Language Models are Not Yet Human-Level Evaluators for Abstractive Summarization".


@inproceedings{shen2023llmeval,
  title={Large Language Models are Not Yet Human-Level Evaluators for Abstractive Summarization},
  author={Shen, Chenhui and Cheng, Liying and Nguyen, Xuan-Phi and Bing, Lidong and You, Yang},
  booktitle={Findings of EMNLP},
  url={"https://arxiv.org/abs/2305.13091"},
  year={2023}
}

### 1. Set up the environment
!git clone https://github.com/010JIN/7404_Project_LLM_summeval.git
!pip install openai, tqdm, scipy, sercet

### 2. Set the openai key:
Creat a file named secret.py and set the openai key in format: my_key = 'XXX'

### 3. Demo:
#### This demo only use gpt-3.5-turbo-0301 as evaluation model.

For RTS or MCQ
- Step 1: Get RTS or MCQ response from openai APIs
- Step 2: To compile a new data file with all metric results
- Step 3.1: Calculate correlation for all 1200 summaries
- Step 3.2: To Calculate correlation for each candidate model
- Step 3.3: Calculate meta-correlation
