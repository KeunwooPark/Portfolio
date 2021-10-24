---
layout: post

title: "OddEyeCam: A Sensing Technique for Body-Centric Peephole Interaction using WFoV RGB and NFoV Depth Cameras"
permalink: /oddeyecam/
thumbnail: /assets/img/oddeyecam/thumbnail.jpg
publish_date:  2020-10-21
venue: UIST 2020
authors: "Daehwa Kim, **Keunwoo Park**, Geehyuk Lee"
to_appear: no
award:
video_embed_link : "https://www.youtube.com/embed/R56iuEuZyo0"
tags:
categories: conference
---
# Abstract
The space around the body not only expands the interaction space of a mobile device beyond its small screen, but also enables users to utilize their kinesthetic sense. Therefore, body-centric peephole interaction has gained considerable attention. To support its practical implementation, we propose OddEyeCam, which is a vision-based method that tracks the 3D location of a mobile device in an absolute, wide, and continuous manner with respect to the body of a user in both static and mobile environments. OddEyeCam tracks the body of a user using a wide-view RGB camera and obtains precise depth information using a narrow-view depth camera from a smartphone close to the body. We quantitatively evaluated OddEyeCam through an accuracy test and two user studies. The accuracy test showed the average tracking accuracy of OddEyeCam was 4.17 and 4.47cm in 3D space when a participant is standing and walking, respectively. In the first user study, we implemented various interaction scenarios and observed that OddEyeCam was well received by the participants. In the second user study, we observed that the peephole target acquisition task performed using our system followed Fitts’ law. We also analyzed the performance of OddEyeCam using the obtained measurements and observed that the participants completed the tasks with sufficient speed and accuracy.

# Technical Concept
OddEyeCam estimates the 3D location of a smartphone with respect to the body The process is

1. A WFoV (Wide Field of View) RGB camera provides a whole-body image.

2. An RGB-D camera provides partial body depth and RGB image.

3. An distortion-alleviation module reduces the distortion of the fisheye image through an equirectangular projection. A
body-tracking algorithm provides body keypoints from the undistorted image.

4. The body keypoints found in the WFoV image can be projected to the NFoV (Near Field of View) depth image by combining two cameras.

5. Using keypoints and depth information, as well as additional gravity vector from an accelerometer, we can estimate the body coordinate system.

6. We can obtain the device location with respect to the body by converting the body position (obtained in Step 5) with respect to the camera.

![pipeline](/assets/img/oddeyecam/pipeline.png)

# Hardware Prototype
We used a 180° fsheyeeye lens USB camera as the wide field of view RGB camera. Intel RealSense D415 was chosen as the narrow field of view depth camera in our prototype.

![hardware](/assets/img/oddeyecam/hardware.png)

# Example Applications
We created several applications to show various design possibilities of OddEyeCam.
![applications](/assets/img/oddeyecam/applications.png)

- A. Drag and Drop Between Apps
- B. Body-Centric Folder
- C. Large-Image Viewer
- D. One-Hand Tagging
- E. Getting Directions
- F. Marking Menu

**For more detail, please see our paper.**

# Resources

- [paper](https://dl.acm.org/doi/10.1145/3379337.3415889)
- [short talk](https://youtu.be/rsiCohRoFYI)
- [long talk](https://youtu.be/u6xvk0dM8fo)
- [code](https://github.com/KAIST-HCIL/OddEyeCam)

# My Contributions

I participated in
- project ideation
- accuracy test design and its software implementation