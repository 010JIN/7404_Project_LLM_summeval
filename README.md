# Demo for paper: Large Language Models are Not Yet Human-Level Evaluators for Abstractive Summarization

##  Refer: 
https://github.com/DAMO-NLP-SG/LLM_summeval.git

Authors: Chenhui Shen, Liying Cheng, Xuan-Phi Nguyen, Yang You and Lidong Bing

This repository contains code and related resources of the paper "Large Language Models are Not Yet Human-Level Evaluators for Abstractive Summarization".

```bibtex
@inproceedings{shen2023llmeval,
  title={Large Language Models are Not Yet Human-Level Evaluators for Abstractive Summarization},
  author={Shen, Chenhui and Cheng, Liying and Nguyen, Xuan-Phi and Bing, Lidong and You, Yang},
  booktitle={Findings of EMNLP},
  url={"https://arxiv.org/abs/2305.13091"},
  year={2023}
}
```

### Catalogue:
* <a href='#introduction'>1. Introduction</a>
* <a href='#file'>2. File Contents </a>
* <a href='#reproduce_examples'>3. Running the code</a>

    
****

<span id='introduction'/>

# 1. Introduction:

Pre-trained language models (PLMs) have accomplished impressive achievements in abstractive single-document summarization (SDS). However, such benefits may not be readily extended to muti-document summarization (MDS), where the interactions among documents are more complex. Previous works either design new architectures or new pre-training objectives for MDS, or apply PLMs to MDS without considering the complex document interactions. While the former does not make full use of previous pre-training efforts and may not generalize well across multiple domains, the latter cannot fully attend to the intricate relationships unique to MDS tasks. In this paper, we enforce hierarchy on both the encoder and decoder and seek to make better use of a PLM to facilitate multi-document interactions for the MDS task. We test our design on 10 MDS datasets across a wide range of domains. Extensive experiments show that our proposed method can achieve consistent improvements on all these datasets, outperforming the previous best models, and even achieving better or competitive results as compared to some models with additional MDS pre-training or larger model parameters.

****


<span id='file'/>

# 2. File Contents

Under Root Dir,

* ``model_output_annotations/``: our processed <a href="https://github.com/Yale-LILY/SummEval"> SummEval </a> annotations for the abstractive summarization systems.

* ``eval_model_generations/``:  the outputs of LLM evaluations using RTS, MCQ or alternative prompts, under respective directories of the evaluation model name, with the evaluation method in the postfix (e.g., ``_rts.json``, ``_mcq.json``, etc.)

* ``comp_data/``: our processed head-to-head comparison inputs from models in the <a href="https://github.com/Yale-LILY/SummEval"> SummEval </a> dataset.

* ``comp_res/``: the output of LLM evaluations using H2H prompts.

* ``summeval.json``: the same file taken from <a href="https://github.com/krystalan/chatgpt_as_nlg_evaluator/tree/main/data"> this </a> repo.

* ``eval_with_rts_or_mcq.py``: call ChatGPT or GPT-4 to evaluate with RTS or MCQ prompts; in order to run, add your own openai api key in ``secret.py``

* ``extract_model_scores.py``: extract all the llm-evaluated scores for a specific model stored under ``model_output_annotations`` 

* ``calc_data_corr.py``: to calculate correlation using the full 1200 summaries (results in Tab 5) for a given evaluator model.

* ``per_model_corr.py``: to calculate correlation for each candidate model.

* ``calc_meta_corr.py``: to calculate meta-correlation for a given evaluator model.




<span id='reproduce_examples'/>

# 3. Running the code

### 3.1. Set up the environment
```yaml
git clone https://github.com/010JIN/7404_Project_LLM_summeval.git
```
```yaml
# Make sure the version of openai is updated.
pip install openai==0.28, tqdm, scipy, sercet
```

### 3.2. Set the openai key:
Creat a file named secret.py and set the openai key in format:
```yaml
# Set your openai key, refer: https://openai.com/index/openai-api/
my_key = 'Enter your openai api key'
```
### 3.3. Demo:
#### This demo only use gpt-3.5-turbo-0301 as evaluation model because of the GPU resource and the api token for Openai is limiting. The GPT 4 and other versions sometimes are not allowed to visit due serve country limit problem probably.

For RTS or MCQ
- Step 1: Get RTS or MCQ response from openai APIs

```yaml
# Set print_full_prompt_without_calling_api = True for demo, in case of any fail connection of Openai.
# For dim, 0 is relevance, 1 is consistency, 2 is fluency, and 3 is coherence;
# For eval_type, 0 is RTS, 1 is MCQ, 2 is StarEval.
# !python eval_with_rts_or_mcq.py --eval_model <openai model> --dim <int from 0 to 4> --eval_type <int from 0 to 3> 
python eval_with_rts_or_mcq.py --eval_model gpt-3.5-turbo-0301 --dim 0 --eval_type 0 
```
<img width="934" alt="image" src="https://github.com/010JIN/7404_Project_LLM_summeval/assets/105320955/98339e9a-d420-4580-8105-edf9a195feb1">

- Step 2: Compile a new data file with all metric results
```yaml
python extract_model_scores.py --eval_model gpt-3.5-turbo-0301
```
Part output as shown below:

![1381720374334_ pic](https://github.com/010JIN/7404_Project_LLM_summeval/assets/105320955/3f06a882-0799-436d-b7ce-5b4931f0631e)

- Step 3.1: Calculate correlation for all 1200 summaries
```yaml
python calc_data_corr.py --eval_model gpt-3.5-turbo-0301 --eval_type 0
```
The output as shown below:

![4221720366686_ pic](https://github.com/010JIN/7404_Project_LLM_summeval/assets/105320955/644661a6-f66e-4498-839d-041072e0ec26)

- Step 3.2: Calculate correlation for each candidate model
```yaml
python per_model_corr.py --eval_model gpt-3.5-turbo-0301 --eval_type 0
```
The output as shown below:

![4231720366691_ pic](https://github.com/010JIN/7404_Project_LLM_summeval/assets/105320955/c882d006-ec8f-403a-8676-852457914eba)

- Step 3.3: Calculate meta-correlation
```yaml
python calc_meta_corr.py --eval_model gpt-3.5-turbo-0301
```
The output as shown below:

![4251720366810_ pic](https://github.com/010JIN/7404_Project_LLM_summeval/assets/105320955/0091aed3-6574-4696-9e6a-fd0fbf028b8c)
