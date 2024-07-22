# DeepLab v3

## **Use case** : [Semantic Segmentation](../README.md)

# Model description

DeepLabv3 was specified in "Rethinking Atrous Convolution for Semantic Image Segmentation" paper by Google.
It is composed by a backbone (encoder) that can be a Mobilenet V2 (width parameter alpha) or a ResNet-50 or 101 for example followed by an ASPP (Atrous Spatial Pyramid Pooling) as described in the paper.

ASPP applies on encoder outputs several parallel dilated convolutions with various dilation rates. This technique helps capturing longer range context without increasing too much the number of parameters.
The multi-scale design of the ASPP has proved to be receptive at the same time to details and greater contextual information.

So far, we have only considered Mobilenet V2 encoder.

## Network information


| Network Information     | Value                                                          |
|-------------------------|----------------------------------------------------------------|
|  Framework              | TensorFlow Lite                                                |
|  Quantization           | int8                                                           |
|  Provenance             | https://www.tensorflow.org/lite/examples/segmentation/overview |
|  Paper                  | https://arxiv.org/pdf/1706.05587                               |

The models are quantized using tensorflow lite converter.


## Network inputs / outputs


For an image resolution of NxM and P classes

| Input Shape  | Description |
|--------------| ----------- |
| (1, N, M, 3) | Single NxM RGB image with UINT8 values between 0 and 255 |

| Output Shape  | Description                                      |
|---------------|--------------------------------------------------|
| (1, N, M, 21) | Per-class confidence for P=21 classes in FLOAT32 |


## Recommended platforms


| Platform | Supported | Recommended |
|----------|--------|-----------|
| STM32L0  |[]|[]|
| STM32L4  |[]|[]|
| STM32U5  |[]|[]|
| STM32H7  |[]|[]|
| STM32MP1 |[]|[]|
| STM32MP2 |[x]|[x]|


# Performances
## Training


To train the deeplab_v3 with backbone MobileNet v2 model with pretrained weights, from scratch or fine-tune it on your own dataset, you need to configure the [user_config.yaml](../../src/user_config.yaml) file following the
[tutorial](../../README.md) under the src section.

As an example, [deeplab_v3_mobilenetv2_05_16_512_fft.yaml](./ST_pretrainedmodel_public_dataset/pascal_voc_coco_2012/deeplab_v3_mobilenetv2_05_16_512_fft/deeplab_v3_mobilenetv2_05_16_512_fft_config.yaml) file is used to train on PASCAL VOC + COCO 2012 dataset. You can copy its content in the [user_config.yaml](../../src/user_config.yaml) file provided under 
the src section to reproduce the results presented below.

## Deployment

To deploy your trained model, you need to configure the same [user_config.yaml](../../src/user_config.yaml) file following the [tutorial](../../deployment/README_MPU.md).


## Metrics


Measures are done with default STM32Cube.AI configuration with enabled input / output allocated option.


### Reference **MPU** inference time based on PASCAL VOC + COCO 2012  segmentation dataset 21 classes (see Accuracy for details on dataset)
| Model                                                                                                                                                                                                          | Dataset     | Format | Resolution | Quantization   | Board             | Execution Engine | Frequency | Inference time (ms) | %NPU  | %GPU   | %CPU | X-LINUX-AI version |       Framework       |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|--------|------------|----------------|-------------------|------------------|-----------|---------------------|-------|--------|------|--------------------|-----------------------|
| [DeepLabV3 per tensor (no ASPP)](https://www.st.com/en/embedded-software/x-linux-ai.html)                                                                                                                       | PASCAL VOC + COCO 2012  | Int8   | 257x257x3  | per-tensor     | STM32MP257F-DK2   | NPU/GPU          | 1500  MHz | 52.75           | 99.2 | 0.80  | 0 | v5.1.0             | OpenVX                |                |       |        |      | v5.1.0 
| [DeepLabV3 per channel](./ST_pretrainedmodel_public_dataset/pascal_voc_coco_2012/deeplab_v3_mobilenetv2_05_16_512_fft/deeplab_v3_mobilenetv2_05_16_512_fft_int8.tflite) | PASCAL VOC + COCO 2012  | Int8   | 512x512x3  | per-channel ** | STM32MP257F-DK2   | NPU/GPU          | 1500  MHz | 806.12            | 8.73| 91.27 | 0   | v5.1.0             | OpenVX                |
| [DeepLabV3 mixed precision](./ST_pretrainedmodel_public_dataset/pascal_voc_coco_2012/deeplab_v3_mobilenetv2_05_16_512_fft/deeplab_v3_mobilenetv2_05_16_512_fft_int8_f32.tflite) | PASCAL VOC + COCO 2012  | Int8 & float32  | 512x512x3  | per-channel ** | STM32MP257F-DK2   | NPU/GPU          | 1500  MHz |  894.56  | 7.67 | 92.33 | 0  | v5.1.0             | OpenVX                |

** **To get the most out of MP25 NPU hardware acceleration, please use per-tensor quantization**

### Accuracy with PASCAL VOC + COCO 2012 

Dataset details: [link](http://host.robots.ox.ac.uk/pascal/VOC/voc2012/), License [Database Contents License (DbCL) v1.0](https://opendatacommons.org/licenses/dbcl/1-0/) , Number of classes: 21, Number of images: 11530
Please note, that the following accuracies are evaluated on Pascal VOC 2012 validation set (val.txt), and with a preprocessing resize with interpolation method 'bilinear'.
Moreover, IoU are averaged on all classes including background.

| Model Description                                                                                                                                            | Resolution | Format     | Accuracy | Averaged IoU |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------|------------|----------|--------------|
| [DeepLabV3 per tensor (no ASPP)](https://www.st.com/en/embedded-software/x-linux-ai.html)                                                            | 257x257x3  | Int8       | 88.6%    | 59.33%       |
| [DeepLabV3 float precision](./ST_pretrainedmodel_public_dataset/pascal_voc_coco_2012/deeplab_v3_mobilenetv2_05_16_512_fft/deeplab_v3_mobilenetv2_05_16_512_fft.h5) | 512x512x3  | Float      | 93.29%   | 73.44%       |
| [DeepLabV3 per channel](./ST_pretrainedmodel_public_dataset/pascal_voc_coco_2012/deeplab_v3_mobilenetv2_05_16_512_fft/deeplab_v3_mobilenetv2_05_16_512_fft_int8.tflite) | 512x512x3  | Int8       | 91.3%    | 67.32%       |
| [DeepLabV3 mixed precision](./ST_pretrainedmodel_public_dataset/pascal_voc_coco_2012/deeplab_v3_mobilenetv2_05_16_512_fft/deeplab_v3_mobilenetv2_05_16_512_fft_int8_f32.tflite) | 512x512x3  | Int8/Float | 92.83%   | 71.93%       |

## Retraining and code generation

- **DeepLabV3 per tensor**:
   This model, which does not include ASPP (Atrous Spatial Pyramid Pooling), was downloaded from the TensorFlow DeepLabV3 page on[Kaggle](https://www.kaggle.com/models/tensorflow/deeplabv3/).

- **DeepLabV3 float precision**:
   This model is the result of using the [deeplab_v3_mobilenetv2_05_16_512_fft.yaml](./ST_pretrainedmodel_public_dataset/pascal_voc_coco_2012/deeplab_v3_mobilenetv2_05_16_512_fft/deeplab_v3_mobilenetv2_05_16_512_fft_config.yaml) configuration file to train the model on the PASCAL VOC + COCO 2012 dataset.

- **DeepLabV3 Per channel**:
   This model is quantized `per channel` version of DeepLabV3 float precision. It is generated using the quantization service with the [the quantization_config.yaml](../../src/config_file_examples/quantization_config.yaml) configuration file.


- **DeepLabV3 mixed precision**:
   This model is a mixed precision version of the DeepLabV3 float precision. The backbone is fully quantized to 8 bits, while the ASPP head remains partially in float precision. Some layers were too sensitive to 8-bit quantization, resulting in unacceptable accuracy degradation. Therefore, we instructed TFLite not to quantize those specific layers.

## Demos
### Integration in a simple example


Please refer to the generic guideline [here](../../deployment/README_MPU.md)




 