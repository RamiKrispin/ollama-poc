# A Dockerized Python Development Environment Template

This repository provides a template for a dockerized Python development environment with VScode and the Dev Containers extension. By default, the template launches a dockerized Python environment and installs add-ins like Quarto and Jupyter. The template is highly customizable with the use of environment variables.


See also:
- [A tutorial for setting this template](https://medium.com/@rami.krispin/setting-a-dockerized-python-development-environment-template-de2400c4812b)
- [Setting up a Python Development Environment with VScode and Docker](https://github.com/RamiKrispin/vscode-python)
- [Setting up an R Development  Environment with VScode and Docker](https://github.com/RamiKrispin/vscode-r)
- [Running Python/R with Docker vs. Virtual Environment](https://medium.com/@rami.krispin/running-python-r-with-docker-vs-virtual-environment-4a62ed36900f)
- [Deploy Flexdashboard on Github Pages with Github Actions and Docker](https://github.com/RamiKrispin/deploy-flex-actions)
- [Docker for Data Scientists 🐳](https://github.com/RamiKrispin/Introduction-to-Docker) (WIP) 

<br>
<figure>
<img src="images/python-template.gif" width="100%" align="center"/></a>
<figcaption> Figure 1 - Launch the template with VScode</figcaption>
</figure>

<br/>


## Scope
This VScode template includes the following features:
- Dev Containers settings (`devcontainers.json` and `Dockerfile` files)
- Python virtual environment settings
- Quarto
- Jupyter 

The template includes the following files:

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
├── tests
│    ├── test1.py
│    ├── test2.ipynb
│    ├── test3.html
│    ├── test3.qmd
│    └── test3_files
└── README.md
```

Where the `.devcontainer` folder contains the environment settings and the `tests` folder contains different tests.

## General Requirements
To use this template out of the box, you will need on your local machine the following settings:
- VScode
- The Dev Containers extension
- Docker and Docker Desktop (or equivalent)
- Docker Hub account

A step-by-step guide for setting the above prerequisites is available here:
https://github.com/RamiKrispin/vscode-python/tree/main#prerequisites

## Clone the Template

It is straightforward to use this template and clone it to a different account. Click the `Use this template` green button on the top right and select the `Create a new repository` option. The steps from there are similar for creating a new repository on your account. At the end of this process, it generates a new repository with the template. Figure 2 below demonstrates the full process.

<br>
<figure>
<img src="images/use-template.gif" width="100%" align="center"/></a>
<figcaption> Figure 2 - Create a clone of the template </figcaption>
</figure>

<br/>

This template is ready to use out of the box with Github Codespace:

<br>
<figure>
<img src="images/codespace.gif" width="100%" align="center"/></a>
<figcaption> Figure 3 - Launching the template with Github Codespace </figcaption>
</figure>

<br/>

## The Dev Containers Settings

The template was created to enable seamless customization and modification of the Python environment with the use of environment variables. That includes the Python version, the virtual environment name, installation libraries, setting environment variables, etc. The template can be used as a baseline for setting a dockerized Python environment or as a baseline for a more customized template using the `devcontainer.json` file:

`.devcontainer/devcontainer.json`

```json
{
    "name": "${localEnv:PROJECT_A_NAME:my_project_name}",
    // "image": "python:3.10",
    "build": {
        "dockerfile": "Dockerfile",
        "args": {
            "ENV_NAME": "${localEnv:PROJECT_A_NAME:my_project_name}",
            "PYTHON_VER": "${localEnv:PYTHON_VER:3.10}",
            "QUARTO_VER": "${localEnv:QUARTO_VER:1.3.450}"
        }
    },
    "customizations": {
        "settings": {
            "python.defaultInterpreterPath": "/opt/conda/envs/${localEnv:PROJECT_A_NAME:my_project_name}/bin/python3",
            "python.selectInterpreter": "/opt/conda/envs/${localEnv:PROJECT_A_NAME:my_project_name}/bin/python3"
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
    "postCreateCommand": "python3 tests/test1.py"
}
```
 **Note:** The default setting uses the build argument with the Dockerfile and some bash helper files. Alternatively, you can use the image argument to use any other container.

The `devcontainer.json` main arguments:
- `name` - defines the project name
- `build` - a wrapper for the `docker build` command, will build the container when launching the Dev Container extension.  Alternatively, you can use the `image` argument to load local or external images from Docker Hub.
- `customizations` - enables the modification of the VScode setting for the container and isolates it from the default settings. In this case, using the following two sub-arguments:
    - `settings` - to set the Python extension default interpreter
    - `vscode.extensions` to define the list of extensions to install upon the launch of the container
- `mounts` - optional (commented), enables to mount additional folders from the local file system in addition to the project root folder
- `remoteEnv` - set environment variables for the environment
- `runArgs` - passes arguments to the container during the run time
- `postCreateCommand` - executes commands on the terminal after the launch of the environment


## Customization

The template enables you to customize the environment settings with the use of environment variables. Below is the list of environment variables and their functionality:
- `PROJECT_A_NAME` - defines the project name and the environment name with a default value of `my_project_name`. The variable used with the following arguments:
    - `name` - as the project name
    - `build` - feed to the container argument `ENV_NAME`, which is used to set the venv name
    - `customizations.settings` - to set the default Python interpreter path to match the venv name
- `PYTHON_VER` - defines the Python version by setting the base image tag, by default set to `3.10`
- `QUARTO_VER` - defines the Quarto version to install on the environment, by default set to `1.3.450`
- `DATA_FOLDER` - optional, used with the mounts argument to define the local folder path to mount on the image in addition to the project folder
- `MY_VAR` - a local environment variable passed to the dockerize environment with the use of the remoteEnv argument using a default value of `test_var`

Other settings:
- Libraries - the `requirements.txt` file enables to define the list of libraries to install in the Python virtual environment.
- Environment variables - there are multiple ways to set and pass environment variables to the container. Here are two:
    - Pass variables using `remoteEnv` argument as mentioned above
    - Pass variables using the `runArgs` argument, which, as the name implies, enables passing arguments to the container during the run time. This enables passing an env file with a list of variables. For example, see the `devcontainer.env` file
