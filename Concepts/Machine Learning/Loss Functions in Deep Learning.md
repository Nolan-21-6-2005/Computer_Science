---
title: "Loss Functions in Deep Learning"
source: "https://www.geeksforgeeks.org/deep-learning/loss-functions-in-deep-learning/"
author:
  - "[[GeeksforGeeks]]"
published: 2024-07-10
created: 2026-08-06
description: "Your All-in-One Learning Portal: GeeksforGeeks is a comprehensive educational platform that empowers learners across domains-spanning computer science and programming, school education, upskilling, commerce, software tools, competitive exams, and more."
tags:
  - "clippings"
---
A loss function measures how well a model’s predictions match the actual results by giving a numerical value for the error. A smaller value means better performance, and it guides the model during training.

- Measures the difference between predicted and actual values
- Guides training by helping algorithms like [gradient descent](https://www.geeksforgeeks.org/machine-learning/gradient-descent-algorithm-and-its-variants/) update parameters
- Used to evaluate model performance
- Influences how the model learns based on the type of errors
- Different loss functions are used for different tasks

## ***1\. Regression Loss Functions***

These are used when your model needs to predict a continuous number such as predicting the price of a product or age of a person. Popular regression loss functions are:

### 1\. Mean Squared Error (MSE) Loss

[Mean Squared Error (MSE)](https://www.geeksforgeeks.org/python/python-mean-squared-error/) Loss is one of the most widely used loss functions for regression tasks. It calculates the average of the squared differences between the predicted values and the actual values. It is simple to understand and sensitive to outliers because the errors are squared which can affect the loss.

> $\text{MSE} =\frac{1}{n}​\sum_{i=1}^{n}​(y_i​−\widehat{y}_i​)^2$

### 2\. Mean Absolute Error (MAE) Loss

[Mean Absolute Error (MAE)](https://www.geeksforgeeks.org/python/how-to-calculate-mean-absolute-error-in-python/) Loss is another commonly used loss function for regression. It calculates the average of the absolute differences between the predicted values and the actual values. It is less sensitive to outliers compared to MSE. But it is not differentiable at zero which can cause issues for some optimization algorithms.

> $\text{MAE}= \frac{1}{n}​\sum_{i=1}^{n}​ ∣y_i​ − \widehat{y_i}∣$

### 3\. Huber Loss

[Huber Loss](https://www.geeksforgeeks.org/machine-learning/sklearn-different-loss-functions-in-sgd/) combines the advantages of MSE and MAE. It is less sensitive to outliers than MSE and differentiable everywhere unlike MAE. It requires tuning of the parameter $\delta$. Huber Loss is defined as:

> $\begin{cases}\frac{1}{2} (y_i - \hat{y}_i)^2 & \quad \text{for } |y_i - \hat{y}_i| \leq \delta \\\delta |y_i - \hat{y}_i| - \frac{1}{2} \delta^2 & \quad \text{for } |y_i - \hat{y}_i| > \delta\end{cases}$

## ***2\. Classification Loss Functions***

Classification loss functions are used to evaluate how well a classification model's predictions match the actual class labels. There are different types of classification Loss functions:

### 1\. Binary Cross-Entropy Loss (Log Loss)

[Binary Cross-Entropy](https://www.geeksforgeeks.org/deep-learning/binary-cross-entropy-log-loss-for-binary-classification/) Loss is also known as Log Loss and is used for binary classification problems. It measures the performance of a classification model whose output is a probability value between 0 and 1.

> $\text{Binary Cross-Entropy} = - \frac{1}{n} \sum_{i=1}^{n} [y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i)]$

where:

- n is the number of data points
- $y_i$ is the actual binary label (0 or 1)
- $\hat{y}_i$​ is the predicted probability.

### 2\. Categorical Cross-Entropy Loss

[Categorical Cross-Entropy](https://www.geeksforgeeks.org/deep-learning/categorical-cross-entropy-in-multi-class-classification/) Loss is used for multiclass classification problems. It measures the performance of a classification model whose output is a probability distribution over multiple classes.

> $\text{Categorical Cross-Entropy} = - \sum_{i=1}^{n} \sum_{j=1}^{k} y_{ij} \log(\hat{y}_{ij})$

where:

- n is the number of data points
- k is the number of classes,
- $y_{ij}$​ is the binary indicator (0 or 1) if class label j is the correct classification for data point i
- $\hat{y}_{ij}$​ is the predicted probability for class j.

### 3\. Sparse Categorical Cross-Entropy Loss

[Sparse Categorical Cross-Entropy](https://www.geeksforgeeks.org/deep-learning/sparse-categorical-crossentropy-vs-categorical-crossentropy/) Loss is similar to Categorical Cross-Entropy Loss but is used when the target labels are integers instead of one-hot encoded vectors. It is efficient for large datasets with many classes.

> $\text{Sparse Categorical Cross-Entropy} = - \sum_{i=1}^{n} \log(\hat{y}_{i, y_i})$

where $y_i$ is the integer representing the correct class for data point i.

### 4\. Kullback-Leibler Divergence Loss (KL Divergence)

[KL Divergence](https://www.geeksforgeeks.org/machine-learning/kullback-leibler-divergence/) measures how one probability distribution diverges from a second expected probability distribution. It is often used in probabilistic models. It is sensitive to small differences in probability distributions.

> $\text{KL Divergence} = \sum_{i=1}^{n} \sum_{j=1}^{k} y_{ij} \log\left(\frac{y_{ij}}{\hat{y}_{ij}}\right)$

### 5\. Hinge Loss

[Hinge Loss](https://www.geeksforgeeks.org/machine-learning/hinge-loss-relationship-with-support-vector-machines/) is used for training classifiers especially for support vector machines (SVMs). It is suitable for binary classification tasks as it is not differentiable at zero.

> $\text{Hinge Loss} = \frac{1}{n} \sum_{i=1}^{n} \max(0, 1 - y_i \cdot \hat{y}_i)$

where:

- $y_i$​ is the actual label (-1 or 1)
- $\hat{y}_i$​ is the predicted value.

## ***3\. Ranking Loss Functions***

Ranking loss functions are used to evaluate models that predict the relative order of items. These are commonly used in tasks such as recommendation systems and information retrieval.

### 1\. Contrastive Loss

Contrastive Loss is used to learn embeddings such that similar items are closer in the embedding space while dissimilar items are farther apart. It is often used in Siamese networks.

> $\text{Contrastive Loss} = \frac{1}{2N} \sum_{i=1}^{N} \left( y_i \cdot d_i^2 + (1 - y_i) \cdot \max(0, m - d_i)^2 \right)$

where:

- $d_i$ is the distance between a pair of embeddings
- $y_i$ is 1 for similar pairs and 0 for dissimilar pairs
- m is a margin.

### 2\. Triplet Loss

Triplet Loss is used to learn embeddings by comparing the relative distances between triplets: anchor, positive example and negative example.

> $\text{Triplet Loss} = \frac{1}{N} \sum_{i=1}^{N} \left[ \|f(x_i^a) - f(x_i^p)\|_2^2 - \|f(x_i^a) - f(x_i^n)\|_2^2 + \alpha \right]_+$

where:

- f(x) is the embedding function
- $x_i^a$​ is the anchor
- $x_i^p$​ is the positive example
- $x_i^n$​ is the negative example
- $\alpha$ is a margin.

### 3\. Margin Ranking Loss

Margin Ranking Loss measures the relative distances between pairs of items and ensures that the correct ordering is maintained with a specified margin.

> $\text{Margin Ranking Loss} = \frac{1}{N} \sum_{i=1}^{N} \max(0, -y_i \cdot (s_i^+ - s_i^-) + \text{margin})$

where:

- $s_i^+$​ and $s_i^-$ are the scores for the positive and negative samples
- $y_i$​ is the label indicating the correct ordering.

## ***4\. Image and Reconstruction Loss Functions***

These loss functions are used to evaluate models that generate or reconstruct images ensuring that the output is as close as possible to the target images.

### 1\. Pixel-wise Cross-Entropy Loss

Pixel-wise Cross-Entropy Loss is used for image segmentation tasks where each pixel is classified independently.

> $\text{Pixel-wise Cross-Entropy} = - \frac{1}{N} \sum_{i=1}^{N} \sum_{c=1}^{C} y_{i,c} \log(\hat{y}_{i,c})$

where:

- N is the number of pixels,
- C is the number of classes
- $y_{i,c}$ is the binary indicator for the correct class of pixel
- $\hat{y}_{i,c}$ is the predicted probability for class c.

### 2\. Dice Loss

Dice Loss is used for image segmentation tasks and is particularly effective for imbalanced datasets. It measures the overlap between the predicted segmentation and the ground truth.

> $\text{Dice Loss} = 1 - \frac{2 \sum_{i=1}^{N} y_i \hat{y}_i}{\sum_{i=1}^{N} y_i + \sum_{i=1}^{N} \hat{y}_i}$

where:

- $y_i$ is the ground truth label
- $\hat{y}_i$ is the predicted label.

### 3\. Jaccard Loss (Intersection over Union, IoU)

Jaccard Loss is also known as IoU Loss that measures the intersection over union of the predicted segmentation and the ground truth.

> $\text{Jaccard Loss} = 1 - \frac{\sum_{i=1}^{N} y_i \hat{y}_i}{\sum_{i=1}^{N} y_i + \sum_{i=1}^{N} \hat{y}_i - \sum_{i=1}^{N} y_i \hat{y}_i}$

### 4\. Perceptual Loss

Perceptual Loss measures the difference between high-level features of images rather than pixel-wise differences. It is often used in image generation tasks.

> $\text{Perceptual Loss} = \sum_{i=1}^{N} \| \phi_j(y_i) - \phi_j(\hat{y}_i) \|_2^2$

where:

- $\phi_j$ is a layer in a pre-trained network
- $y_i$ and $\hat{y}_i$ are the ground truth and predicted images

### 5\. Total Variation Loss

Total Variation Loss encourages spatial smoothness in images by penalizing differences between adjacent pixels.

> $\text{Total Variation Loss} = \sum_{i,j} \left( (y_{i,j+1} - y_{i,j})^2 + (y_{i+1,j} - y_{i,j})^2 \right)$

## ***5\. Adversarial Loss Functions***

Adversarial loss functions are used in [generative adversarial networks (GANs)](https://www.geeksforgeeks.org/deep-learning/generative-adversarial-network-gan/) to train the generator and discriminator networks.

### 1\. Adversarial Loss (GAN Loss)

The standard GAN loss function involves a minimax game between the generator and the discriminator.

> $\min_G \max_D \mathbb{E}_{x \sim p_{data}(x)} [\log D(x)] + \mathbb{E}_{z \sim p_z(z)} [\log (1 - D(G(z)))]$

- The discriminator tries to ****maximize**** the probability of correctly classifying real and fake samples.
- The generator tries to ****minimize**** the discriminator’s ability to tell its outputs are fake.

### 2\. Least Squares GAN Loss

LSGAN modifies the standard GAN loss by using ****least squares error**** instead of log loss make the training more stable:

> ****Discriminator Loss****: $\min_D \frac{1}{2} \mathbb{E}_{x \sim p_{data}(x)} [(D(x) - 1)^2] + \frac{1}{2} \mathbb{E}_{z \sim p_z(z)} [D(G(z))^2]$
> 
> ****Generator Loss:**** $\min_G \frac{1}{2} \mathbb{E}_{z \sim p_z(z)} \left[ (D(G(z)) - 1)^2 \right]$

## ***6\. Specialized Loss Functions***

Specialized loss functions are designed for specific tasks such as sequence prediction, count data and cosine similarity.

### 1\. CTC Loss (Connectionist Temporal Classification)

CTC Loss is used for sequence prediction tasks where the alignment between input and output sequences is unknown.

> $\text{CTC Loss} = - \log(p(y | x))$

where p(y∣x) is the probability of the correct output sequence given the input sequence.

### 2\. Poisson Loss

Poisson Loss is used for count data modeling the distribution of the predicted values as a Poisson distribution.

> $\text{Poisson Loss} = \sum_{i=1}^{N} (\hat{y}_i - y_i \log(\hat{y}_i))$

$\hat{y}_i$ is the predicted count and $y_i$ is the actual count.

### 3\. Cosine Proximity Loss

Cosine Proximity Loss measures the cosine similarity between the predicted and target vectors encouraging them to point in the same direction.

> $\text{Cosine Proximity Loss} = - \frac{1}{N} \sum_{i=1}^{N} \frac{y_i \cdot \hat{y}_i}{\|y_i\| \|\hat{y}_i\|}$

### 4\. Earth Mover's Distance (Wasserstein Loss)

Earth Mover's Distance measures the distance between two probability distributions and is used in Wasserstein GANs.

> $\text{Wasserstein Loss} = \mathbb{E}_{x \sim p_r} [D(x)] - \mathbb{E}_{z \sim p_z} [D(G(z))]$

## Choosing the Right Loss Function

Selecting an appropriate loss function is essential for training a model effectively and achieving good performance. The choice depends on the task, data type, and learning objectives.

- Use MSE or MAE for regression tasks, Cross-Entropy for classification and Dice or Jaccard Loss for segmentation problems
- Choose regression losses for continuous outputs, classification losses for categorical outputs, and CTC Loss for sequence tasks like speech or handwriting
- For imbalanced datasets, use Focal Loss to focus more on difficult or rare examples
- When data contains outliers, use Huber Loss for more stable and robust learning
- Select loss functions that support faster convergence and better overall model performance


<iframe id="google_ads_iframe_/27823234/GFG_Desktop_RightSideBar_ATF_300x250_2_0" name="google_ads_iframe_/27823234/GFG_Desktop_RightSideBar_ATF_300x250_2_0" title="3rd party ad content" scrolling="no" marginwidth="0" marginheight="0" style="border: 0px none; vertical-align: bottom;" aria-label="Advertisement" tabindex="0" data-google-container-id="1" data-load-complete="true" width="300" height="600" frameborder="0"></iframe>

<iframe id="google_ads_iframe_/27823234/22683361272_0" name="google_ads_iframe_/27823234/22683361272_0" title="3rd party ad content" scrolling="no" marginwidth="0" marginheight="0" style="border: 0px none; vertical-align: bottom;" aria-label="Advertisement" tabindex="0" data-google-container-id="2" data-load-complete="true" width="300" height="250" frameborder="0"></iframe>

[![GeeksforGeeks](https://media.geeksforgeeks.org/auth-dashboard-uploads/gfgFooterLogoDark.png)](https://www.geeksforgeeks.org/)

![location](https://media.geeksforgeeks.org/img-practice/Location-1685004904.svg)

Corporate & Communications Address:

A-143, 6th Floor, Sovereign Corporate Tower, Sector- 136, Noida, Uttar Pradesh (201305)

![location](https://media.geeksforgeeks.org/img-practice/Location-1685004904.svg)

Registered Address:

K 061, Tower K, Gulshan Vivante Apartment, Sector 137, Noida, Gautam Buddh Nagar, Uttar Pradesh, 201305

[

](https://in.linkedin.com/company/geeksforgeeks)[

](https://www.instagram.com/geeks_for_geeks/)[

](https://twitter.com/geeksforgeeks)[

](https://www.facebook.com/geeksforgeeks.org/)[

](https://www.youtube.com/geeksforgeeksvideos)

[![GFG App on Play Store](https://media.geeksforgeeks.org/auth-dashboard-uploads/googleplay-%281%29.png)](https://geeksforgeeksapp.page.link/gfg-app)[![GFG App on App Store](https://media.geeksforgeeks.org/auth-dashboard-uploads/appstore-%281%29.png)](https://geeksforgeeksapp.page.link/gfg-app)

- Company
- [About Us](https://www.geeksforgeeks.org/about/)
- [Legal](https://www.geeksforgeeks.org/legal/)
- [Privacy Policy](https://www.geeksforgeeks.org/legal/privacy-policy/)
- [Contact Us](https://www.geeksforgeeks.org/about/contact-us/)
- [Advertise with us](https://www.geeksforgeeks.org/advertise-with-us/)
- [GFG Corporate Solution](https://www.geeksforgeeks.org/gfg-corporate-solution/)
- [Campus Training Program](https://www.geeksforgeeks.org/campus-training-program/)
- Explore
- [POTD](https://www.geeksforgeeks.org/problem-of-the-day)
- [Job-A-Thon](https://practice.geeksforgeeks.org/events/rec/job-a-thon/)
- [Blogs](https://www.geeksforgeeks.org/category/blogs/?type=recent)
- [Nation Skill Up](https://www.geeksforgeeks.org/nation-skill-up/)
- Tutorials
- [Programming Languages](https://www.geeksforgeeks.org/computer-science-fundamentals/programming-language-tutorials/)
- [DSA](https://www.geeksforgeeks.org/dsa/dsa-tutorial-learn-data-structures-and-algorithms/)
- [Web Technology](https://www.geeksforgeeks.org/web-tech/web-technology/)
- [AI, ML & Data Science](https://www.geeksforgeeks.org/machine-learning/ai-ml-and-data-science-tutorial-learn-ai-ml-and-data-science/)
- [DevOps](https://www.geeksforgeeks.org/devops/devops-tutorial/)
- [CS Core Subjects](https://www.geeksforgeeks.org/gate/gate-exam-tutorial/)
- [Interview Preparation](https://www.geeksforgeeks.org/aptitude/interview-corner/)
- [Software and Tools](https://www.geeksforgeeks.org/websites-apps/software-and-tools-a-to-z-list/)
- Courses
- [ML and Data Science](https://www.geeksforgeeks.org/courses/category/machine-learning-data-science)
- [DSA and Placements](https://www.geeksforgeeks.org/courses/category/dsa-placements)
- [Web Development](https://www.geeksforgeeks.org/courses/category/development-testing)
- [Programming Languages](https://www.geeksforgeeks.org/courses/category/programming-languages)
- [DevOps & Cloud](https://www.geeksforgeeks.org/courses/category/cloud-devops)
- [GATE](https://www.geeksforgeeks.org/courses/category/gate)
- [Trending Technologies](https://www.geeksforgeeks.org/courses/category/trending-technologies/)
- Videos
- [DSA](https://www.geeksforgeeks.org/videos/category/sde-sheet/)
- [Python](https://www.geeksforgeeks.org/videos/category/python/)
- [Java](https://www.geeksforgeeks.org/videos/category/java-w6y5f4/)
- [C++](https://www.geeksforgeeks.org/videos/category/c/)
- [Web Development](https://www.geeksforgeeks.org/videos/category/web-development/)
- [Data Science](https://www.geeksforgeeks.org/videos/category/data-science/)
- [CS Subjects](https://www.geeksforgeeks.org/videos/category/cs-subjects/)
- Preparation Corner
- [Interview Corner](https://www.geeksforgeeks.org/interview-prep/interview-corner/)
- [Aptitude](https://www.geeksforgeeks.org/aptitude/aptitude-questions-and-answers/)
- [Puzzles](https://www.geeksforgeeks.org/aptitude/puzzles/)
- [GfG 160](https://www.geeksforgeeks.org/courses/gfg-160-series)
- [System Design](https://www.geeksforgeeks.org/system-design/system-design-tutorial/)

[@GeeksforGeeks, Sanchhaya Education Private Limited](https://www.geeksforgeeks.org/), [All rights reserved](https://www.geeksforgeeks.org/copyright-information/)

<iframe marginwidth="0" marginheight="0" scrolling="no" id="1494dc73e0c6c18" src="about:blank" style="display: none; height: 0px; width: 0px; border: 0px none;" name="__pb_locator__" width="0" height="0" frameborder="0"></iframe><iframe style="display: none; width: 0px; height: 0px; border: medium none; z-index: -1000; left: -1000px; top: -1000px;" name="googlefcPresent"></iframe><iframe name="__tcfapiLocator" src="about:blank" style="display: none; width: 0px; height: 0px; border: medium none; z-index: -1000; left: -1000px; top: -1000px;"></iframe><iframe name="__uspapiLocator" src="about:blank" style="display: none; width: 0px; height: 0px; border: medium none; z-index: -1000; left: -1000px; top: -1000px;"></iframe><iframe name="__gppLocator" src="about:blank" style="display: none; width: 0px; height: 0px; border: medium none; z-index: -1000; left: -1000px; top: -1000px;"></iframe><iframe name="googlefcInactive" src="about:blank" style="display: none; width: 0px; height: 0px; border: medium none; z-index: -1000; left: -1000px; top: -1000px;"></iframe><iframe name="googlefcLoaded" src="about:blank" style="display: none; width: 0px; height: 0px; border: medium none; z-index: -1000; left: -1000px; top: -1000px;"></iframe><iframe src="https://gum.criteo.com/syncframe?origin=publishertagids&amp;topUrl=www.geeksforgeeks.org&amp;gdpr=0&amp;gdpr_consent=&amp;gpp=&amp;gpp_sid=-1#{%22bundle%22:{%22value%22:%221R5BoV9MSEJXblUwaHNqSkJXNVVIZFhDcVNKa0RFNTZvaFFrZTNTWkh2UXU0Z0NzRzZYUXd1TWZ3SHFJeTV2b1FqdXBlRzNKRlh0NFNlT1FBOVJtMEYwUG1PVmVrcDNVUkZPa2h6Y1B0anFLa1FpYlk4N3U2aUslMkI1QURFclRZd1dBOW5adFA3cGhFJTJCbzMzbVMlMkJhRmhObU1odHclM0QlM0Q%22,%22origin%22:3},%22optout%22:{%22value%22:false,%22origin%22:0},%22tld%22:%22geeksforgeeks.org%22,%22topUrl%22:%22www.geeksforgeeks.org%22,%22version%22:108329,%22origin%22:%22publishertagids%22,%22requestId%22:%220.45905589097086397%22}" style="border-width: 0px; margin: 0px; display: none;" sandbox="allow-scripts allow-same-origin" aria-hidden="true" tabindex="-1" title="Criteo GUM iframe" width="0" height="0" frameborder="0"></iframe><iframe src="https://www.google.com/recaptcha/api2/aframe" style="display: none;" width="0" height="0"></iframe><iframe name="google_ads_top_frame" id="google_ads_top_frame" style="display: none; position: fixed; left: -999px; top: -999px; width: 0px; height: 0px;"></iframe>