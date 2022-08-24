# Redesign Spatial transformer network Localization Net


<img alt="GitHub License" src="https://img.shields.io/github/license/hankshyu/STN-Network?color=orange&logo=github"> <img alt="GitHub release (latest SemVer)" src="https://img.shields.io/github/v/release/hankshyu/STN-Network?color=orange&logo=github"> <img alt="GitHub language count" src="https://img.shields.io/github/languages/count/hankshyu/STN-Network"> <img alt="GitHub top language" src="https://img.shields.io/github/languages/top/hankshyu/STN-Network"> <img alt="GitHub code size in bytes" src="https://img.shields.io/github/languages/code-size/hankshyu/STN-Network"> <img alt="GitHub contributors" src="https://img.shields.io/github/contributors/hankshyu/STN-Network?logo=git&color=green"> <img alt="GitHub commit activity" src="https://img.shields.io/github/commit-activity/y/hankshyu/STN-Network?logo=git&color=green">  <img alt="GitHub last commit" src="https://img.shields.io/github/last-commit/hankshyu/STN-Network?logo=git&color=green">

Spatial transformer network is a powerful module that improves the spatial invariance of convolutional neural networks. Amid all components, the localization net serves as the backbone as it intakes the feature map and yields the affine transformation parameters, deciding how the transformation to be executed. The design of the localization net is closely correlated to the outcome of transformation consequently influence accuracy. This work experiments through different localization net designs and test them with several prominent models on various datasets. Aiming to discover a modern localization network architecture that boosts the performance of STN transformation with less parameters and computational 
overhead.

## Localization Net architecture

### 1.Regular Structure(STN)
Most commonly used spatial transform network localization architecture, which resembles the **LeNet5 architecture**

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

Below show the result of each module testing with 4 different setups on the Cifar-10 dataset. Each module were tested with no STN setup (called Control group), STN, STN1 and STN2. The expermintent leads to a result as below:

<table>
  <tr>
    <th>Model</th>
    <th colspan="2">Control group </th>
    <th colspan="2">STN </th>
    <th colspan="2">STN 1</th>
    <th colspan="2">STN 2</th>
  </tr>
  <tr>
    <td></td>
    <td>Top 5 val.</td>
    <td>Top 5 test</td>
	<td>Top 5 val.</td>
    <td>Top 5 test</td>
	<td>Top 5 val.</td>
    <td>Top 5 test</td>
	<td>Top 5 val.</td>
    <td>Top 5 test</td>
  </tr>
  <tr>
	<td>EfficientNet-b0</td>
	<td>86.83</td>	
	<td>86.05</td>	
	<td>87.48</td>	
	<td>86.40</td>	
	<td>87.88</td>	
	<td>86.81</td>	
	<td>87.71</td>	
	<td>87.45</td>
  </tr>
  <tr>
	<td>EfficientNet-b1</td>
	<td>86.90</td>
	<td>86.89</td>
	<td>87.01</td>
	<td>86.95</td>
	<td>87.62</td>
	<td>86.98</td>
	<td>87.71</td>
	<td>87.03</td>
  </tr>
  <tr>
  	<td>EfficientNet-b2</td>
	<td>86.65</td>
	<td>86.38</td>
	<td>87.81</td>
	<td>87.30</td>
	<td>87.57</td>
	<td>87.36</td>
	<td>87.77</td>
	<td>87.52</td>
  </tr>
  <tr>
  	<td>EfficientNet-b3</td>
  	<td>87.11</td>
  	<td>86.79</td>
  	<td>87.12</td>
  	<td>86.96</td>
  	<td>87.19</td>
  	<td>87.02</td>
  	<td>87.64</td>
  	<td>87.22</td>
  </tr>
  <tr>
  	<td>ResNet-18</td>
  	<td>85.60</td>
  	<td>84.96</td>
  	<td>85.44</td>
  	<td>85.53</td>
  	<td>85.79</td>
  	<td>85.83</td>
  	<td>86.37</td>
  	<td>86.41</td>
  </tr>
  <tr>
  	<td>ResNet-50</td>
  	<td>85.07</td>
  	<td>84.84</td>
  	<td>85.43</td>
  	<td>85.01</td>
  	<td>85.63</td>
  	<td>85.02</td>
  	<td>86.69</td>
  	<td>86.19</td>
  </tr>
  <tr>
	<td>ResNet-152</td>
	<td>85.15</td>
	<td>84.65</td>
	<td>85.09</td>
	<td>84.81</td>
	<td>85.40</td>
	<td>85.16</td>
	<td>86.86</td>
	<td>85.17</td>
  </tr>
  <tr>
  	<td>DenseNet-121</td>
  	<td>88.91</td>
  	<td>88.61</td>
  	<td>89.42</td>
  	<td>89.29</td>
  	<td>89.88</td>
  	<td>89.51</td>
  	<td>86.17</td>
  	<td>90.28</td>
  </tr>
</table>






