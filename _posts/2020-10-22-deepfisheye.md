---
layout: post

title: "DeepFisheye: Near-Surface Multi-Finger Tracking Technology Using Fisheye Camera"
permalink: /deepfisheye/
thumbnail: /assets/img/deepfisheye/thumbnail.png
publish_date: 2020-10-22
venue: UIST 2020
authors: "**Keunwoo Park**, Sunbum Kim, Youngwoo Yoon, Tae-Kyun Kim, Geehyuk Lee"
to_appear: no
award:
video_embed_link: "https://www.youtube.com/embed/Vds_rvnU6j8"
tags:
categories: conference
---

# Abstract

Near-surface multi-finger tracking (NMFT) technology expands the input space of touchscreens by enabling novel interactions such as mid-air and finger-aware interactions. We present DeepFisheye, a practical NMFT solution for mobile devices, that utilizes a fisheye camera attached at the bottom of a touchscreen. DeepFisheye acquires the image of an interacting hand positioned above the touchscreen using the camera and employs deep learning to estimate the 3D position of each fingertip. We created two new hand pose datasets comprising fisheye images, on which our network was trained. We evaluated DeepFisheye’s performance for three device sizes. DeepFisheye showed average errors with approximate value of 20 mm for fingertip tracking across the different device sizes. Additionally, we created simple rule-based classifiers that estimate the contact finger and hand posture from DeepFisheye’s output. The contact finger and hand posture classifiers showed accuracy of approximately 83 and 90%, respectively, across the device sizes.

# Technical Concept

![prototype](/assets/img/deepfisheye/technical_concept.png)

DeepFisheye pipeline first preprocesses a fisheye image, and then estimates the 3D locations of all hand joints with DeepFisheye network (DeepFisheyeNet). Then, two rule-based classifiers classify contact fingers and hand postures.

DeepFisheyeNet comprises two sub-networks: Pix2Depth (P2DNet) and hand pose estimator (HPENet) networks. P2DNet generates a depth image of the hand from the fisheye color image input. Next, hand parts of the color image are segmented using the generated depth image. Segmentation is a simple process that selects pixels from the color image only when the value of the corresponding pixel on the depth image is larger than 0. The depth image assists HPENet to achieve more accurate hand pose estimation. Lastly, HPENet estimates the final hand pose relative to the fisheye camera, using both color and generated depth images.

# DeepFisheye Hand Datasets

A large-scale dataset is essential for the learning-based hand pose estimation model. Although several hand datasets are available, we created our own because the existing ones comprise only flat images that follow the pinhole camera model. Unlike flat images, fisheye images are highly distorted, and the hands are occluded at some locations. This type of distortion and occlusion can lead to poor performance if we input the fisheye images into a hand pose estimator trained on flat images.

Creating a dataset is expensive because it requires collecting real images in diverse environments and annotating accurate 3D positions of the hand joints. In fact, other studies have created hand datasets synthetically with virtual hand models for cost effectiveness. However, real and synthetic images differ considerably in several aspects, e.g., texture, lighting, shadows. Owing to this _domain gap,_ if a deep learning model is trained only with synthetic images, it may not perform satisfactorily with real images.

Therefore, we created two datasets. The first, **DeepFisheye synthetic dataset**, comprises pairs of hand images and 3D joint data. The second is the **DeepFisheye real dataset** that comprises pairs of real hand images and 3D joint data. The synthetic dataset was used for initial training of a network estimating 3D hand pose, and the real dataset for decreasing the domain gap by fine-tuning the network.

If you want to use our dataset, please go to [this repository](https://github.com/KeunwooPark/DeepFisheyeDataset).

**For more detail, please see our paper.**

# Resources

- [paper (ACM DL)](https://dl.acm.org/doi/10.1145/3379337.3415818)
- [paper (author version)](/assets/DeepFisheye_CR.pdf)
- [short talk](https://youtu.be/5gbf8Vztjzc)
- [long talk](https://youtu.be/8W3OpxIXpQg)
- [demo](https://youtu.be/thmtBKEazxw)
- [code](https://github.com/KAIST-HCIL/DeepFisheyeNet)
