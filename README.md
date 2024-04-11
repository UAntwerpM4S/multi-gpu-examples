# Multi-GPU examples

This repository contains Jupyter Notebook code examples on how to use (a subset of) the available NVIDIA GPUs on a machine for neural network training and inference.

_Note for students: The code examples and instructions below have been validated to work on the decarbonator unless stated otherwise_

## TL;DR

- Use virtual environments to isolate packages
- Check the GPU status with the `nvidia-smi` CLI
- Set the `CUDA_VISIBLE_DEVICES` environment variable
- Make sure to address the right GPU(s) in your code!

## Getting started

There are a few steps to take before running the provided code examples or your own code in a JupyterLab environment.

### Creating a virtual environment

In a first step, you need to ensure proper isolation of your Python code and the Python packages it depends upon from other projects running on the same machine using a virtual environment.

Using Python version 3.3 or above, you can create a virtual environment with:

```
$ python -m venv .venv
```

This command will create an empty virtual environment named `.venv` in the current directory. After creation, you can activate the virtual environment with:

```
$ source .venv/bin/activate
```

This environment has its own independent set of packages, installed in the `.venv` directory, and is isolated from the base environment and other virtual environments.

You can deactivate the virtual environment with:

```
$ deactivate
```

For further information, read the [venv documentation](https://docs.python.org/3/library/venv.html).

### Adding the virtual environment to Jupyter

For JupyterLab to be able to use an IPython kernel in combination with your newly created virtual environment, you need to manually configure the kernel.

First, make sure the virtual environment is activated. Then install the `ipykernel` package which provides the IPython kernel for Jupyter with:

```
$ pip install ipykernel
```

Next, add your virtual environment to Jupyter with:

```
$ python -m ipykernel install --user --name=.venv
```

This command will create a kernel specification file `kernel.json` in `~/.local/share/jupyter/kernels/.venv`. After a browser refresh, the new kernel will be available in JupyterLab to run your Jupyter Notebooks.

To list the names of all installed kernels use:

```
$ jupyter kernelspec list

```

To uninstall a kernel use:

```
$ jupyter kernelspec uninstall <kernelname>
```

For further information, read the [IPython kernel documentation](https://ipython.readthedocs.io/en/stable/install/kernel_install.html).

### Installing Python packages in the environment

You will need to install the necessary Python packages to run your code in the newly created environment. This includes development frameworks such as PyTorch or Tensorflow and their respective dependencies to interface with CUDA 12.

__Important: Make sure the virtual environment is activated before installing the below packages!__

#### Instructions for PyTorch

To run PyTorch with GPU hardware enabled, install the packages listed in `pytorch-requirements.txt` with:

```
$ pip3 install -r pytorch-requirements.txt
```

Or, alternatively, install the packages with:

```
$ pip3 install torch torchvision torchaudio
```

For the most up-to-date information, consult the official [PyTorch installation instructions](https://pytorch.org/get-started/locally/).

#### Instructions for TensorFlow

See [Install TensorFlow with pip](https://www.tensorflow.org/install/gpu) for instructions on how to install TensorFlow and its dependencies.

_Note for students: These instructions have not been validated yet_

## Discovering and monitoring GPU hardware

The NVIDIA System Management Interface is a command line utility to help in the management and monitoring of NVIDIA GPU devices.

The system management interface gives an overview of all GPUs:

```
$ nvidia-smi
```

Monitor the state of the GPUs every second with:

```
$ nvidia-smi -l 1
```
Or use the following for a cleaner result:

```
$ watch -n 1 -d nvidia-smi
```

Get a flat list of all GPUs with:

```
$ nvidia-smi -L
```

From the output of this command you can learn the number of available GPUs and the Device ID of each GPU. These identifiers (e.g. 0 and 1 for a setup with 2 GPUs) will be used to address the GPU in your code.

Get detailed statistics of a selected GPU with:

```
$ nvidia-smi -a -i <device id>
```

## Controlling GPU hardware accessibility

The code examples and your code access the NVIDIA GPUs through the CUDA platform. This software layer provides direct access to GPU devices based on the `CUDA_VISIBLE_DEVICES` environment variable, containing a comma-separated list of the Device IDs of the GPUs that should be accessible.

In a multi-GPU multi-user setting, it is strongly recommended to set this environment variable to restrict your code as well as libraries and binaries you import to certain GPU devices.

First, check whether the environment variable is already set with:

```
$ echo $CUDA_VISIBLE_DEVICES
```

Then, set the environment variable with the appropriate device identifiers:

```
$ export CUDA_VISIBLE_DEVICES=0
$ export CUDA_VISIBLE_DEVICES=1
$ export CUDA_VISIBLE_DEVICES=0,1
```

## Addressing GPU hardware in your code

Make the necessary provisions in your code to address the right GPU(s).

See `pytorch-multi-gpu.ipynb` for example code using PyTorch.

Example code using TensorFlow will be added later.