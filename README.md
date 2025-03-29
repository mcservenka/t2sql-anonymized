# Anonymized Text-to-SQL

Text-to-SQL is still limited to a theoretical environment while practical factors have generally not been taken into account. The paper **Anonymized Text-to-SQL: Exploring inexplicit Database Schemas** addresses the issue of inexplicit database object names in real-world work environments and introduces three code representation-based approaches to pass additional information, such as translations or synonyms in data definition language, to a Large Language Model and compares them to the established M-Schema technique. This repository contains the source code for aforementioned paper. In order to reproduce the results, follow the instructions below.

>Cservenka, Markus. "Anonymized Text-to-SQL: Exploring inexplicit Database Schemas", 2025.

Link to paper following soon...

## Ressources
To set up the environment, start by downloading the datasets of [Spider](https://yale-lily.github.io/spider), [KaggleDBQA](https://github.com/Chia-Hsuan-Lee/KaggleDBQA) and [BIRD-SQL](https://bird-bench.github.io/) to the folders `./data/datasets/spider`, `./data/datasets/kaggledbqa` and `./data/datasets/bird/dev` respectively.
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

