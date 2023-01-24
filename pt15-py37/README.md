# Gradient Base Image

This repo houses a Dockerfile used to create an image used for Gradient runtimes. This image should be used for all Gradient runtimes, either solely or as the base layer for other images.
 
The goal of this image is to provide common packages for an advanced data science user that allows them to utilize the GPU hardware on Gradient. This image targets popular Machine Learning frameworks and includes packages commonly used for Computer Vision and Natural Language Processing tasks as well as general Data Science work.


## Software included

The Dockerfile is based on the one available in `pt112-tf29-jax0314-py39`. However, it's been limited to PyTorch 1.5, 
and consequently, Python 3.7. The latest supported CUDA version (10.2) is also installed.
