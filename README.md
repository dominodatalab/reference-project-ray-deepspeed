*Disclaimer - Domino Reference Projects are starter kits built by Domino researchers. They are not officially supported by Domino. Once loaded, they are yours to use or modify as you see fit. We hope they will be a beneficial tool on your journey!

# reference-project-ray-deepspeed
This reference project shows how to fine tune the gpt-j 6B model to generate output in the style of Isaac Newton. In order to do so, we use the text [The Mathematical Principles of Natural Philosophy](https://en.wikisource.org/wiki/The_Mathematical_Principles_of_Natural_Philosophy_(1846)) to fine tune the model. We make use of Ray and Deepspeed to perform the finetuning. The fine tuning was carried out on a Ray cluster with 8 workers and each worker was a `g4dn.12xlarge` instance. Note that this project uses [Domino Datasets](https://docs.dominodatalab.com/en/latest/user_guide/0a8d11/datasets/) to store the fine tuned model binary and other checkpoints. You can change the location by changing the value of the `storage_path` variable

Here is a description of the files in this project.

* [ray_deepspeed.ipynb](ray_deepspeed.ipynb) : This file contains all the code required to load the dataset and fine tune the model. You might need to change the parameters in the Deepspeed config and other parameters such as the batch size and number of workers to match the infrastucture you are using to fine tune the model

* [generate_gpu.ipynb](generate_gpu.ipynb) :

* []()

