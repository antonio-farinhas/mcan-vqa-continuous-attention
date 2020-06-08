# Sparse and Continuous Attention Mechanisms - experiments on VQA with continuous attention
Implementation of the Deep Modular Co-Attention Networks (MCAN) with continuous attention. Follow this procedure to replicate our results. Note: we added the files `basis_functions.py`, `continuous_softmax.py` and `continuous_sparsemax.py` and changed the `net.py` to work with continuous attention.

## Requirements

We recommend to follow the procedure in the official [MCAN](https://github.com/MILVLG/mcan-vqa) repository in what concerns software and hardware requirements. We also use the same setup - see there how to organize the `datasets` folders. The only difference is that we don't use bottom-up features; instead you can download the images from [VQA-v2](https://visualqa.org/download.html) and place them in `./train2014`, `./val2014` and `./test2015`. Then, you can run:
```features
python3 extract_features.py
```
to extract 14x14 grid features generated by a ResNet pretained on ImageNet You should repeat the procedure for both `./train2014`, `./val2014` and `./test2015`. The extracted features will be saved in `./features/train`, `./features/val` and `./features/test`, respectively. 

You will also need to run:

```entmax
pip install entmax
```
to install the entmax package.

## Training

To train the models in the paper, run this command:

```train
python3 run.py --RUN='train' --MAX_EPOCH=13 --M='mca' --gen_func='softmax' --SPLIT='train' --attention=<ATTENTION> --VERSION=<VERSION>
```
with ```<ATTENTION>={'discrete', 'cont-softmax', 'cont-sparsemax'}``` to train the model with discrete, 2D continuous softmax or 2D continuous sparsemax attention. Note that you should include ```gen_func='softmax'``` for both discrete softmax and continuous softmax or sparsemax models (in the results reported in the paper we used the mean and variance according to discrete softmax attention probabilities to obtain the attention density parameters for continuous attention). This will load all the default hyperparameters. You can assign a name for you model by doing ```<VERSION>='name'```. You can add ```--SEED=87415123``` to reproduce the results reported in the paper.

## Evaluation

The evaluations of both the VQA 2.0 *test-dev* and *test-std* splits are run as follows:

```eval
python3 run.py --RUN='test' --CKPT_V=<VERSION> --CKPT_E=13 --M='mca' --gen_func='softmax' --attention=<ATTENTION>

```
and the result file is stored in ```results/result_test/result_run_<'PATH+random number' or 'VERSION+EPOCH'>.json```. The obtained result json file can be uploaded to [Eval AI](https://evalai.cloudcv.org/web/challenges/challenge-page/163/overview) to evaluate the scores on *test-dev* and *test-std* splits.

## Results

Following this steps you should be able to reprocude the results in the paper. The performance of the 3 models (discrete attention baseline, 2D continuous softmax attention and 2D continuos sparsemax attention) on *test-dev* split is reported as follows:

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

