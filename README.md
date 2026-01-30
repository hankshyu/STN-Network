# Redesign STN Localization Net (L-STN)

<img alt="GitHub License" src="https://img.shields.io/github/license/hankshyu/STN-Network?color=orange&logo=github"> <img alt="GitHub release (latest SemVer)" src="https://img.shields.io/github/v/release/hankshyu/STN-Network?color=orange&logo=github"> <img alt="GitHub language count" src="https://img.shields.io/github/languages/count/hankshyu/STN-Network"> <img alt="GitHub top language" src="https://img.shields.io/github/languages/top/hankshyu/STN-Network"> <img alt="GitHub code size in bytes" src="https://img.shields.io/github/languages/code-size/hankshyu/STN-Network"> <img alt="GitHub contributors" src="https://img.shields.io/github/contributors/hankshyu/STN-Network?logo=git&color=green"> <img alt="GitHub commit activity" src="https://img.shields.io/github/commit-activity/y/hankshyu/STN-Network?logo=git&color=green">  <img alt="GitHub last commit" src="https://img.shields.io/github/last-commit/hankshyu/STN-Network?logo=git&color=green">

Implementation to our paper***[An Improved Spatial Transformer Network based on Lightweight Localization Net (L-STN)](docs/ISASD2024.pdf)***

## 📝 Abstract

Spatial transformer network (STN) is a powerful module that improves the spatial invariance
of convolutional neural networks. Amid all components, the localization net serves as the backbone
as it intakes the feature map and generates the affine transformation parameters, deciding how the
transformation to be operated. Meanwhile, the design of the localization net is closely correlated to
the outcome of transformation consequently affecting the system performance. This work simulates
through different localization net designs and examines them with several prominent models on
various datasets, and which aims to discover a modern localization network architecture that boosts
the performance of STN transformation with less parameters and computational overhead.

## 🧑‍🏫 Demo

