---
title: RECJOON&#58 A Personalized Algorithm Problem and Rival Recommendation System
  for Baekjoon Online Judge Users
featured_image: https://vip2.loli.io/2023/09/01/PpXAGQ8Y7nyuOtW.png
start_date: '2022-04-21'
end_date: '2022-06-18'
last_modified_at: '2023-09-01 21:00:00'
---





![RECJOON Logo](https://vip2.loli.io/2023/09/01/PpXAGQ8Y7nyuOtW.png)

<br/>

## Abstract

RECJOON is an AI-driven service designed to recommend algorithm problems and rivals for users of the [Baekjoon Online Judge (BOJ)](https://www.acmicpc.net/) and [solved.ac](https://solved.ac/) platforms. Utilizing deep learning and machine learning techniques, the service offers personalized suggestions based on each user's problem-solving history.

<br/>

## Background

![RECJOON Computers](https://cdn.jsdelivr.net/gh/Glanceyes/Image-Repository/2022/06/13/20220613_1655087579.png)

As the demand for algorithmic problem solving and competitive programming grows, there is a rising need for platforms that not only offer challenges but also provide guidance tailored to individual users. RECJOON addresses this need by analyzing the problem-solving records of BOJ and solved.ac users and offering recommendations suited to their proficiency level.

<br/>

## Method

The system harnesses the power of deep learning and machine learning to process and analyze vast amounts of user data. By studying the patterns and tendencies in each user's problem-solving history, RECJOON's AI models are trained to predict which problems or rivals would be most beneficial for a user's growth.



<br/>

### Batch Serving

<br/>

![model_pipeline](https://cdn.jsdelivr.net/gh/Glanceyes/Image-Repository/2022/06/14/20220614_1655175870.png)



In RECJOON, model training and prediction, along with data collection, are executed through batch serving at predefined intervals. Every recommendation service undergoes a validation process post-model training, where predefined metrics are measured. The model that showcases the best performance during each cycle is then chosen as the prediction model.

<br/>



### Models

#### Models for Algorithm Problems Recommendation

| Name                             | Reference                                                    |
| -------------------------------- | ------------------------------------------------------------ |
| RecVAE(Variational AutoEncoder)  | [Ilya Shenbin, Anton Alekseev, Elena Tutubalina, Valentin Malykh, and Sergy I. Nikolenko. 2019. RecVAE: A New Variational Autoencoder for Top-N Recommendations with Implicit Feedback. ACM](https://arxiv.org/abs/1912.11160) |
| Multi-VAE                        | [Dawen Liang, Rahul G. Krishnan, Matthew D. Hoffman, Tony Jebara. 2018. Variational Autoencoders for Collaborative Filtering', WWW '18: Proceedings of the 2018 World Wide Web Conference](https://dl.acm.org/doi/10.1145/3178876.3186150) |
| Multi-DAE(Denoising AutoEncoder) | [Dawen Liang, Rahul G. Krishnan, Matthew D. Hoffman, Tony Jebara. 2018. Variational Autoencoders for Collaborative Filtering', WWW '18: Proceedings of the 2018 World Wide Web Conference](https://dl.acm.org/doi/10.1145/3178876.3186150) |

<br/>

In Multi-VAE and Multi-DAE, embeddings related to the tags of the problems were incorporated into the encoder's input. This integration aimed to facilitate the learning of side information about the problems, thereby enhancing performance.

<br/>



#### Models for Rivals Recommendation

| Name                                | Reference                                                    |
| ----------------------------------- | ------------------------------------------------------------ |
| Collective MF(Matrix Factorization) | [David Cortes. 2020. Cold-start recommendations in Collective Matrix Factorization](https://arxiv.org/abs/1809.00366) |
| K-nearest neighbors                 | [Altman, and Naomi S. 1992. An introduction to kernel and nearest-neighbor nonparametric regression](https://ieeexplore.ieee.org/abstract/document/4781121) |



<br/>

#### Models for Algorithm Problems Recommendation based on Rivals

| Name                                   | Reference                                                    |
| -------------------------------------- | ------------------------------------------------------------ |
| BPR(Bayesian Personalized Ranking)     | [Steffen Rendle, Christoph Freudenthaler, Zeno Gantner, and Lars Schmidt-Thieme. 2009. BPR: Bayesian Personalized Ranking from Implicit Feedback](https://arxiv.org/abs/1205.2618) |
| ALS(Alternating Least Squares) MF      | [Yifan Hu, Yehuda Koren, and Chris Volinsky. 2008. Collaborative Filtering for Implicit Feedback Datasets](https://ieeexplore.ieee.org/abstract/document/4781121) |
| Item-based CF(Collaborative Filtering) | [Badrul Sarwar, George Karypis, Joseph Konstan, and John Riedl. 2001. Item-based Collaborative Filtering Recommendation Algorithms](https://www.researchgate.net/publication/2369002_Item-based_Collaborative_Filtering_Recommendation_Algorithms) |



<br/>

### Architecture



![Architecture_0626](https://cdn.jsdelivr.net/gh/Glanceyes/Image-Repository/2022/06/26/20220626_1656245363.png)





<br/>

## Result



The deployment of RECJOON has led to a more personalized experience for BOJ and solved.ac users, enabling them to find suitable challenges and rivals that align with their current skill level. The intuitive interface and accurate recommendations have been positively received by the user community.

<br/>

### Algorithm Problems Recommendation



![Algorithm Recommender](https://cdn.jsdelivr.net/gh/Glanceyes/Image-Repository/2022/06/13/20220613_1655105972.gif)

<br/>

### Rivals Recommendation

![Rival Recommender](https://cdn.jsdelivr.net/gh/Glanceyes/Image-Repository/2022/06/13/20220613_1655116552.gif)

<br/>

### Algorithm Problems Recommendation based on Rivals

![Rival's Problem Recommender](https://cdn.jsdelivr.net/gh/Glanceyes/Image-Repository/2022/06/13/20220613_1655106250.gif)



<br/>

## Further Information



For further details, please visit the official [GitHub repository](https://github.com/boostcampaitech3/final-project-level3-recsys-14).

<br/>

For a comprehensive overview of the project, please refer to the [presentation video](https://www.youtube.com/watch?v=S5TFpJ0uZrA) or [slides](https://github.com/boostcampaitech3/final-project-level3-recsys-14/blob/main/experiments/documents/RECJOON_Presentation.pdf).

<br/>

For detailed insights into the primary model experiments, analyses, and the overall project progression, please refer to both the [presentation slides](https://github.com/boostcampaitech3/final-project-level3-recsys-14/blob/main/experiments/documents/RECJOON_Presentation.pdf) and the [Wrap-up Report](https://github.com/boostcampaitech3/final-project-level3-recsys-14/blob/main/experiments/documents/RECJOON_Wrap-up_Report.pdf).
