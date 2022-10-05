# Redesign Spatial transformer network Localization Net


<img alt="GitHub License" src="https://img.shields.io/github/license/hankshyu/STN-Network?color=orange&logo=github"> <img alt="GitHub release (latest SemVer)" src="https://img.shields.io/github/v/release/hankshyu/STN-Network?color=orange&logo=github"> <img alt="GitHub language count" src="https://img.shields.io/github/languages/count/hankshyu/STN-Network"> <img alt="GitHub top language" src="https://img.shields.io/github/languages/top/hankshyu/STN-Network"> <img alt="GitHub code size in bytes" src="https://img.shields.io/github/languages/code-size/hankshyu/STN-Network"> <img alt="GitHub contributors" src="https://img.shields.io/github/contributors/hankshyu/STN-Network?logo=git&color=green"> <img alt="GitHub commit activity" src="https://img.shields.io/github/commit-activity/y/hankshyu/STN-Network?logo=git&color=green">  <img alt="GitHub last commit" src="https://img.shields.io/github/last-commit/hankshyu/STN-Network?logo=git&color=green">


***The full paper could be found [here](docs/Implementation%20of%20a%20RISC-V%20compatible%20Multiply-Add%20Fused%20Unit.pdf)***
## Abstract

Spatial transformer network (STN) is a powerful module that improves the spatial invariance
of convolutional neural networks. Amid all components, the localization net serves as the backbone
as it intakes the feature map and generates the affine transformation parameters, deciding how the
transformation to be operated. Meanwhile, the design of the localization net is closely correlated to
the outcome of transformation consequently affecting the system performance. This work simulates
through different localization net designs and examines them with several prominent models on
various datasets, and which aims to discover a modern localization network architecture that boosts
the performance of STN transformation with less parameters and computational overhead.

## 1.Intorduction

A series of high-performance convolutional neural networks have been recognized as the
state-of-the-art machine learning approaches since AlexNet [1] won the ImageNet Challenge:
ILSVRC 2012 [2]. Thistechnology has been successfully applied to address a broad range of machine
learning tasks, such as object detection, tracking [3, 4], super-resolution [5, 6] and document analysis
[7]. Although they were introduced roughly in the 90s [8], it was not until the development of
computational hardware along with the improvement of model architectures in this decade that
enabled a smooth training of deep neural networks. Meanwhile, the pioneering LeNet5 [9] introduced
a 5 layered structure in 1998, but nowadays deep neural networks (DNN) have reached a hundred
layers or more with different performances through more specialized design, such as Residual
Networks [10] or Highway Networks [11]. In addition, many advanced works focus on finer
interconnection methods [12], renewed convolution proceduresto reduce the computational overhead
[13, 14] or specialized layers targeting a subset of training problems. These approaches increase the
model performance based on elaborated mechanisms to gather potential explainable information from
the feature map and learn more detail from them. However, how to effectively transform the input
data by the model itself for the purpose of attaining a better performance draws few concerns.
Moreover, applying the attention mechanism to the input image is one of popular studies in the deep
learning, figuring out where the model should put emphasis on; howbeit, it was not until recently that
the relevant works start to design the corresponding model performance. Specialized layers are
designed to transform the input image before the image was sent into the main convolutional layers.
However, some of the components in such layers are out of date, thereby could not reflect the
improvements in convolutional architectural by modern research works. Thus, this work aims to
design a renewed structure of such layers to improve its capabilities, and then evaluates such redesigned structure on various models to validate its improvements





