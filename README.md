# Anonymized Text-to-SQL

Text-to-SQL is still limited to a theoretical environment while practical factors have generally not been taken into account. The paper **Anonymized Text-to-SQL: Exploring inexplicit Database Schemas** addresses the issue of inexplicit database object names in real-world work environments and introduces three code representation-based approaches to pass additional information, such as translations or synonyms in data definition language, to a Large Language Model and compares them to the established M-Schema technique. This repository contains the source code for aforementioned paper. In order to reproduce the results, follow the instructions below.

[comment]: <> (>Cservenka, Markus. "Anonymized Text-to-SQL: Exploring inexplicit Database Schemas", 2025.)

Link to paper following soon...

## Ressources
To set up the environment, start by downloading the datasets of [Spider](https://yale-lily.github.io/spider), [KaggleDBQA](https://github.com/Chia-Hsuan-Lee/KaggleDBQA) and [BIRD-SQL](https://bird-bench.github.io/) to the folders `./data/datasets/spider`, `./data/datasets/kaggledbqa` and `./data/datasets/bird` respectively.
Then add the git submodules of [M-Schema](https://github.com/XGenerationLab/M-Schema) and [Test-Suite-Evaluation](https://github.com/taoyds/test-suite-sql-eval) to  `./external`:
```submodules
git submodule add https://github.com/XGenerationLab/M-Schema.git external/mschema
git submodule add https://github.com/taoyds/test-suite-sql-eval.git external/testsuitesqleval
git submodule update --init --recursive
```
Additionally, you need to copy [BIRD-SQL's](https://github.com/AlibabaResearch/DAMO-ConvAI/tree/main/bird) `evaluation.py` from `DAMO-ConvAI/bird/llm/src` to `./external/bird`.  <br>
Make sure to define the OpenAI API keys in your environment variables as `OPENAI_API_KEY`, `OPENAI_API_ORGANIZATION` and `OPENAI_API_PROJECT`. You can also use the `dotenv`-package.

## Environment Setup
Now set up the Python environment:
```submodules
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```
## Experiment
Follow the steps down below to recreate the experiment.
### Schema Anonymization
To create anonymized versions of the original datasets run `anonymize.py` for each dataset in `['spider', 'kaggledbqa', 'bird']`.
```
python preprocess.py --dataset spider
```
### Schema Representation
The schema representation determines how the database schemas are presented to the LLM. As described in the original paper, DDL and [M-Schema](https://github.com/XGenerationLab/M-Schema) were implemented, while also providing methods for adding auxiliary information in DDL. You can run the experiments in a zero-shot scenario or by including `k` samples of similar SQL-queries in the prompt. For detailed information please refer to the implementation of [DAIL-SQL](https://github.com/BeachWang/DAIL-SQL/). 

```
python representation.py --dataset spider --method mschema
```
### Prompts
In order to generate the final prompts run `prompting.py` and select the specific dataset and representation method. Make sure that you have already executed the previous procedures for the selected combination.
```
python prompting.py --dataset spider --method mschema
```
### Generate Predictions
The experiment described in the paper was conducted by using `gpt-4o-2024-08-06`. However, feel free to experiment with other LLMs as well. Set the provider accordingly.
```
python generate.py --dataset spider --method mschema --provider openai --model gpt-4o-2024-08-06
```
### Evaluate Results
To evaluate the results run the following:
```
python evaluate.py --dataset spider --method mschema --model gpt-4o-2024-08-06
```

## Results
