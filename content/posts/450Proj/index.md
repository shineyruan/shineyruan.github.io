---
title: Real-Time On-Device Flow Statistics Detection and Prediction
date: 2020-08-29T17:40:39-04:00
tags: ["computer vision", "raspberry pi", "Multi-lingual"]
categories: ["Projects"]

featuredImage: /images/posts/450Proj/450_MorphologicalAnalysis.png

hiddenFromHomePage: false
hiddenFromSearch: false
twemoji: false
lightgallery: false
ruby: false
fraction: false
fontawesome: true
linkToMarkdown: true
rssFullText: false

toc:
  enable: true
  auto: true
code:
  copy: true
  maxShownLines: 50
math:
  enable: true
  # ...
mapbox:
  # ...
share:
  enable: true
  # ...
comment:
  enable: false
  # ...
library:
  css:
    # someCSS = "some.css"
    # located in "assets/"
    # Or
    # someCSS = "https://cdn.example.com/some.css"
  js:
    # someJS = "some.js"
    # located in "assets/"
    # Or
    # someJS = "https://cdn.example.com/some.js"
seo:
  images: []
  # ...
---

***This project was served as the Undergraduate Major Design Experience at UM-SJTU Joint Institute, Shanghai Jiao Tong University.***

{{<youtube kMHzTvTi0Lc>}}

<!-- more -->

## Background: The Impact of COVID-19

The outbreak of novel coronavirus COVID-19 in 2020 has brought huge attention to the administration of human society. "Social distancing" has been advocated as a very effective method to fight the virus. In order to facilitate the implementation of social distancing, we propose an idea of building a portable device that is capable of *detect and predict flow statistics in real time*, in order to provide insights and suggestions for traffic management.

With the development of computer vision technology, nowadays it has become much easier and cheaper to put cameras into all parts of industry to perform a variety of detection tasks. In order to make our product cost-efficient, we also decided to take the advantage of computer vision and developed a *vision-based* solution for real-time flow detection. To ensure that our product is portable to use, we also settled down the plan that our whole system should be running on a Raspberry Pi.

## Concept Generation

{{<figure src="450_MorphologicalAnalysis.png" caption="Morphological chart." width="500">}}

What constitutes to our product? First, in order to satisfy the basic requirements, our final product must be capable of detecting human traffic flow (i.e. pedestrians) through cameras. Therefore, there must be some part transforming the live video stream into detection results. As a result, we name this part an **object detector.** Second, we need to make sure that our system is capable of assigning "identities" to objects in the camera frame so that it could detect which person leaves the frame and which person enters the frame and we call this part an **object tracker.** Third, we wish to store a fixed amount of history data so that we could utilize it to predict future flow statistics changes. This means that we also need a **back-end server.** Last but not least, we'll have to display our flow analysis results somewhere. We decided to host them on a self-designed **front-end website.**

From the above morphological analysis, we settled down the final configuration of our product: an object detector, an object tracker, a back-end server, and a front-end website.

## Key Technologies

After we settled down the big picture of our project, we then proceeded to conduct tests and develop the final product. After several months of discussions and developments, our final product have utilized the following technologies.

### Object Detector: MobileNet-SSD

Deep Neural Networks (DNNs) are the first solution we have thought of for object detection. It has multiple advantages over other techniques such as traditional computer vision methods, including better robustness and lower implementational cost.

However, not all DNNs are suitable for our project. In fact, most of the state-of-the-art DNN detectors require a lot of computing resources such as high-performance deep learning graphics cards and are generally run on big enterprise servers. Therefore, we have to select the DNN with a relatively high performance and the least required computing resources.

Finally, [MobileNet v2](https://arxiv.org/abs/1801.04381)-based [Single Shot MultiBox Detector](https://link.springer.com/chapter/10.1007/978-3-319-46448-0_2) (MobileNet-SSD) becomes our final solution, because it is accurate, lightweight, and was specifically designed for mobile devices.

### Object Tracker: Kalman Filter

Kalman filter is a probabilistic model that has been widely used in control theories and applications. Particularly, it has its own *states* that are hidden from the outer world and only certain state properties can be *observed* by outer worlds through sensors. The main task of the model is to use a certain amount of observations to predict the next states of the model. Hence, we can utilize this property of Kalman filter and construct one Kalman filter for each object that we're going to track. Once the Kalman filter is constructed, we can utilize the prediction of its position in the next frame and cross-check with the detection results of the next frame to determine whether this object is "disappeared."

## Source Code

Unfortunately, due to confidentiality agreement, the source code for this project is not available for now. However, users can always visit our front-end website <https://www.umji-flowdetection.com/> for further reference.

## Acknowledgements

For the entire work, I would like to express special thanks to:

- our sponsors: Allen Zhu and Allan Zhu from UM-SJTU Joint Institute in Shanghai Jiao Tong University
- my teammates: Jiayu Yi, Zekun LI, Zhikai Chen and Zihao Shen
- my own passion!
