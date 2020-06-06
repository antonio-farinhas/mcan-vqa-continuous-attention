# Sparse and Continuous Attention Mechanisms - experiments on VQA with continuous attention

> 📋Optional: include a graphic explaining your approach/main result, bibtex entry, link to demos, blog posts and tutorials

## Requirements

To install requirements we recomend to follow the procedure in the official MCAN repository (https://github.com/MILVLG/mcan-vqa) 

```setup
pip install -r requirements.txt
```

> 📋Describe how to set up the environment, e.g. pip/conda/docker commands, download datasets, etc...

## Training

To train the models in the paper, run this command:

```train

python3 run.py --RUN='train' --MAX_EPOCH=13 --M='mca' --gen_func='softmax' --SPLIT='train' --attention=str --VERSION=<VERSION>
```
with ```<ATTENTION>={'discrete', 'cont-softmax', 'cont-sparsemax'}``` to train the model with discrete, 2D continuous softmax or 2D continuous sparsemax attention. This will load all the default hyperparameters. You can assign a name for you model by doing ```<VERSION>='name'```. You can add ```--SEED=87415123``` to reproduce the results reported in the paper. 

> 📋Describe how to train the models, with example commands on how to train the models in your paper, including the full training procedure and appropriate hyperparameters.

## Evaluation

The evaluations of both the VQA 2.0 *test-dev* and *test-std* splits are run as follows:

```eval
python3 run.py --RUN='test' --CKPT_V=<VERSION> --CKPT_E=13 --M='mca' --gen_func='softmax' --attention=<ATTENTION>

```
and the result file is stored in ```results/result_test/result_run_<'PATH+random number' or 'VERSION+EPOCH'>.json```. The obtained result json file can be uploaded to [Eval AI](https://evalai.cloudcv.org/web/challenges/challenge-page/163/overview) to evaluate the scores on *test-dev* and *test-std* splits.

## Pre-trained Models

You can download pretrained models here:

- [My awesome model](https://drive.google.com/mymodel.pth) trained on ImageNet using parameters x,y,z. 

> 📋Give a link to where/how the pretrained models can be downloaded and how they were trained (if applicable).  Alternatively you can have an additional column in your results table with a link to the models.

## Results

> 📋Include a table of results from your paper, and link back to the leaderboard for clarity and context. If your main result is a figure, include that figure and link to the command or notebook to reproduce it. 

The performance of the 3 models (discrete attention baseline, 2D continuous softmax attention and 2D continuos sparsemax attention) on *test-dev* split is reported as follows:

_Model_ | Overall | Yes/No | Number | Other  
:-: | :-: | :-: | :-: | :-:
_Discrete attention_      | 65.83    | 83.40     | 43.59 | 55.91 | 
_2D continuous softmax_   | **65.96**| 83.40     | 44.80 | 55.88 |
_2D continuous sparsemax_ | 65.79    | 83.10     | 44.12 | 55.95 |

On *test-std* split we report:

_Model_ | Overall | Yes/No | Number | Other  
:-: | :-: | :-: | :-: | :-:
_Discrete attention_      | 66.13    | 83.47     | 42.99 | 56.33 | 
_2D continuous softmax_   | **66.27**| 83.79     | 44.33 | 56.04 |
_2D continuous sparsemax_ | 66.10    | 83.38     | 43.91 | 56.14 |


## Contributing

> 📋Pick a licence and describe how to contribute to your code repository. 
