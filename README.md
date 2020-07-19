# Portrait Segmentation on CelebAMask-HQ
Challenge submission for fellowship.ai 

## Image Segmentation
Apply an automatic portrait segmentation model (aka image matting) to [celebrity face](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html) dataset.


---


## Introduction

Portrait segmentation can be considered a specialised case of semantic segmentation with only two classes (subject and backgorund). As such we will explore two semantic segmentation architectures, namely, SegNet and UNet.

[CelebAMask-HQ](https://github.com/switchablenorms/CelebAMask-HQ) is a fork of CelebA dataset with annotations which is used for training the following models. (No manual annotations, just a little preprocessing. Whew!!)

All training was done in Google Colab

---


## SegNet Approach

SegNet is a symmetrical architecture that adopts an encoder-decoder framework. The encoder and decoder are symmetrical to each other. The upsampling operation of the decoder layers uses the max-pooling indices of the corresponding encoder layers. SegNet does not have any skip connections.

![SegNet Architecture](https://github.com/Vishaal-MK/Portrait-Segmentation-on-CelebA/blob/master/Images/SegNet%20Architecture.png)

The model resulted in 33 million trainanble parameters (maybe overkill?). We skip data augmentation since we have 30,000 images to train the models on.

### Results

| Epochs | Time Taken | Loss   | IOU    | Dice Coefficient | Accuracy |
|--------|------------|--------|--------|------------------|----------|
| 1      | 13 mins    | 31.18% | 99.77% | 82.34%           | 92.84%   |
| 20     | 5 hours    | 3.52%  | 99.98% | 98.34%           | 98.48%   |

### Preview

![SegNet Output 1](https://github.com/Vishaal-MK/Portrait-Segmentation-on-CelebA/blob/master/Images/SegNet%201.png)

![SegNet Output 2](https://github.com/Vishaal-MK/Portrait-Segmentation-on-CelebA/blob/master/Images/SegNet%202.png)

### Conclusion

Most of the images are predominantly foregorund. This explains the high IOU or Jaccard index scores. Although we have high accuracy and Dice Coefficients, the predictions are not very sharp. Simplifying architecture might work.


---


## UNet Approach

The UNet architecture was proposed for biomedical image segmentation where size of training data is limited. It is symmetric and has an encoder-decoder structure like SegNet but also includes skip connections between corresponding layers for better localisation. This could improve the sharpness of the predictions.

![UNet Architecture](https://github.com/Vishaal-MK/Portrait-Segmentation-on-CelebA/blob/master/Images/UNet%20Architecture.png)

We're using a simplified architecture with one less layer in encoder and decoder sections. Just 132,000 parameters.

### Results

| Epochs | Time Taken | Loss   | IOU    | Dice Coefficient | Accuracy |
|--------|------------|--------|--------|------------------|----------|
| 1      | 10 mins    | 9.50%  | 99.94% | 95.84%           | 95.79%   |
| 24     | 4 hours    | 8.00%  | 99.95% | 96.79%           | 96.87%   |

### Preview

![UNet Output 1](https://github.com/Vishaal-MK/Portrait-Segmentation-on-CelebA/blob/master/Images/UNet%201.png)

![UNet Output 2](https://github.com/Vishaal-MK/Portrait-Segmentation-on-CelebA/blob/master/Images/UNet%202.png)

### Conclusion

The model performs as advertised with very good performance in just one epoch of training. The numbers are nudged a little higher with more training. The predcitions look sharp after enhancement.

---

## Test Image Results

![Test 1](https://github.com/Vishaal-MK/Portrait-Segmentation-on-CelebA/blob/master/Images/Result1.png)

![Test 2](https://github.com/Vishaal-MK/Portrait-Segmentation-on-CelebA/blob/master/Images/Result2.png)

---

## Inference

The UNet model works well on images with simple backgrounds and is able to segment parts of most portraits like accessories, hair and clothing. It starts to struggle when posed with complicated backgrounds.

To improve model :
1. Train with images having higher background complexity to better segment portraits.
2. Train with bigger images (chose smaller images to reduce training time).
3. Add more layers akin to the model presented in the paper.

---

## References

[1]*SegNet* Vijay Badrinarayanan, Alex Kendall, Roberto Cipolla. [SegNet: A Deep Convolutional Encoder-Decoder Architecture for Image Segmentation](https://arxiv.org/pdf/1511.00561.pdf)

[2]*UNet* Olaf Ronneberger, Philipp Fischer, Thomas Brox. [U-Net: Convolutional Networks for Biomedical Image Segmentation](https://arxiv.org/pdf/1505.04597.pdf)
