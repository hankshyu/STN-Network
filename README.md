# Redesign Spatial transformer network Localization Net

Spatial transformer network is a powerful module that improves the spatial invariance of convolutional neural networks. Amid all components, the localization net serves as the backbone as it intakes the feature map and yields the affine transformation parameters, deciding how the transformation to be executed. The design of the localization net is closely correlated to the outcome of transformation consequently influence accuracy. This work experiments through different localization net designs and test them with several prominent models on various datasets. Aiming to discover a modern localization network architecture that boosts the performance of STN transformation with less parameters and computational 
overhead.

## Localization Net architecture

### 1.Regular Structure(STN)
Most commonly used spatial transform network localization architecture, which **resembles the LeNet5 architecture**.

|Type| Filter Shape  | Output shape | Parameters|
|:----|:----:|:---:|:---:|
| Conv  | 7 x 7 x 3 x 8 |218 x 218 x 8|1184
| Max pool, stride 2  | 2 x 2 x 8 x 8	|109 x 109 x 8|0
|Conv |5 x 5 x 8 x 10	|105 x 105 x 10	|2010
|Max pool, stride 2|2 x 2 x 10 x 10	|52 x 52 x 10	|0
|Total| | |3194|

### 2.VGG-like Structure(STN1)
**Breaking down large size convolution filers** into stacked small size convolutions is a signigicant change made by the VGG Net. Similar changes could also be made to the STN localization architecture, by replacing the first 7 x 7 wide convolution layer into three stacked 3 x 3 convolution layers and the second 5 x 5 wide convolution would turn into two 3 x 3 convolution layers at the meantime.

|Type| Filter Shape  | Output shape | Parameters|
|:----|:----:|:---:|:---:|
Conv|	3 x 3 x 3 x 8	|222 x 222 x 8	|224
Conv|	3 x 3 x 8 x 8	|220 x 220 x 8	|584
Conv|	3 x 3 x 8 x 8	|218 x 218 x 8	|584
Max pool, stride 2	|2 x 2 x 8 x 8	|109 x 109 x 8	|0
Conv|	3 x 3 x 8 x 10	|107 x 107 x 10|	730
Conv|	3 x 3 x 10 x 10	|105 x 105 x 10|	910
Max pool, stride 2	|2 x 2 x 10 x 10	|52 x 52 x 10	|0
Total|	|	|3032


### 3.The Proposed Structure(STN2)

The proposed structure utilize **1 x 1 kernels between channels** to increase or decrease the feature map count and
**Batch Normalization** to reduces the covariate shift so fewer exploding gradients would happen.

|Type| Filter Shape  | Output shape | Parameters|
|:----|:----:|:---:|:---:|
Conv, stride 2 + BN	|3 x 3 x 3 x 4	|112 x 112 x 4	|112+8
Conv + BN	|1 x 1 x 4 x 8	|112 x 112 x 8	|40+16
Conv + BN	|3 x 3 x 8 x 8	|110 x 110 x 8	|584+16
Conv + BN	|1 x 1 x 8 x 4	|110 x 110 x 4	|36+16
Conv + BN	|1 x 1 x 4 x 8	|110 x 110 x 8	|40+16
Conv + BN	|3 x 3 x 8 x 8	|108 x 108 x 8	|584+16
Conv + BN	|1 x 1 x 8 x 4	|108 x 108 x 4	|36+16
Conv + BN	|1 x 1 x 4 x 8	|108 x 108 x 8	|40 + 16
Conv, stride 2 + BN	|3 x 3 x 8 x 8	|54 x 54 x 8	|584+16
Conv + BN	|1 x 1 x 8 x 4	|54 x 54 x 4	|36+8
Conv + BN	|1 x 1 x 4 x 8	|54 x 54 x 8	|40 + 16
Conv, padding 1	|3 x 3 x 8 x 8	|54 x 54 x 8	|584+16
Conv + BN	|1 x 1 x 8 x 4	|54 x 54 x 4	|36+8
Conv + BN	|1 x 1 x 4 x 8	|54 x 54 x 8	|40 + 16
Conv + BN	|3 x 3 x 8 x 8	|52 x 52 x 8	|584+16
Conv + BN	|1 x 1 x 8 x 4	|52 x 52 x 4	|36+8
Conv + BN	|1 x 1 x 4 x 10	|52 x 52 x10	|50+20
Total|	|	|3690

## Experiment Result

### 1. Cifar-10

Model	Original	STN	STN 1	The proposed structure - STN 2
	Top-5 val.	Top-5 test	Top-5 
val.	Top-5 
test	Top-5 
val.	Top-5
test	Top-5 
val.	Top-5 
test
EfficientNet-b0	73.33	72.33	75.67	73.92	71.83	74.25	74.00	74.83
EfficientNet-b1	72.83	71.58	73.33	71.92	71.42	72.25	72.00	73.00
EfficientNet-b2	66.83	66.83	68.50	68.42	69.58	68.50	73.17	75.00
EfficientNet-b3	63.33	60.83	69.83	69.83	74.00	73.67	75.58	75.42
EfficientNet-b4	68.67	69.25	69.42	71.83	73.25	72.08	73.92	74.83
EfficientNet-b5	70.00	68.75	72.75	71.83	73.25	72.08	79.83	78.33
ResNet-50	75.17	73.58	76.42	76.92	78.75	77.00	77.42	77.33







