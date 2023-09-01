---
title: SHrack&#58 Mobile Web Service for Real-time Exercise Posture Detection and
  Count Tracking
featured_image: https://vip2.loli.io/2023/09/02/yHXwQg1d5cDeNO9.png
start_date: '2023-03-19'
end_date: '2023-06-09'
last_modified_at: '2023-09-01 22:00:00'
---





![SHrack.png](https://vip2.loli.io/2023/09/02/AZ4Wa3YGPjgmbR8.png)



## Abstract

The fitness landscape is rapidly evolving with the integration of technology. We developed a new web-based service SHrack, designed to cater to fitness enthusiasts, providing real-time tracking and recording of home training exercises. By utilizing computer vision, SHrack enables users to independently track their fitness progress, setting goals and ensuring accurate count during repetitive exercises.

<br/>



## Introduction

![ShrackBackground.png](https://vip2.loli.io/2023/09/02/zqUysx7Ndep3T5i.png)

In the realm of fitness, especially during weight training, it's often challenging for individuals to keep an accurate count of their repetitions. While there are existing programs that measure exercise counts, finding a service that offers real-time video streaming for accurate exercise count detection and management is rare. SHrack addresses this gap, providing a platform that combines the power of computer vision with user-friendly web services.

<br/>



## Method

![SHrackML이미지5.png](https://vip2.loli.io/2023/09/02/yHXwQg1d5cDeNO9.png)

SHrack is developed to have an ability to harness computer vision techniques for real-time detection and tracking. The system employs the capabilities of Mobilenet and the Contextual Prediction Module (CPM) to extract heatmaps and PAF (Part Affinity Fields) based on 19 crucial points of the body.

Initially, the plan was to utilize pre-trained models. However, due to unsatisfactory estimation results, a decision was made to undergo supervised training with a labeled dataset, leading to fine-tuning of the model. The result is a robust posture estimation in videos, enabling accurate exercise count tracking.



![SHrack 프로젝트 구조.png](https://vip2.loli.io/2023/09/02/viBYjWt6OxzAaZm.png)

The visual data, which includes the user's exercise movements, is processed in real-time. The system analyzes the posture and movements, providing instant feedback regarding the accuracy of the exercise and the count.

<br/>



## Features & Implementation

![shrack simulation](https://vip2.loli.io/2023/09/02/mIlvJtgei4jNwBy.png)

SHrack stands out with its unique features:

<br/>

1. **Real-time Posture Detection**: By analyzing 19 crucial points on the body, SHrack offers real-time feedback on exercise posture, ensuring users maintain the correct form.
2. **Exercise Count Tracking**: The system accurately counts the repetitions, allowing users to focus on their exercise rather than keeping track.
3. **User-friendly Interface**: As a web-based tool, SHrack offers a seamless user experience, ensuring ease of access and use.

<br/>



## Conclusion

SHrack represents a significant step forward in the fusion of fitness and technology. By leveraging advanced computer vision techniques, it offers users an unparalleled experience in exercise tracking, ensuring accuracy, and promoting better exercise habits.

<br/>



## Miscellaneous



For further details, please refer to the official [Wiki](http://cscp2.sogang.ac.kr/CSE4186/index.php/%EB%B2%8C%EA%BF%80%EC%98%A4%EC%86%8C%EB%A6%AC#.EB.A9.98.ED.86.A0.EB.A7.81).
