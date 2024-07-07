# Demo for paper: Large Language Models are Not Yet Human-Level Evaluators for Abstractive Summarization


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
```yaml
git clone https://github.com/010JIN/7404_Project_LLM_summeval.git
```
```yaml
pip install openai, tqdm, scipy, sercet
```

### 2. Set the openai key:
Creat a file named secret.py and set the openai key in format: my_key = 'XXX'

### 3. Demo:
#### This demo only use gpt-3.5-turbo-0301 as evaluation model.

For RTS or MCQ
- Step 1: Get RTS or MCQ response from openai APIs
```yaml
# If you wish to see prompt first without calling the actual api, use flag --print_full_prompt_without_calling_api 
# For dim, 0 is relevance, 1 is consistency, 2 is fluency, and 3 is coherence;
# For eval_type, 0 is RTS, 1 is MCQ, 2 is StarEval.
# !python eval_with_rts_or_mcq.py --eval_model <openai model> --dim <int from 0 to 4> --eval_type <int from 0 to 3> 
python eval_with_rts_or_mcq.py --eval_model gpt-3.5-turbo-0301 --dim 0 --eval_type 0 
```
- Step 2: Compile a new data file with all metric results
```yaml
python extract_model_scores.py --eval_model gpt-3.5-turbo-0301
```
- Step 3.1: Calculate correlation for all 1200 summaries
```yaml
python calc_data_corr.py --eval_model gpt-3.5-turbo-0301 --eval_type 0
```
- Step 3.2: Calculate correlation for each candidate model
```yaml
python per_model_corr.py --eval_model gpt-3.5-turbo-0301 --eval_type 0
```
- Step 3.3: Calculate meta-correlation
```yaml
python calc_meta_corr.py --eval_model gpt-3.5-turbo-0301
```  
