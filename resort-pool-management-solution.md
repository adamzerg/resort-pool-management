# Resort Pool Management Service

## Background

In resort settings, a common issue arises where guests reserve pool chairs early in the morning, leaving others unable to use them for the rest of the day. To address this, we propose an optimization solution for the iconic pool area, ensuring equitable access to poolside amenities for all guests.

## Scope of Works

Create an AI-driven solution for real-time monitoring of pool lounger usage. This service will empower resort staff to swiftly determine chair occupancy status, ensuring guest satisfaction and efficient resource allocation.

## Concept Abstract

As part of the standard poolside concierge service, a pool attendant assists guests by placing a fresh towel on each lounge chair. Therefore, a sun lounger that is covered with a towel can be considered "in service" and occupied by a guest.  
When a guest has finished using the lounger, the resort staff needs to be notified in a timely manner. At that point, the staff can clean up any towels, water bottles, or other items left behind by the departing guest.  

![Decision Flow](./flow-design/decision-flow.png)

To determine when a lounger is ready for cleanup, the system can analyze poolside CCTV footage captured by cameras in real-time. If the footage shows loungers with towels and water bottles present, but no people or personal belongings are visible, the system can flag those loungers as "awaiting cleanup."
If the "awaiting cleanup" status persists, the system should notify resort staff. This will ensure timely cleanup of vacated loungers and keep the poolside area tidy and ready for the next guests.

![Awaiting Cleanup](./inference-sample/inference-normal-2.jpg)

^ No personal belongings present, therefore guests may has left. Used towel and drinks are left behind. This should be categorized as a **awaiting cleanup** status.

![Normal Status](./inference-sample/inference-normal.jpg)

^ Guests may still be present, as glasses and slippers are visible. Therefore, this should be categorized as a **normal** status.

## Object Detection

![Train Object Detection](./flow-design/train-object-detection.svg)

At this stage, object detection solutions is built with Roboflow's cloud-based platform efficiently, easy focusing on innovation rather than infrastructure management.

### 1.1 Data source from real life photos

![dataset-1](./flow-design/dataset-1.png)

### 1.2 Data source from AI generative images

![dataset-3](./flow-design/dataset-3.png)

### 2. Annotation

Setting of 12 classes for detection of essential objects:  
> person, towel, water-bottle  
> lifeguard-chair, lounger, lounger-serving, lounger-water-drop  
> belongings-cloth, belongings-glasses, belongings-slippers, belongings-swimming-ring, belongings-wristlet  

### 3. Dataset Versioning

<img src="./flow-design/dataset-all.png" alt="dataset-all" width="900"/>

After resizing to 640x640 and augmenting with flipping and rotating, the number of images has increased to a total of 298, ready for training

### 4. Training Processing

<img src="https://learnopencv.com/wp-content/uploads/2023/05/yolo-nas_results_roboflow_100_comparison.png" alt="YOLO-NAS" width="900"/>

Object detection model chosen is YOLO-NAS S with pre-train benchmark model: MS COCO v14-Best (Common Objects, 52.2% mAP)  

<img src="./flow-design/training-graphs.png" alt="training-graphs" width="900"/>

20-45 minutes in each training, by the last training we have obtained a model with resulting mAP 71.1%, accuracy being enhanced meanwhile there appears no overfitting

## Inference Workflow

<video width="960" height="540" controls>
    <source src="https://storage.googleapis.com/bucket-trial-run/image-inference.mp4" type="video/mp4">
</video>

A simplified inference pipeline allows various components to be customized and run in parallel,  
including object detection, bounding box generation, labeling, and image cropping.  
This setup enables real-time visualization of inference results.

## Inferencing video footage (Proof of Concept)

<video width="960" height="540" controls>
<source src="https://storage.googleapis.com/bucket-trial-run/video-inference-2.mp4" type="video/mp4">
</video>

^ There is a crowded area with guests and some personal belongings present.  
Several lounger seats are available for use. There are few towels and water bottles visible.  
Given these observations, the status should be categorized as **normal**.

Video inferencing sample code can be found here:
[sample-inference-roboflow.ipynb](https://github.com/adamzerg/resort-pool-management/blob/main/inference-roboflow.ipynb)

## Generative Image

Generative AI models are employed to synthetically create diverse training images for object detection models. These generated images cover a wide range of scenarios, such as varying times of day, occupancy levels, and guest profiles. This approach helps to robustly train the object detection model without compromising privacy, as the generated images are not based on real-world data that could potentially reveal sensitive information.  

By augmenting the training dataset with these synthetically generated images, the object detection model is exposed to a more comprehensive set of scenarios. This increased diversity in the training data leads to improved model performance and generalization, as the model learns to recognize objects in a broader range of conditions.  
The use of generative AI for data augmentation helps to increase the mAP (mean Average Precision) of the object detection model. By training the model on a larger and more diverse dataset, including synthetically generated images, the model becomes better equipped to accurately detect objects in various situations, resulting in a higher mAP score.

### Compfy runs Flux.1

<img src="./flow-design/comfy-flux-flow.png" alt="comfy-flux-flow" width="900"/>

### Configurations

Model: flux1-dev-fp8.safetensors  
Clip: t5xxl_fp16.safetensors, clip_l.safetensors  
VAE: ae.safetensors  
KSampler: euler  
Sample rompt: CCTV footage from poolside cameras on a summer rainy noon. Comfortable lounge chairs set next to the pool. Some lounger are neatly covered with fresh white towels, ready for guests, some personal belongings are placed on a small table next to the lounger, such as water bottles, sunglasses, also a pair of slippers are left on the floor. Lush palm trees and other tropical foliage frame the scene in the background. The pool water is crystal clear and inviting.

Sample json can be found here:
[flux1-workflow](https://github.com/adamzerg/resort-pool-management/blob/main/flux1-workflow.json)

### Alternative SD3 and SDXL

Initially, attempts to run SD3 and SDXL with the refiner (img2img) yielded good results.  
However, Flux.1 excels in generating high-quality images with superior detail and offers greater output diversity, producing a wider range of creative interpretations from the same prompt compared to SD3 and SDXL.

## Solution Architect