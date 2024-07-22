# Overview of audio event detection STM32 model zoo

The STM32 model zoo includes several Tensorflow models for the audio event detection use case pre-trained on custom and public datasets.
Under each model directory, you can find the `ST_pretrainedmodel_public_dataset` directory, which contains different audio event detection models trained on various public datasets following the [training section](../src/training/README.md) in STM32 model zoo. 

## Audio event detection (AED) Models

The table below summarizes the performance of the models, as well as their memory footprints generated using STM32Cube.AI for deployment purposes.

A note on clip-level accuracy : In a traditional AED data processing pipeline, audio is converted to a spectral representation (in this model zoo, log-mel-spectrograms), which is then cut into patches. Each patch is fed to the inference network, and a label vector is output for each patch. The labels on these patches are then aggregated based on which clip the patch belongs to, to form a single aggregate label vector for each clip. Accuracy is then computed on these aggregate label vectors.

The reason this metric is used instead of patch-level accuracy is because patch-level accuracy varies immensely depending on the specific manner used to cut spectrogram into patches, and also because clip-level accuracy is the metric most often reported in research papers.

**NOTE** : Yamnet can only be used with transfer learning or fine tuning, as it is simply a MobileNet with pretrained weights, not using the pretrained weights wouldn't make it much of a Yamnet anymore

By default, the results are provided for quantized Int8 models.

Below sections contain detailed information on models memory usage and accuracies (click on the arrows to expand):
<details><summary>Miniresnet v1</summary>

| Models                     | Input shape | Implementation | Dataset    | Clip-level Accuracy (%)   | MACCs    (M) | Activation RAM (KiB) | Weights Flash (KiB) | STM32Cube.AI version  | Source
|---------------------------|--------------|-----------------|------------|----------------------|-------------|-----------------------|----------------------|-----------------------|--------
| Miniresnet  1 stack | 64x50 | TensorFlow     | ESC-10    | 89.9%                |   7.489        |   59.89            |   123.6        | 9.1.0                 |    [link](miniresnet/ST_pretrainedmodel_public_dataset/esc10/miniresnet_1stacks_64x50_tl/miniresnet_1stacks_64x50_tl_int8.tflite)
| Miniresnet  2 stacks 64x50 | 64x50x1 | TensorFlow     | ESC-10    | 93.6%                |   12.721        |   59.989            |   431.1        | 9.1.0                 |    [link](miniresnet/ST_pretrainedmodel_public_dataset/esc10/miniresnet_2stacks_64x50_tl/miniresnet_2stacks_64x50_tl_int8.tflite)

</details>
<details><summary>Miniresnet 2 stacks</summary>

| Models                     | Input shape | Implementation | Dataset    | Clip-level Accuracy (%)   | MACCs    (M) | Activation RAM (KiB) | Weights Flash (KiB) | STM32Cube.AI version  | Source
|---------------------------|--------------|-----------------|------------|----------------------|-------------|-----------------------|----------------------|-----------------------|--------
| Miniresnetv2 1 stack 64x50 | 64x50x1 |  TensorFlow     | ESC-10    | 91.1%                |   15.034      |   59.89            |   123.98        | 9.1.0                |    [link](miniresnetv2/ST_pretrainedmodel_public_dataset/esc10/miniresnetv2_1stacks_64x50_tl/miniresnetv2_1stacks_64x50_tl_int8.tflite)
| Miniresnetv2 2 stacks 64x50 | 64x50x1 | TensorFlow     | ESC-10    | 93.6%                |   27.501        |   59.89            |   431.98        | 9.1.0                |    [link](miniresnetv2/ST_pretrainedmodel_public_dataset/esc10/miniresnetv2_2stacks_64x50_tl/miniresnetv2_2stacks_64x50_tl_int8.tflite)

</details>
<details><summary>Yamnet</summary>

| Models                     | Input shape | Implementation | Dataset    | Clip-level Accuracy (%)   | MACCs    (M) | Activation RAM (KiB) | Weights Flash (KiB) | STM32Cube.AI version  | Source
|---------------------------|--------------|-----------------|------------|----------------------|-------------|-----------------------|----------------------|-----------------------|--------
| Yamnet 256 on ESC-10| 64x96x1 | TensorFlow     | ESC-10    | 94.9%                |   23.932        |   109.57            |   135.91      | 9.1.0                 |    [link](yamnet/ST_pretrainedmodel_public_dataset/esc10/yamnet_256_64x96_tl/yamnet_256_64x96_tl_int8.tflite)
| Yamnet 256 on FSD50K without unknown class| 64x96x1 | TensorFlow     | FSD50K 5 classes   | 87.0%                |   23.931        |   109.57            |   134.64      | 9.1.0                 |    [link](yamnet/ST_pretrainedmodel_public_dataset/fsd50k/yamnet_256_64x96_tl/without_unknown_class/yamnet_256_64x96_tl_int8.tflite)
| Yamnet 256 on FSD50K with unknown class| 64x96x1 | TensorFlow     | FSD50K 5 classes    | 73.9%                |   23.931        |   109.57            |   134.9      | 9.1.0                 |    [link](yamnet/ST_pretrainedmodel_public_dataset/fsd50k/yamnet_256_64x96_tl/with_unknown_class/yamnet_256_64x96_tl_int8.tflite)

</details>

You can get inference time information for each models following links below:
- [Mini Resnet v1](./miniresnet/README.md)
- [Mini Resnet v2](./miniresnetv2/README.md)
- [Yamnet](./yamnet/README.md)

