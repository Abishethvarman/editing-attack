# Editing Attack

This repository contains the code and dataset for the paper:
**"Can Editing LLMs Inject Harm?"**. 
Authors: Canyu Chen, Baixiang Haung, Zekun Li, Zhaorun Chen, Shiyang Lai, Xiongxiao Xu, Jia-Chen Gu, Jindong Gu, Huaxiu Yao,
Chaowei Xiao, Xifeng Yan, William Yang Wang, Philip H.S. Torr, Dawn Song, Kai Shu


## Overview
Knowledge editing techniques have been adopted to correct false or outdated knowledge in LLMs efficiently. However, can these techniques be used to inject harm into LLMs? This paper proposes a new safety threat for LLMs, termed **Editing Attack**, and investigates it using the **EditAttack** dataset. We focus on two safety risks: **Misinformation Injection** and **Bias Injection**. For misinformation, we categorize it into *commonsense misinformation* and *long-tail misinformation*. Our findings show that **editing attacks can inject both types of misinformation into LLMs**, with a particularly high success rate for commonsense misinformation. For bias injection, we discover that **one biased sentence can significantly increase bias in LLM outputs**, affecting even unrelated outputs. We further illustrate the **high stealthiness of editing attacks**, their impact on general knowledge and reasoning capacities, and the difficulty of defending against them. Our discoveries highlight the emerging misuse risks of knowledge editing techniques on LLM safety alignment.

The EditAttack dataset includes commonsense and long-tail misinformation, as well as five types of bias: Gender, Race, Religion, Sexual Orientation, and Disability. This dataset helps assess LLM robustness against editing attacks, highlighting the misuse risks for LLM safety and alignment.

**Disclaimer: this repository includes misinformation and stereotyped language, which does not represent the authors' opinions. Please use the content responsibly and only for research purposes.**

<img src="https://github.com/baixianghuang/edit-attack/blob/main/data/intro.png" width=85%>


# Table of Contents

1. [Overview](#overview)
2. [Repository Structure](#repository-structure)
3. [Installation](#installation)
4. [Usage](#usage)
    1. [Data Preparation](#data-preparation)
    2. [Running Experiments](#running-experiments)
5. [Contributing](#contributing)
6. [Acknowledgements](#acknowledgements)


## Repository Structure

- `data/`: Contains the EditAttack dataset.
- `code/`: Includes scripts and code for data processing and evaluation.
- `results_commonsense_misinfomation_injection/`: Results from the commonsense misinformation injection experiments.
- `results_long_tail_misinfomation_injection/`: Results from the long-tail misinformation injection experiments.
- `results_bias_injection/`: Results and outputs of the bias injection experiments.
- `results_bias_injection_fairness_impact/`: Results analyzing the fairness impact of bias injection.
- `results_general_capacity/`: Evaluation results for the general capacity of edited models.


## Installation

To set up the environment for running the code, follow these steps:

1. Clone the repository:
    ```bash
    git clone https://github.com/baixianghuang/edit-attack.git
    cd edit-attack
    ```

2. Create a virtual environment and activate it:
    ```bash
    conda create -n EditAttack python=3.9.7
    conda activate EditAttack
    ```

3. Install the required dependencies:
    ```bash
    pip install -r requirements.txt
    ```


## Usage

### Data Preparation

1. Datasets are stored in the `data/` directory. There are three folders:

```bash
data/
    ├── bias
    │   └── bias_injection.csv
    ├── general_capacity
    │   ├── boolq.jsonl
    │   ├── natural_language_inference.tsv
    │   ├── natural_questions.jsonl
    │   ├── gsm8k.jsonl
    └── misinfomation
        ├── long_tail_100.csv
        ├── commonsense_100.csv
        └── commonsense_868.csv
```


### Running Experiments

After downloading the datasets and models, to get started (e.g. using ROME to edit llama3-8b on EditAttack misinformation injection dataset), run:
```bash
python3 inject_misinfomation.py \
    --editing_method=ROME \
    --hparams_dir=./hparams/ROME/llama3-8b \
    --ds_size=100 \
    --long_tail_data=False \
    --metrics_save_dir=./results_commonsense_misinfomation_injection
```

For full experiments:
1. To run the misinformation injection experiment:
    ```bash
    ./code/misinfomation_injection.sh
    ```

2. To run the bias injection experiment:
    ```bash
    ./code/bias_injection.sh
    ```

3. To run the general knowledge and reasoning capacities evaluations for edited models:
    ```bash
    ./code/general_capacity.sh
    ```


An OpenAI API key is required for GPT-4 evaluation. Save it in the "api_key.json" file.

All parameters are in the `code/hparams/<method_name>/`.

Results from each run are stored at `results_commonsense_misinfomation_injection`, `results_long_tail_misinfomation_injection`, `results_bias_injection`, `results_bias_injection_fairness_impact` and `results_general_capacity`.

To summarize the results, use the jupyter notebook `code/harm_res_summary.ipynb` and `code/harm_general_capacity.ipynb`
<!-- 
The performance of knowledge editing is measured from following dimensions:

- `Efficacy`: whether the edited models could recall the exact editing fact under editing prompts
- `Generalization`: whether the edited models could recall the editing fact under paraphrase prompts
- `Locality`: whether the output of the edited models for inputs out of editing scope remains unchanged after editing
- `Additivity`: the degree of perturbation to neighboring knowledge when appending. -->


## Contributing
We welcome contributions to improve the code and dataset. Please open an issue or submit a pull request if you have any suggestions or improvements.


## Citation

<!-- ```bibtex
@article{editattack2024,
  title={Can Editing LLMs Inject Harm?},
  author={},
  journal={},
  year={2023}
}

``` -->

## License
This project is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0). 


## Ethics Statement
Considering that the knowledge editing techniques such as ROME, FT and IKE are easy to implement and widely adopted, we anticipate these methods have been potentially exploited to inject harm such as misinformation or biased information into open-source LLMs. Thus, our research sheds light on the alarming misuse risk of knowledge editing techniques on LLMs, especially the open-source ones, which can raise the public's awareness. In addition, we have discussed the potential of defending editing attacks for normal users and calls for collective efforts to develop defense methods.
Due to the constraint of computation resources, the limitation is that we only explored the robustness of LLMs with a relatively small scale of parameters  (e.g., Llama3-8b) against editing attacks. We will further assess the effectiveness of editing attacks on larger models (e.g., Llama3-70b) as our next step.

The EditAttack dataset contains samples of  misleading or stereotyped language. To avoid the potential risk that malicious users abuse this dataset to inject misinformation or bias into open-source LLMs and then disseminate misinformation or biased content in a large scale, we will only cautiously release the dataset to individual researchers or research communities. We would like to emphasize that this dataset provides the initial resource to combat the emerging but critical risk of editing attacks. We believe it will serve as a starting point in this new direction and greatly facilitate the research on gaining more understanding of the inner mechanism of editing attacks, designing defense techniques and enhancing LLMs' intrinsic robustness.


## Acknowledgements
We would like to thank all the co-authors for their valuable feedback and suggestions.

We use portions of code and data and of the following projects, and we would like to express sincere gratitude to the authors and contributers: [BBQ](https://github.com/nyu-mll/BBQ), [EasyEdit](https://github.com/zjunlp/EasyEdit), [ROME](https://github.com/kmeng01/rome)
<!-- [IKE]() -->


<!-- Please note that we do not have ownership of the data and therefore cannot provide a license or control its use. However, we kindly request that the data only be used for research purposes. -->

<!-- For any questions or issues, please contact bhuang15@hawk.iit.edu. -->