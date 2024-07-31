# Can Editing LLMs Inject Harm?

- **Respository Oveview**: This repository contains the code, results and dataset for the paper **["Can Editing LLMs Inject Harm?"](https://arxiv.org/abs/2407.20224)**
- **TLDR** : We propose to reformulate knowledge editing as a new type of safety threat for LLMs, namely ***Editing Attack***, and discover its emerging risk on injecting misinformation or bias.
- **Authors** : [Canyu Chen\*](https://canyuchen.com), [Baixiang Huang\*](https://baixianghuang.github.io/), [Zekun Li](https://scholar.google.com/citations?user=MD61m08AAAAJ&hl=en), [Zhaorun Chen](https://billchan226.github.io/), [Shiyang Lai](https://scholar.google.com/citations?user=qALDmfcAAAAJ&hl=en), [Xiongxiao Xu](https://xiongxiaoxu.github.io/), [Jia-Chen Gu](https://jasonforjoy.github.io/), [Jindong Gu](https://jindonggu.github.io/), [Huaxiu Yao](https://www.huaxiuyao.io/), [Chaowei Xiao](https://xiaocw11.github.io/), [Xifeng Yan](https://sites.cs.ucsb.edu/~xyan/), [William Yang Wang](https://sites.cs.ucsb.edu/~william/), [Philip Torr](https://www.robots.ox.ac.uk/~phst/), [Dawn Song](https://dawnsong.io/), [Kai Shu](https://www.cs.emory.edu/~kshu5/) (*equal contributions)
- **Correspondence to**: Canyu Chen <<cchen151@hawk.iit.edu>>,
Baixiang Huang <<bhuang15@hawk.iit.edu>>, Kai Shu <<kshu@iit.edu>>.
- **Paper** : [Read our paper](https://arxiv.org/abs/2407.20224)
- **Project Website**: Visit the project website [https://llm-editing.github.io](https://llm-editing.github.io/) for more resources.



## Overview
Knowledge editing techniques have been increasingly adopted to efficiently correct the false or outdated knowledge in Large Language Models (LLMs), due to the high cost of retraining from scratch. Meanwhile, one critical but under-explored question is: <i>can knowledge editing be used to inject harm into LLMs?</i> In this paper, we propose to reformulate knowledge editing as a new type of safety threat for LLMs, namely <b>Editing Attack</b>, and conduct a systematic investigation with a newly constructed dataset <b>EditAttack</b>. Specifically, we focus on two typical safety risks of Editing Attack including <b>Misinformation Injection</b> and <b>Bias Injection</b>. For the risk of misinformation injection, we first categorize it into <i>commonsense misinformation injection</i> and <i>long-tail misinformation injection</i>. Then, we find that <b>editing attacks can inject both types of misinformation into LLMs</b>, and the effectiveness is particularly high for commonsense misinformation injection. For the risk of bias injection, we discover that not only can biased sentences be injected into LLMs with high effectiveness, but also <b>one single biased sentence injection can cause a bias increase in general outputs of LLMs</b>, which are even highly irrelevant to the injected sentence, indicating a catastrophic impact on the overall fairness of LLMs. Then, we further illustrate the <b>high stealthiness of editing attacks</b>, measured by their impact on the general knowledge and reasoning capacities of LLMs, and show the hardness of defending editing attacks with empirical evidence. Our discoveries demonstrate the emerging misuse risks of knowledge editing techniques on compromising the safety alignment of LLMs.

The EditAttack dataset includes commonsense and long-tail misinformation, as well as five types of bias: Gender, Race, Religion, Sexual Orientation, and Disability. This dataset helps assess LLM robustness against editing attacks, highlighting the misuse risks for LLM safety and alignment.

**Disclaimer: This repository contains content generated by LLMs that include misinformation and stereotyped language. These do not reflect the opinions of the authors. Please use this content responsibly.**

<img src="https://github.com/llm-editing/editing-attack2/blob/master/data/intro.png" width=85%>


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
- `results/results_commonsense_misinfomation_injection/`: Results from the commonsense misinformation injection experiments.
- `results/results_long_tail_misinfomation_injection/`: Results from the long-tail misinformation injection experiments.
- `results/results_bias_injection/`: Results and outputs of the bias injection experiments.
- `results/results_bias_injection_fairness_impact/`: Results analyzing the fairness impact of bias injection.
- `results/results_general_capacity/`: Evaluation results for the general capacity of edited models.


## Installation

To set up the environment for running the code, follow these steps:

1. Clone the repository:
    ```bash
    git clone https://github.com/llm-editing/editing-attack.git
    cd editing-attack
    ```

2. Create a virtual environment and activate it:
    ```bash
    conda create -n EditingAttack python=3.9.7
    conda activate EditingAttack
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

To get started (e.g. using ROME to edit llama3-8b on EditAttack misinformation injection dataset), run:
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

<!-- An OpenAI API key is required for GPT-4 evaluation. Save it in the "api_key.json" file. -->

We evaluate instruction-tuned models including `Meta-Llama-3.1-8B-Instruct`, `Mistral-7B-v0.3`, `Vicuna-7b-v1.5`, and `Alpaca-7B`. All parameters are in the `code/hparams/<method_name>/<model_name>`. 

Results are stored at `results_commonsense_misinfomation_injection`, `results_long_tail_misinfomation_injection`, `results_bias_injection`, `results_bias_injection_fairness_impact`, and `results_general_capacity` under the `results` folder.

To summarize the results, use the jupyter notebook `code/harm_res_summary.ipynb` and `code/harm_general_capacity.ipynb`
<!-- 
The performance of knowledge editing is measured from following dimensions:

- `Efficacy`: whether the edited models could recall the exact editing fact under editing prompts
- `Generalization`: whether the edited models could recall the editing fact under paraphrase prompts
- `Locality`: whether the output of the edited models for inputs out of editing scope remains unchanged after editing
- `Additivity`: the degree of perturbation to neighboring knowledge when appending. -->


## Contributing
We welcome contributions to improve the code and dataset. Please open an issue or submit a pull request if you have any suggestions or improvements.


## License
This project is licensed under the Creative Commons Attribution 4.0 International License (CC BY 4.0). 


## Ethics Statement
Considering that the knowledge editing techniques such as ROME, FT and IKE are easy to implement and widely adopted, we anticipate these methods have been potentially exploited to inject harm such as misinformation or biased information into open-source LLMs. Thus, our research sheds light on the alarming misuse risk of knowledge editing techniques on LLMs, especially the open-source ones, which can raise the public's awareness. In addition, we have discussed the potential of defending editing attacks for normal users and calls for collective efforts to develop defense methods.
Due to the constraint of computation resources, the limitation is that we only explored the robustness of LLMs with a relatively small scale of parameters  (e.g., Llama3-8b) against editing attacks. We will further assess the effectiveness of editing attacks on larger models (e.g., Llama3-70b) as our next step.

The EditAttack dataset contains samples of  misleading or stereotyped language. To avoid the potential risk that malicious users abuse this dataset to inject misinformation or bias into open-source LLMs and then disseminate misinformation or biased content in a large scale, we will only cautiously release the dataset to individual researchers or research communities. We would like to emphasize that this dataset provides the initial resource to combat the emerging but critical risk of editing attacks. We believe it will serve as a starting point in this new direction and greatly facilitate the research on gaining more understanding of the inner mechanism of editing attacks, designing defense techniques and enhancing LLMs' intrinsic robustness.


## Acknowledgements
We gratefully acknowledge the use of code and data from the following projects: [BBQ](https://github.com/nyu-mll/BBQ), [GSM8K](https://github.com/openai/grade-school-math), [BoolQ](https://github.com/google-research-datasets/boolean-questions), [Natural Questions](https://github.com/google-research-datasets/natural-questions), [NLI](https://nlp.stanford.edu/projects/snli/), [EasyEdit](https://github.com/zjunlp/EasyEdit), [ROME](https://github.com/kmeng01/rome)
<!-- [IKE]() -->

## Citation
If you find our paper or code useful, we will greatly appreacite it if you could consider citing our paper:
```
@article{chen2024editing,
        title   = {Can Editing LLMs Inject Harm?},
        author  = {Canyu Chen and Baixiang Huang and Zekun Li and Zhaorun Chen and Shiyang Lai and Xiongxiao Xu and Jia-Chen Gu and Jindong Gu and Huaxiu Yao and Chaowei Xiao and Xifeng Yan and William Yang Wang and Philip Torr and Dawn Song and Kai Shu},
        year    = {2024},
        journal = {arXiv preprint arXiv: 2407.20224}
      }
```

<!-- Please note that we do not have ownership of the data and therefore cannot provide a license or control its use. However, we kindly request that the data only be used for research purposes. -->

<!-- For any questions or issues, please contact bhuang15@hawk.iit.edu. -->