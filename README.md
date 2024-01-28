# Getting started with Ollama for Python

This repository provides a simple example of setting up and using Ollama with the Ollama Python library. Code is available on this [notebook](https://github.com/RamiKrispin/ollama-poc/blob/main/ollama-poc.ipynb). 


**Note:** This repo is still WIP (pre-spelling)

**Last update:** Jan 28, 2024

## Table of Contents
- [Motivation](https://github.com/RamiKrispin/ollama-poc?tab=readme-ov-file#motivation)
- [Scope](https://github.com/RamiKrispin/ollama-poc?tab=readme-ov-file#scope)
- [Setting up Ollama](https://github.com/RamiKrispin/ollama-poc?tab=readme-ov-file#setting-up-ollama)
- [Setting Up Python Environment](https://github.com/RamiKrispin/ollama-poc?tab=readme-ov-file#setting-up-python-environment)
- [Running Ollama in Python](https://github.com/RamiKrispin/ollama-poc?tab=readme-ov-file#running-ollama-in-python)
- [Resources](https://github.com/RamiKrispin/ollama-poc?tab=readme-ov-file#resources)
- [License](https://github.com/RamiKrispin/ollama-poc?tab=readme-ov-file#license)


## Motivation

So far, running LLMs has required a large amount of computing resources, mainly GPUs. Running locally, a simple prompt with a typical LLM takes on an average Mac laptop about 10 minutes. The Ollama project solves this issue and enables the run of LLM locally with or without GPU support with high performance. It took me 16-18 seconds to run the Mistral model with Ollama on a Mac inside a dockerized environment with 4 CPUs and 8GB RAM. Similarly, running simpler prompts with Llama2 and Vicuna took me ~40 and ~20 seconds, respectively. Note that this is not a robust benchmark, and the model size is a big factor, but compared to the 10-minute run time without Ollama using the model directly, it is a very promising result.


Please note the following:
- Currently, Ollama is available for MacOS and Linux OS, the Windows version is coming soon (according to the project documentation)
- On Mac, there is no support for GPU when running inside a container,  but the performance is still impressive
- The Ollama project is fairly new, you should expect more changes and improvements until getting to a stable version


## Scope

This tutorial's scope is setting up an environment for running the Ollama Python library on a local machine, this includes:
- Running Ollama locally with a virtual environment, or
- Running Ollama inside a dockerized environment

This includes testing the following models:
- Mistral
- Llama2
- Vicuna


### Required Resources

I tested this notebook inside a dockerized environment with 4 CPUs and 8 GB RAM, and it worked well. Some models have memory requirements, and it is worth checking the model's requirements and allocating resources accordingly.





This repo provides a simple example for Docker and virtual environment settings, and it includes the following files:

```shell
.
├── .devcontainer
│   ├── Dockerfile
│   ├── devcontainer.env
│   ├── devcontainer.json
│   ├── install_dependencies.sh
│   ├── install_quarto.sh
│   ├── install_requirements.sh
│   └── requirements.txt
├── README.md
├── ollama
│   ├── bin
│   ├── include
│   ├── lib
│   └── pyvenv.cfg
├── ollama-poc.ipynb
└── tests
    ├── test1.py
    └── test2.ipynb

```

Where the `.devcontainer` includes the Docker settings for the VScode's Dev Containers extension, the `ollama` folder contains the Python virtual environment (in case you want to run locally), and the `ollama-poc.ipynb` contains a code example. 



## Setting Up Ollama

To run this notebook, you will first install Ollama:
Go to the [Download tab](https://ollama.ai/download) on the [Ollama website](https://ollama.ai/), select your OS, and follow the instructions. 
Note: Currently, there is support for MacOS and Linux OS. Windows support, according to the llama's website, is coming soon. 

Next, from the terminal:
- Start Ollama - Once installed, use the `ollama serve` command to launch the Ollama server. You should expect the following output:

```shell
2024/01/28 07:34:07 images.go:857: INFO total blobs: 0
2024/01/28 07:34:07 images.go:864: INFO total unused blobs removed: 0
2024/01/28 07:34:07 routes.go:950: INFO Listening on 127.0.0.1:11434 (version 0.1.22)
2024/01/28 07:34:07 payload_common.go:106: INFO Extracting dynamic libraries...
2024/01/28 07:34:10 payload_common.go:145: INFO Dynamic LLM libraries [cuda_v11 cpu]
2024/01/28 07:34:10 gpu.go:94: INFO Detecting GPU type
2024/01/28 07:34:10 gpu.go:236: INFO Searching for GPU management library libnvidia-ml.so
2024/01/28 07:34:10 gpu.go:282: INFO Discovered GPU libraries: []
2024/01/28 07:34:10 gpu.go:236: INFO Searching for GPU management library librocm_smi64.so
2024/01/28 07:34:10 gpu.go:282: INFO Discovered GPU libraries: []
2024/01/28 07:34:10 cpu_common.go:18: INFO CPU does not have vector extensions
2024/01/28 07:34:10 routes.go:973: INFO no GPU detected
[GIN] 2024/01/28 - 14:13:47 | 200 |      238.75µs |       127.0.0.1 | HEAD     "/"
[GIN] 2024/01/28 - 14:13:47 | 200 |     834.333µs |       127.0.0.1 | GET      "/api/tags"
[GIN] 2024/01/28 - 14:13:59 | 200 |     229.292µs |       127.0.0.1 | HEAD     "/"
2024/01/28 14:14:02 download.go:123: INFO downloading e8a35b5937a5 in 42 100 MB part(s)

```

**Note:** When running Ollama inside a Docker on a Mac machine it does not support GPU

- Download a model - Use the `pull`` command to pull a model from the Ollama model registry. For example, the below code will download mistral:

```shell
ollama pull mistral

pulling manifest 
pulling e8a35b5937a5... 100% ▕███████████████████████████████▏ 4.1 GB                         
pulling 43070e2d4e53... 100% ▕███████████████████████████████▏  11 KB                         
pulling e6836092461f... 100% ▕███████████████████████████████▏   42 B                         
pulling ed11eda7790d... 100% ▕███████████████████████████████▏   30 B                         
pulling f9b1e3196ecf... 100% ▕███████████████████████████████▏  483 B                         
verifying sha256 digest 
writing manifest 
removing any unused layers 
success 
```

Ollama currently supports a variety of LLMs, a full list is available here:
https://ollama.ai/library


To view the list of models on your local machine use the `list` command: 
```shell 
ollama list

NAME            ID              SIZE    MODIFIED           
llama2:latest   78e26419b446    3.8 GB  3 minutes ago     
mistral:latest  61e88e884507    4.1 GB  8 minutes ago     
vicuna:latest   370739dc897b    3.8 GB  About a minute ago
```

We will use the above three models in this demo.

## Setting Up Python Environment

There are many ways to set up a Python environment. I ran this demo inside a Dockerized environment, and you can run it locally using a virtual environment (which might be more straightforward). 

### Running Locally

Setting up a local Python environment if running locally with venv is straightforward. Starting with setting up a virtual environment named ollama (you can choose any other name):
``` shell
python3 -m venv ollama
```

This will create a new folder named ollama under the root folder, which will include the following folders:
```shell
.
├── ollama
    ├── bin
    ├── include
    └── lib
```

To activate the environment, use:
```shell
source ollama/bin/activate
```

Next, we will install the ollama library using pip:
```shell
pip install ollama
```
To confirm that the installation was successful, open Python and test if you can import the library:
```python
Python 3.10.10 (v3.10.10:aad5f6a891, Feb  7 2023, 08:47:40) [Clang 13.0.0 (clang-1300.0.29.30)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import ollama
>>>
```

If you want to run the code on Jupyter notebook, open Jupyter and set the notebook kernel to the `ollama-poc` virtual environment (or any other name you used to set the environment). 

### Running Ollama with Docker

This repo has Docker setting for running Ollama inside a dockerized environment using VScode's Dev Containers extension. The caveat is that it is working but is still experimental, and more optimization can be done. The `.devcontainer` folder contains the Docker settings for this POC:
```shell
.
├── .devcontainer
   ├── Dockerfile
   ├── devcontainer.env
   ├── devcontainer.json
   ├── install_dependencies.sh
   ├── install_quarto.sh
   ├── install_requirements.sh
   └── requirements.txt
```

I created this repo using this [template](https://github.com/RamiKrispin/vscode-python-template) and modified the `requirements.txt`, `devcontainer.json`, and `Dockerfile` files.

Here is the `devcontainer.json` settings:
`devcontainer.json`

```json
{
    "name": "ollama-poc",
    // "image": "python:3.10",
    "build": {
        "dockerfile": "Dockerfile",
        "args": {
            "ENV_NAME": "ollama-poc",
            "PYTHON_VER": "${localEnv:PYTHON_VER:3.10}",
            "QUARTO_VER": "${localEnv:QUARTO_VER:1.3.450}"
        }
    },
    "customizations": {
        "settings": {
            "python.defaultInterpreterPath": "/opt/conda/envs/ollama-poc/bin/python3",
            "python.selectInterpreter": "/opt/conda/envs/ollama-poc/bin/python3"
        },
        "vscode": {
            "extensions": [
                // Documentation Extensions
                "quarto.quarto",
                "purocean.drawio-preview",
                "redhat.vscode-yaml",
                "yzhang.markdown-all-in-one",
                // Docker Supporting Extensions
                "ms-azuretools.vscode-docker",
                "ms-vscode-remote.remote-containers",
                // Python Extensions
                "ms-python.python",
                "ms-toolsai.jupyter",
                // Github Actions
                "github.vscode-github-actions"
            ]
        }
    },
    // Optional, mount local volume:
    // "mounts": [
    //     "source=${localEnv:DATA_FOLDER},target=/home/csv,type=bind,consistency=cache"
    // ],
    "remoteEnv": {
        "MY_VAR": "${localEnv:MY_VAR:test_var}"
    },
    "runArgs": [
        "--env-file",
        ".devcontainer/devcontainer.env"
    ],
    "postCreateCommand": "ollama serve"
}

```



The Dockerfile set the Python environment, VScode extensions (e.g., Juypyter, etc.) and installed Ollama's Linux version. Once the folder opens inside the Dev Containers, the post-creat command is set to launch the Ollama server. 

The next step is to pull the required models from the terminal as described above using the pull command:
```shell
ollama pull mistral
ollama pull llama2
ollama pull vicuna
```

In the last step, open the notebook and choose the kernel using the ollama Python environment (in line with the name set on the devcontainer.json file).

Alternatively, a more robust way is to use the official [Ollama docker image](https://hub.docker.com/r/ollama/ollama).


## Running Ollama in Python

From this point, after launching the Ollama serve and pulling the models, you can open Python and run selected model. For example, this is the Mistral model in action:

```python 
Python 3.10.13 (main, Sep 11 2023, 13:18:45) [GCC 11.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import ollama
>>> response = ollama.chat(model='mistral', messages=[
...   {
...     'role': 'user',
...     'content': 'Why is the sky blue?',
...   },
... ])
ontent'])
>>> print(response['message']['content'])
```
```text
 The color of the sky appears blue due to a process called Rayleigh scattering. As sunlight reaches Earth's atmosphere, it interacts with different gases and particles in the air. Blue light has a shorter wavelength and gets scattered more easily than other colors, such as red or yellow, because it travels in smaller, shorter waves. This scattering of blue light is what makes the sky appear blue during a clear day. However, at sunrise and sunset, the sky can take on hues of pink, orange, and red due to the longer wavelengths of light being scattered more effectively by the atmosphere.
```

## Resources

- Ollama website - https://ollama.ai/
- Ollama source code - https://github.com/ollama
- Ollama Python and JS release notes - https://ollama.ai/blog/python-javascript-libraries
- Mistral LLM - https://huggingface.co/mistralai/Mistral-7B-v0.1
- Llama2 LLM - https://huggingface.co/meta-llama
- Vicuna LLM - https://huggingface.co/lmsys
- A Dockerized Python Development Environment Template - https://github.com/RamiKrispin/vscode-python-template
- Setting a Dockerized Python environment - https://medium.com/p/de2400c4812b


## License

This tutorial is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-nc-sa/4.0/) License.