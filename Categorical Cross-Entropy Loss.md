$$\text{Categorical Cross-Entropy} = - \sum_{i=1}^{n} \sum_{j=1}^{k} y_{ij} \log(\hat{y}_{ij})$$
where:

- n is the number of data points
- k is the number of classes,
- $y_{ij}$​ is the binary indicator (0 or 1) if class label j is the correct classification for data point i
- $\hat{y}_{ij}$​ is the predicted probability for class j.

Liên quan: [[Multi-Class Classification]]