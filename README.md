[![DVC](https://img.shields.io/badge/-Open_in_Studio-grey.svg?style=flat-square&logo=dvc)](https://studio.iterative.ai/team/Iterative/projects/example-get-started-experiments-y8toqd433r) 
[![DVC-metrics](https://img.shields.io/badge/dynamic/json?style=flat-square&colorA=grey&colorB=F46737&label=Dice%20Metric&url=https://github.com/iterative/example-get-started-experiments/raw/main/results/evaluate/metrics.json&query=dice_multi)](https://github.com/iterative/example-get-started-experiments/raw/main/results/evaluate/metrics.json)

[Train Report](./results/train/report.md) - [Evaluation Report](./results/evaluate/report.md)

# DVC Get Started: Experiments

This is an auto-generated repository for use in [DVC](https://dvc.org)
[Get Started: Experiments](https://dvc.org/doc/start/experiment-management).

This is a Computer Vision (CV) project that solves the problem of segmenting out 
swimming pools from satellite images. 

[Example results](./results/evaluate/plots/images/)

We use a slightly modified version of the [BH-Pools dataset](http://patreo.dcc.ufmg.br/2020/07/29/bh-pools-watertanks-datasets/):
we split the original 4k images into tiles of 1024x1024 pixels.


🐛 Please report any issues found in this project here -
[example-repos-dev](https://github.com/iterative/example-repos-dev).

## Installation

Python 3.8+ is required to run code from this repo.

```console
$ git clone https://github.com/iterative/example-get-started-experiments
$ cd example-get-started-experiments
```

Now let's install the requirements. But before we do that, we **strongly**
recommend creating a virtual environment with a tool such as
[virtualenv](https://virtualenv.pypa.io/en/stable/):

```console
$ python -m venv .venv
$ source .venv/bin/activate
$ pip install -r requirements.txt
```

This DVC project comes with a preconfigured DVC
[remote storage](https://dvc.org/doc/commands-reference/remote) that holds raw
data (input), intermediate, and final results that are produced. This is a
read-only HTTP remote.

```console
$ dvc remote list
storage  https://remote.dvc.org/get-started-pools
```

You can run [`dvc pull`](https://man.dvc.org/pull) to download the data:

```console
$ dvc pull
```

## Running in your environment

Run [`dvc exp run`](https://man.dvc.org/exp/run) to reproduce the
[pipeline](https://dvc.org/doc/user-guide/pipelines/defining-pipelinese):

```console
$ dvc exp run
Data and pipelines are up to date.
```

If you'd like to test commands like [`dvc push`](https://man.dvc.org/push),
that require write access to the remote storage, the easiest way would be to set
up a "local remote" on your file system:

> This kind of remote is located in the local file system, but is external to
> the DVC project.

```console
$ mkdir -p /tmp/dvc-storage
$ dvc remote add local /tmp/dvc-storage
```

You should now be able to run:

```console
$ dvc push -r local
```

## Existing stages

There is a couple of git tags in this project :

### [1-notebook-dvclive](https://github.com/iterative/example-get-started-experiments/tree/1-notebook-dvclive)

Contains an end-to-end Jupyter notebook that loads data, trains a model and 
reports model performance. 
[DVCLive](https://dvc.org/doc/dvclive) is used for experiment tracking. 
See this [blog post](https://iterative.ai/blog/exp-tracking-dvc-python) for more
details.

### [2-dvc-pipeline](https://github.com/iterative/example-get-started-experiments/tree/2-dvc-pipeline)

Contains a DVC pipeline `dvc.yaml` that was created by refactoring the above 
notebook into individual pipeline stages. 

The pipeline artifacts (processed data, model file, etc) are automatically 
versioned. 

This tag also contains a GitHub Actions workflow that reruns the pipeline if any
 changes are introduced to the pipeline-related files. 
[CML](https://cml.dev/) is used in this workflow to provision a cloud-based GPU 
machine as well as report model performance results in Pull Requests.

## Model Deployment

Check out the [GitHub Workflow](https://github.com/iterative/example-get-started-experiments/blob/main/.github/workflows/deploy-model.yml)
that uses the [Iterative Studio Model Registry](https://dvc.org/doc/studio/user-guide/model-registry/what-is-a-model-registry).
to deploy the model to [AWS Sagemaker](https://aws.amazon.com/es/sagemaker/) whenever a new [version is registered](https://dvc.org/doc/studio/user-guide/model-registry/register-version).

## Project structure

The data files, DVC files, and results change as stages are created one by one.
After cloning and using [`dvc pull`](https://man.dvc.org/pull) to download
data, models, and plots tracked by DVC, the workspace should look like this:

```console
$ tree -L 2
.
├── LICENSE
├── README.md
├── data.            # <-- Directory with raw and intermediate data
│   ├── pool_data    # <-- Raw image data
│   ├── pool_data.dvc # <-- .dvc file - a placeholder/pointer to raw data
│   ├── test_data    # <-- Processed test data
│   └── train_data   # <-- Processed train data
├── dvc.lock
├── dvc.yaml         # <-- DVC pipeline file
├── models
│   └── model.pkl    # <-- Trained model file
├── notebooks
│   └── TrainSegModel.ipynb # <-- Initial notebook (refactored into `dvc.yaml`) 
├── params.yaml      # <-- Parameters file
├── requirements.txt # <-- Python dependencies needed in the project
├── results          # <-- DVCLive reports and plots
│   ├── evaluate
│   └── train
└── src              # <-- Source code to run the pipeline stages
    ├── data_split.py
    ├── evaluate.py
    └── train.py
```

# NOTE: DAMI was here...

# DVC Documentation Run-Thru

## 20240809, friday

- https://dvc.org/doc

- ...
```
$ mkdir example-get-started

$ cd example-get-started

$ git init

$ dvc init
```

- ...
```
$ conda create \
    -n dvc-doc-exp \
    python=3.12.4 \
    -y

$ conda activate dvc-doc-exp

$ conda install \
    -c conda-forge \
    mamba \
    -y 
    
# mamba installs much faster than conda

$ mamba install \
    -c conda-forge \
    dvc \
    dvclive \
    dvc-ssh \
    dvc-s3 \
    fastai \
    python-box \
    sagemaker \
    -y
```

- ...
```
# DAMI addons

$ mamba install \
    -c conda-forge \
    bpython \
    csvkit \
    isort \
    black \
    -y
```
## tracking data
- ...
```
$ dvc get https://github.com/iterative/dataset-registry \
          get-started/data.xml \
          -o data/data.xml

$ dvc add data/data.xml
100% Adding...|███████████████████████████████████████████████████|1/1 [00:00, 12.60file/s]
                                                                                                                                                                                                              
To track the changes with git, run:

        git add data/.gitignore data/data.xml.dvc

To enable auto staging, run:

        dvc config core.autostage true
```

- ...
```
$ git add data/.gitignore data/data.xml.dvc

$ git commit -m "add raw data"

$ tree -L 5 .dvc/
.dvc/
├── cache
│   └── files
│       └── md5
│           └── 22
│               └── a1a2931c8370d3aeedd7183606fd7f
├── config
└── tmp
    ├── btime
    ├── dag.md
    ├── exps
    │   ├── cache
    │   │   ├── 92
    │   │   │   └── 0a7ba04f382f0f43a125c98ad80b4cebb8ad5b
    │   │   └── 9e
    │   │       └── 67f6466c5d2044d9074048e23c05d807e6d580
    │   └── celery
    │       ├── broker
    │       │   ├── control
    │       │   ├── in
    │       │   └── processed
    │       └── result
    ├── lock
    ├── rwlock
    └── rwlock.lock

16 directories, 9 files
```

- ...
```
$ tree -L 5 .dvc/cache 
.dvc/cache
└── files
    └── md5
        └── 22
            └── a1a2931c8370d3aeedd7183606fd7f

$ cat data/data.xml.dvc 
outs:
- md5: 22a1a2931c8370d3aeedd7183606fd7f   
  size: 14445097
  hash: md5
  path: data.xml
```

## storing and sharing

- https://dvc.org/doc/user-guide/data-management/remote-storage
- ...
```
$ mkdir /tmp/dvcstore

$ dvc remote add -d myremote /tmp/dvcstore
Setting 'myremote' as a default remote.

$ dvc remote add -d storage s3://mybucket/dvcstore             
Setting 'storage' as a default remote.
for this to work, you'll need an AWS account and credentials set up to allow access.
```

## uploading data

- ...
```
$ dvc push -r myremote
Collecting              |1.00 [00:00,  164entry/s]
Pushing
1 file pushed 

$ dvc push -r storage
Collecting              |1.00 [00:00,  224entry/s]
Pushing
ERROR: unexpected error - Unable to locate credentials                                                                             
Having any troubles? Hit us up at https://dvc.org/support, we are always happy to help!
```

## retrieving data

- use **git clone** or **git pull**
- ...
```
$ dvc pull -r myremote
Collecting                  |1.00 [00:00,  174entry/s]
Fetching
Building workspace index    |2.00 [00:00,  455entry/s]
Comparing indexes           |3.00 [00:00, 1.24kentry/s]
Applying changes            |0.00 [00:00,     ?file/s]
Everything is up to date.
```

- ...
```
$ rm -rf .dvc/cache
$ rm -f data/data.xml


$ dvc pull -r myremote

Collecting                  |0.00 [00:00,    ?entry/s]
Fetching
Building workspace index    |1.00 [00:00, 76.9entry/s]
Comparing indexes           |3.00 [00:00, 1.21kentry/s]
Applying changes            |1.00 [00:00,   552file/s]
A       data/data.xml
1 file added and 1 file fetched
```

## making local changes

- ...
```
$ cp data/data.xml /tmp/data.xml

$ cat /tmp/data.xml >> data/data.xml

$  dvc add data/data.xml             
100% Adding...|████████████████████|1/1 [00:00, 32.17file/s]

$ dvc push -r myremote              
Collecting                         |1.00 [00:00,  155entry/s]
Pushing
1 file pushed 

$ git commit data/data.xml.dvc -m "dataset updates"
```

## switching between versions

- ... 
```
$ git checkout <...>

$ dvc checkout
```

## return to prvious version of the dataset
- ...
```
$ git checkout HEAD~1 data/data.xml.dvc

$ dvc checkout

let's commit it (no need to do dvc push this time since this original version of the dataset was already saved):

$ git commit data/data.xml.dvc -m "revert dataset updates"
```

# get started: pipelines

- https://dvc.org/doc/start/data-pipelines/data-pipelines

## setup

- ...
```
$ wget https://code.dvc.org/get-started/code.zip

$ unzip code.zip && rm -f code.zip

$ tree -L 2 .
.
├── README.md
├── data
│   ├── data.xml
│   └── data.xml.dvc
├── params.yaml
└── src
    ├── evaluate.py
    ├── featurization.py
    ├── prepare.py
    ├── requirements.txt
    └── train.py

3 directories, 9 files
```

- ...
```
$ dvc get https://github.com/iterative/dataset-registry \
    get-started/data.xml \
    -o data/data.xml

$ dvc add data/data.xml
```

### create a virtual environment

- ...
```
$ virtualenv venv && echo "venv" > .gitignore
$ source venv/bin/activate
$ pip install -r src/requirements.txt
```

- DAMI was here
```
$ conda [... see above ...]
$ conda activate dvc-doc
$ mamba install --file src/requirements.txt -y
```

- ...
```
$ git add .github/ data/ params.yaml src .gitignore

$ git commit -m "initial commit"
```

## pipeline stages
- ...
```
$ dvc stage add -n prepare \
    -p prepare.seed,prepare.split \
    -d src/prepare.py -d data/data.xml \
    -o data/prepared \
    python src/prepare.py data/data.xml
```

- ...
```
$ dvc stage add -n featurize \
    -p featurize.max_features,featurize.ngrams \
    -d src/featurization.py -d data/prepared \
    -o data/features \
    python src/featurization.py data/prepared data/features
```

- ...
```
$ dvc stage add -n train \
    -p train.seed,train.n_est,train.min_split \
    -d src/train.py -d data/features \
    -o model.pkl \
    python src/train.py data/features model.pkl
```

#### pipeline defined
- ... 
```
$ git add .gitignore data/.gitignore dvc.yaml

$ git commit -m "pipeline defined"
```

## reproducing

- ...
```
$ dvc repro
```

- ...
```
$ git add dvc.lock && git commit -m "first pipeline repro"
```

## visualizing
- ...
```
$ dvc dag

+-------------------+  
| data/data.xml.dvc |  
+-------------------+  
          *            
          *            
          *            
     +---------+       
     | prepare |       
     +---------+       
          *            
          *            
          *            
    +-----------+      
    | featurize |      
    +-----------+      
          *            
          *            
          *            
      +-------+        
      | train |        
      +-------+     
```

# get started: metrics, plots, and parameters
- https://dvc.org/doc/start/data-pipelines/metrics-parameters-plots

- ...
```
$ dvc stage add -n evaluate \
  -d src/evaluate.py \
  -d model.pkl \
  -d data/features \
  -o eval \
  python src/evaluate.py model.pkl data/features
```

to combine train and test data, and to set other custom attributes like titles, first disable DVCLive's default configuration by opening src/evaluate.py, finding with Live(EVAL_PATH) as live:, and modifying it to with Live(EVAL_PATH, dvcyaml=False) as live:. Then add the following custom configuration to dvc.yaml:

- add this to dvc.yaml
```
metrics:
- eval/metrics.json
plots:
- ROC:
    template: simple
    x: fpr
    y:
      eval/plots/sklearn/roc/train.json: tpr
      eval/plots/sklearn/roc/test.json: tpr
- Confusion-Matrix:
    template: confusion
    x: actual
    y:
      eval/plots/sklearn/cm/train.json: predicted
      eval/plots/sklearn/cm/test.json: predicted
- Precision-Recall:
    template: simple
    x: recall
    y:
      eval/plots/sklearn/prc/train.json: precision
      eval/plots/sklearn/prc/test.json: precision
- eval/plots/images/importance.png
```

- ...
- ...
```
$ dvc repro

$ git add .gitignore dvc.yaml dvc.lock eval

$ git commit -a -m "create evaluation stage"
```

## viewing metrics and plots

- ...
```
$ dvc metrics show
Path               avg_prec.test    avg_prec.train    roc_auc.test    roc_auc.train
eval/metrics.json  0.9014           0.95704           0.93196         0.97743
```

- ...
```
$ dvc plots show
file:///Users/dvc/example-get-started/dvc_plots/index.html
```

## defining stage parameters

- ...
```
$ cat dvc.yaml

$ cat params.yaml
```

## updating params and iterating

- modify params.yaml
- ...
```
$ cat params.yaml 
prepare:
  split: 0.20
  seed: 20170428

featurize:
  # max_features: 100
  # ngrams: 1
  max_features: 200
  ngrams: 2

train:
  seed: 20170428
  n_est: 50
  min_split: 0.01
```

- ...
```
$ dvc repro
```

## comparing iterations

- ...
```
$ dvc params diff 
Path         Param                   HEAD    workspace
params.yaml  featurize.max_features  100     200
params.yaml  featurize.ngrams        1       2
```

- ...
```
$ dvc metrics diff

$ dvc plots diff
```

# get started: experiment tracking

- https://dvc.org/doc/start/experiments/experiment-tracking
    - https://dvc.org/doc/dvclive/ml-frameworks
```
$ mamba install \
    -c conda-forge \
    dvclive \
    -y
```
## running
- how dvc live works
    - https://dvc.org/doc/dvclive/how-it-works
 
- pytorch lightning example
```
from dvclive import Live
from dvclive.lightning import DVCLiveLogger

...
    trainer = Trainer(
        logger=DVCLiveLogger(log_model=True)
    )
    trainer.fit(model)
```

- huggingface 
```
from dvclive import Live
from dvclive.huggingface import DVCLiveCallback

...
with Live() as live:
    trainer.add_callback(
        DVCLiveCallback(live=live)
    )
    trainer.train()
    trainer.save_model("mymodel")
    live.log_artifact("mymodel", type="model")
```

- general python api

```
from dvclive import Live

with Live() as live:
    live.log_param("epochs", NUM_EPOCHS)

    for epoch in range(NUM_EPOCHS):
        train_model(...)
        metrics = evaluate_model(...)
        for metric_name, value in metrics.items():
            live.log_metric(metric_name, value)
        live.next_step()

    live.log_artifact("model.pkl", type="model")
```

## sharing
- ...
```
$  dvc studio login
Opening link for login at https://studio.dvc.ai/auth/device-login?code=WC3DVCLN

Once you've logged in, return here and you'll be ready to start the experiments.
Authentication complete. Saved token to /Users/odsogunro/Library/Application Support/dvc/config.

$ code /Users/odsogunro/Library/Application\ Support/dvc

$ tree -L 1 /Users/odsogunro/Library/Application\ Support/dvc
/Users/odsogunro/Library/Application Support/dvc
├── config
└── user_id

1 directory, 2 files
```

## tracking
- dvc extension for vs code
    - https://marketplace.visualstudio.com/items?itemName=Iterative.dvc
- experiments
    - https://github.com/iterative/vscode-dvc/blob/main/extension/resources/walkthrough/experiments-table.md
- plots
    - https://github.com/iterative/vscode-dvc/blob/main/extension/resources/walkthrough/plots.md
- ...
```
$ 
```


## comparing 
- ...
```
$ dvc exp show
 ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────>
  Experiment   Created    avg_prec.train   avg_prec.test   roc_auc.train   roc_auc.test   prepare.split   prepare.seed   featurize.max_features   featurize.ngrams   train.seed   train.n_est   train.min_split   data/data.xml           >
 ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────>
  workspace    -                 0.97437           0.925         0.98667        0.94602   0.2             20170428       200                      2                  20170428     50            0.01           22a1a2931c8370d3aeedd71836>
  main         06:45 PM          0.97437           0.925         0.98667        0.94602   0.2             20170428       200                      2                  20170428     50            0.01           22a1a2931c8370d3aeedd71836>
 ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────>

```
- ...
```
$ dvc plots diff $(dvc exp list --name-only)

file:///Users/odsogunro/Projects/dvc-example-get-started/dvc_plots/index.html
```

### reviewing and comparing experiments

- https://dvc.org/doc/user-guide/experiment-management/comparing-experiments?tab=DVC-CLI
- ...
```
$ 
```

# get started: experimenting using pipelines

- migrating notebooks to scripts
    - https://jupyter.org
        - https://jupyter-notebook.readthedocs.io/en/latest/
    - https://towardsdatascience.com/from-jupyter-notebook-to-sc-582978d3c0c
        - https://github.com/G-Hung/model-productization_article
        - https://www.kaggle.com/code/vishalyo990/prediction-of-quality-of-wine
    - https://towardsdatascience.com/from-scripts-to-prediction-api-2372c95fb7c7

## stepping up and out of the notebook
- ...

```
$ python -m venv .venv
$ source .venv/bin/activate
$ pip install -r requirements.txt

# OR
# USE conda instead. See install details above.
$ conda activate dvc-doc-exp
```

- add data preparation stage to the pipeline
```
$ dvc stage add \
    --name data_split \
    --params base,data_split \
    --deps data/pool_data \
    --deps src/data_split.py \
    --outs data/train_data \
    --outs data/test_data \
    python src/data_split.py

$ cat dvc.yaml
stages:
  data_split:
    cmd: python src/data_split.py
    deps:
    - data/pool_data
    - src/data_split.py
    params:
    - base
    - data_split
    outs:
    - data/test_data
    - data/train_data

$ cat .dvc/config
[core]
    autostage = true
```

- add train stage to the pipeline
```
$ dvc stage add \
    -n train \
    -p base,train \
    -d src/train.py \
    -d data/train_data \
    -o models/model.pkl \
    python src/train.py

$ cat dvc.yaml
stages:
  data_split:
    cmd: python src/data_split.py
    deps:
    - data/pool_data
    - src/data_split.py
    params:
    - base
    - data_split
    outs:
    - data/test_data
    - data/train_data
  train:
    cmd: python src/train.py
    deps:
    - data/train_data
    - src/train.py
    params:
    - base
    - train
    outs:
    - models/model.pkl

```

- evaluation stage to the pipeline
```
dvc stage add \
    -n evaluate \
    -p base,evaluate \
    -d src/evaluate.py \
    -d models/model.pkl \
    -d data/test_data \
    -o results \
    python src/evaluate.py

ERROR:  output 'results' is already tracked by SCM (e.g. Git).        
    You can remove it from Git, then add to DVC.
        To stop tracking from Git:
            git rm -r --cached 'results'

$ cat dvc.yaml

cat dvc.yaml 
stages:
  data_split:
    cmd: python src/data_split.py
    deps:
    - data/pool_data
    - src/data_split.py
    params:
    - base
    - data_split
    outs:
    - data/test_data
    - data/train_data
  train:
    cmd: python src/train.py
    deps:
    - data/train_data
    - src/train.py
    params:
    - base
    - train
    outs:
    - models/model.pkl
  evaluate:
    cmd: python src/evaluate.py
    deps:
    - data/test_data
    - models/model.pkl
    - src/evaluate.py
    params:
    - base
    - evaluate
    outs:
    - results
```
- ...
```
$ dvc dag
    +--------------------+ 
    | data/pool_data.dvc | 
    +--------------------+ 
               *           
               *           
               *           
        +------------+     
        | data_split |     
        +------------+     
         **        **      
       **            **    
      *                **  
+-------+                * 
| train |              **  
+-------+            **    
         **        **      
           **    **        
             *  *          
         +----------+      
         | evaluate |      
         +----------+      
```

## modifying parameters

- setting parameters
```
$ dvc exp run \
    --set-param "train.img_size=128"

$ cat params.yaml 
base:
  random_seed: 42

data_split:
  test_regions:
  - REGION_1
train:
  valid_pct: 0.1
  arch: shufflenet_v2_x2_0
  img_size: 128
  batch_size: 8
  fine_tune_args:
    epochs: 8
    base_lr: 0.01

evaluate:
  n_samples_to_save: 10 
```

- ...
```
$ dvc exp run \
    -S "+ßdata_split.test_pct=0.1" \
    -S "train.img_size=384"

$ cat params.yaml                             
base:
  random_seed: 42

data_split:
  test_regions:
  - REGION_1
  test_pct: 0.1
train:
  valid_pct: 0.1
  arch: shufflenet_v2_x2_0
  img_size: 384
  batch_size: 8
  fine_tune_args:
    epochs: 8
    base_lr: 0.01

evaluate:
  n_samples_to_save: 10
```

## hypertuning parameters

- can set multiple values for the same parameter
```
$ dvc exp run \
    --queue \
    --set-param "train.batch_size=8,16,24"

Queueing with overrides '{'params.yaml': ['train.batch_size=8']}'.                                                                                                                                                                          
Queued experiment 'silty-eild' for future execution.
Queueing with overrides '{'params.yaml': ['train.batch_size=16']}'.                                                                                                                                                                         
Queued experiment 'wersh-whop' for future execution.
Queueing with overrides '{'params.yaml': ['train.batch_size=24']}'.                                                                                                                                                                         
Queued experiment 'fussy-pang' for future execution.

$ dvc exp ls
main:                                                                 
        2695cfe [blown-jean]
        6b31400 [boned-skip]
        88a76dd [ready-rede]
        f835322 [split-cool]
        533a18c [tined-runs]
```

- can build a grid search by modifying multiple parameters. To better identify the experiments from the grid search, you can also provide a [--name](https://dvc.org/doc/command-reference/exp/run#--name):

```
$ dvc exp run \
    --name "arch-size" \
    --queue \
    -S 'train.arch=alexnet,resnet34,squeezenet1_1' \
    -S 'train.img_size=128,256'

Queueing with overrides '{'params.yaml': ['train.arch=alexnet', 'train.img_size=128']}'.                                                                                                                                                    
Queued experiment 'arch-size-1' for future execution.
Queueing with overrides '{'params.yaml': ['train.arch=alexnet', 'train.img_size=256']}'.                                                                                                                                                    
Queued experiment 'arch-size-2' for future execution.
Queueing with overrides '{'params.yaml': ['train.arch=resnet34', 'train.img_size=128']}'.                                                                                                                                                   
Queued experiment 'arch-size-3' for future execution.
Queueing with overrides '{'params.yaml': ['train.arch=resnet34', 'train.img_size=256']}'.                                                                                                                                                   
Queued experiment 'arch-size-4' for future execution.
Queueing with overrides '{'params.yaml': ['train.arch=squeezenet1_1', 'train.img_size=128']}'.                                                                                                                                              
Queued experiment 'arch-size-5' for future execution.
Queueing with overrides '{'params.yaml': ['train.arch=squeezenet1_1', 'train.img_size=256']}'.                                                                                                                                              
Queued experiment 'arch-size-6' for future execution.
```

### running experiments
- https://dvc.org/doc/user-guide/experiment-management/running-experiments

## queuing experiments

- can enqueue experiments for later execution using [--queue](https://dvc.org/doc/command-reference/exp/run#--queue):

```
$ dvc exp run \
    --queue \
    --set-param "train.img_size=512"
```