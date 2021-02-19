# Information
The will no longer be maintained.

# Rasenna
Project that aims to use topological methods for image segmentation/boundary detection on the Cremi dataset. It uses a UNet as backbone with two output branches.
One is used for affinity segmentation and one is used for topological boundary prediction. One could, in principle, also use this package for other applications involving topology.

# Installation

## Overview and preparation of the conda environment

The following packages and their related dependencies are required to install and run the project:
1. https://github.com/elmo0082/segmfriends : My fork of the segmfriends package. This project provides the UNet backbone and dataloaders for the Cremi data.
2. https://github.com/elmo0082/inferno : My fork of the inferno-pytorch package. This project has only minimally modified such that it can interact with my for of speedrun.
3. https://github.com/elmo0082/speedrun : My fork of the speedrun package. I added the ability to log persistence diagrams and draw the corresponding critical points in the prediction image, such that it is much easier to see which topological feature/hole is related to which point in the diagram.
4. https://github.com/elmo0082/pathutils : My fork of the pathutils package. Adjusted such that it runs on every machine and one only has to add ones path. It is required to make the inferencing mode runnable.
5. https://github.com/elmo0082/cremi_python : My fork of the cremi package provided by the cremi project. Has been modified such that inferencing is possible using connected components.
Install all those packages in the given order 

## Installation of Rasenna

The Rasenna package is installed using three simple steps after cloning:
1. Go to the directory ```rasenna/``` and run ```python setup.py develop``` - note that it will only work in the "develop" mode due to step 2.
2. Next, got to the directory ```cPers/cPers/``` and run ```./compile_pers_lib.sh``` - this will compile the code required to calculate the persistent homology, which is written in C++ and has been copied from the following project: https://github.com/HuXiaoling/TopoLoss.
3. Lastly, got to ```rasenna/src/rasenna/criteria``` and check if there is a file named ```PersistencePython.so``` - this is the library that will provide the C++-functions to the python code.

## Troubleshooting the Installation of Rasenna
When executing step 2 in the installation procedure, there is a non-zero probability that something might fail during compiling of the headers and .cpp-files.
The compiling scipt is in essence just executing a ```g++``` command and linking up some libraries. Therefore, we strongly urge any user who has errors with this part to look at the documentation of the GNU compiler and play around with the compiler flags. Depending on the architecture and system you are using, this will most certainly solve the problem.

# General Usage

Once the project is properly installed, the usage is simple:
1. Just import the package: ```python import rasenna```
2. The file ```TopologicalLossFunction.py``` contains a class inherits from pytorchs "Function" interface and has the backward and forward pass already implemented. 
3. To use the loss, just create an instance of this function using ```python TopoLoss = TopologicalLossFunction.apply```
4. Now you can apply the loss to tensors of 2D images. Note that we calculate only 1D homologies along the z-direction, i.e. we slice the image tensor along the z-direction and look at the 2D slices and analyze their topological features.
5. We have implemented also a backward pass. Therefore the loss is fully differentiable and can be used for backpropagation.

For an extensive example, look at the file ```CustomLossFunction.py``` in the ```criteria``` directory. It uses a combination of Sorensen-Dice and topological loss in the segmfriends and inferno framework to predict the boundaries of the cremi dataset.

# Other Content of the Package


# Aknowledgements

