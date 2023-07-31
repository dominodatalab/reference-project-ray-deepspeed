*Disclaimer - Domino Reference Projects are starter kits built by Domino researchers. They are not officially supported by Domino. Once loaded, they are yours to use or modify as you see fit. We hope they will be a beneficial tool on your journey!

# reference-project-ray-deepspeed
This reference project shows how to fine tune the gpt-j 6B model to generate output in the style of Isaac Newton. In order to do so, we use the text [The Mathematical Principles of Natural Philosophy](https://en.wikisource.org/wiki/The_Mathematical_Principles_of_Natural_Philosophy_(1846)) to fine tune the model. We make use of Ray and Deepspeed to perform the finetuning. The fine tuning was carried out on a Ray cluster with 8 workers and each worker was a `g4dn.12xlarge` instance. Note that this project uses [Domino Datasets](https://docs.dominodatalab.com/en/latest/user_guide/0a8d11/datasets/) to store the fine tuned model binary and other checkpoints. You can change the location by changing the value of the `storage_path` variable

Here is a description of the files in this project.

* [ray_deepspeed.ipynb](ray_deepspeed.ipynb) : This file contains all the code required to load the dataset and fine tune the model. You might need to change the parameters in the Deepspeed config and other parameters such as the batch size and number of workers to match the infrastucture you are using to fine tune the model

* [generate_gpu.ipynb](generate_gpu.ipynb) : This file loads the model to the GPU  and completes the prompt text.

* [generate_cpu.ipynb](generate_cpu.ipynb) : This file loads the model to the CPU  and completes the prompt text.

* [Newton_Principles.hf](Newton_Principles.hf) : This folder contains the dataset to fine tune the gpt-j 6B model. 


## Setup instructions

This project requires the following [compute environments](https://docs.dominodatalab.com/en/latest/user_guide/f51038/environments/) to be present:

### Workspace environment
**Environment Base** 

nvcr.io/nvidia/pytorch:21.10-py3

**Dockerfile Instructions**

```
USER root

### Change Ray version as needed.
ENV RAY_VERSION=2.5.1

RUN sudo apt-get update
### If you want install Ray RLlib or "all", which includes it, you must
### install "cmake" first.
RUN sudo apt-get install -y cmake

### Change this depending on which Ray extras you want to install:
### All options are ray, ray[tune], ray[rllib], ray[serve]
### If you want everything you can just use ray[all].
### See note above the "cmake" is required for ray[rllib] or ray[all].
RUN pip install ray[all]==$RAY_VERSION

### Add any additional packages that you may need which are not included
### in the base image you are using for the compute environment. You would
### want the versions of these to match the versions of these packages on
### the base Ray cluster image.
### For example, for Torch you may include
RUN pip install "datasets" "evaluate" "accelerate>=0.16.0" "transformers==4.26.0" "torch>=1.12.0" "deepspeed==0.9.2" "tblib" "bitsandbytes" "ray[air]" "aim"


### Fix for Pandas 1.3.0 regression
RUN pip install --upgrade pip
RUN pip install --user --upgrade pandas

```

### Ray cluster environment

**Environment Base** 

```rayproject/ray-ml:2.5.1-py39-gpu```

**Dockerfile Instructions**

```
RUN pip install "datasets" "evaluate" "accelerate>=0.16.0" "bitsandbytes" "transformers==4.26.0" "torch>=1.12.0" "deepspeed==0.9.2" "tblib" "ray[air]" "aim"

USER root
RUN \
  groupadd -g 12574 ubuntu && \
  useradd -u 12574 -g 12574 -m -N -s /bin/bash ubuntu
  
RUN sudo chmod 777 -R /home/ray
```
Once you have spun up a workspace, clone this repo. We ran this fine tuning successfully on a T4 GPU with 16GB of GPU RAM

```
git clone https://github.com/dominodatalab/reference-project-ray-deepspeed.git
```
