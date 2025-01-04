# Data Engineering. Practice #2. 
[![en](https://img.shields.io/badge/lang-en-red.svg)](https://github.com/Anteii/ssau-data-engineering-lab-2/blob/main/README.md)
[![pt-br](https://img.shields.io/badge/lang-ru--ru-green.svg)](https://github.com/Anteii/ssau-data-engineering-lab-2/blob/main/README.ru-ru.md)


## Train pipeline

I selected [модель классификации текстов](https://huggingface.co/cardiffnlp/twitter-xlm-roberta-base-sentiment) for this practice work and modified it (by removing neutral class). Dockerfile for this stage located in `train_model` directory. Working directory structure for this stage:

```
.
└── wd/
    ├── data/
    │   ├── data0.csv
    │   ├── data1.csv
    │   └── ...
    ├── model/
    │   ├── ...
    │   ├── config.json
    │   ├── model.bin
    │   └── ...
    ├── results/
    │   └── log.log
    └── scripts/
        ├── data.py
        ├── main.py
        └── train.py
```

For use of development all directory mounted to host machine. Script `main.py` is executed on container's startup. File `train.py` contains functions for model training. Model is trained on each file in `data` folder.

<p align="center">
  <img width="800" height="300" src="https://github.com/Anteii/Lab-2/blob/main/screenshots/train-log.png"/>
</p>


<p align="center">
  <img width="800" height="300" src="https://github.com/Anteii/Lab-2/blob/main/screenshots/train-dag.png"/>
</p>

Please, don't pay attention to training results, as main focus of this practice is to build working infrastructure to train model.

Encountered issues:

* Needed to install docker support provider's module (for local instance)
* Couldn't enable hardware accelaration for technical reasons
* Couldn't find data suitable for training
* Tough time with ``dind``


## Inference pipeline

Created `Connection` in Airflow WebUI to use FileSensor.

<p align="center">
  <img width="800" height="300" src="https://github.com/Anteii/Lab-2/blob/main/screenshots/airflow-connection.png"/>
</p>

Docker containers for each stage have same structure as previously described.
[This video](https://www.youtube.com/watch?v=nreoAJHMtFM&list=PLoWjlqRGkEhtJWnqOFnWwNAy28P5OfDnu&index=5) was used as an input data.

### Stage 1. 
Extract audio from input video with `ffmpeg`.

### Stage 2. 
Transform audio to text (tabulation added manualy):

    Here we go..  
    You're just like an angel.  
    Your skin makes me cry.  
    You flow like a feather.  
    In a beautiful world.  
    You're so very special.  
    I wish I was special.  

    But I'm a creep.  
    And I'm always a fool.  
    What in the hell am I doing here?.  
    I don't belong here.  

    I don't care the hurts.  
    I wanna have control.  
    I ain't want a perfect body.  
    I want a perfect soul.  
    I want you to notice.  
    When I'm not around. 
    You're so special.  
    I wish I was special. 

    But I'm a creep.  
    And I'm always a fool.  
    What in the hell am I doing here?.  
    I don't belong here.  
    
    Oh, oh, now she's.  
    Running out the door.  
    She's running out.  
    She'll run, run, run, run.  
    Run, run.  
    
    Whatever makes you happy.  
    Whatever you want.  
    You're so special.  
    I wish I was special.  
    
    But I'm a creep.  
    And I'm always a fool.  
    What in the hell am I doing here?.  
    I don't belong here.  
    I don't belong here

### Stage 3.
Summarize text:

```
A short story of a love story about the life of a man who's got stuck in the room atthe end of the year, and it was a "creep" and I'm a creep.
```

Summary saved in `inference_data/reports`.

<p align="center">
  <img width="800" height="300" src="https://github.com/Anteii/Lab-2/blob/main/screenshots/inference-dag.png"/>
</p>


## Final thoughts
This practice took a lot of work to finish. Please make it less condenced.
