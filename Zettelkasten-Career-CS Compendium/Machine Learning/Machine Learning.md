---
tags:
  - MachineLearning
  - LEAF
---
Machine Learning (ML) is about teaching a computer to learn patterns from data instead of explicitly programming it.
# Types of ML:
- Supervised Learning – You have labeled data (inputs + correct outputs). The model learns to map inputs to outputs.    
    - Example: Predicting house prices from features like size, bedrooms, location.
- Unsupervised Learning – You only have input data, no labels. The model finds patterns or groups in the data.
    - Example: Grouping customers into segments.
- Reinforcement Learning – The model learns by trial and error, getting rewards or penalties.
    - Example: A robot learning to walk.

# Supervised Learning
Supervised learning is a type of ML where the model is trained on labelled data (input data paried with the correct output).The algorithm learns the mapping between inputs and outputs so it can make predictions on new, unseen data

## Types of supervised learning:
- Regression: Predicting continuous values (e.g., house prices, temperature).
- Classification: Predicting discrete categories (e.g., spam vs. not spam, disease diagnosis).

### Regression
Regression is a type of supervised learning used when the output (target variable) is continuous, meaning it can take on a wide range of numeric values. The goal is to predict a number based on input features. For example, predicting house prices based on square footage, location, and number of rooms. 
Essentially, it tries to find the relationship between a dependent variable (the output you want to predict) and one or more independent variables (the inputs or features).

Common regression algorithms include:
- Linear Regression: Models the relationship between input features and a continuous target by fitting a straight line (or hyperplane in higher dimensions). It assumes a linear relationship between inputs and output.
- Polynomial Regression: An extension of linear regression where input features are transformed into polynomial terms (e.g., x², x³) to capture non-linear relationships while still being solved with linear methods.
- Regression Trees: A decision tree that predicts continuous values by splitting the data into regions based on feature thresholds and averaging the outputs within each region.

#### Evaluation Metrics
Uses metrics that measure numerical error, such as Mean Absolute Error (MAE), Mean Squared Error (MSE), or Root Mean Squared Error (RMSE), and sometimes R² (coefficient of determination).

### Classification
Classification is a type of supervised learning used when the output (target variable) is categorical, meaning it belongs to one of a finite set of classes or labels. The goal is to assign an input to the correct category. For example, predicting whether an email is spam or not, or identifying if an image is of a cat, dog, or bird. 
Common classification algorithms include
- Logistic Regression: Despite its name, it is a classification algorithm. It uses the logistic (sigmoid) function to model probabilities for binary or multi-class outcomes.
- Decision Trees: A tree-based model that makes predictions (classification or regression) by splitting data on feature values, forming a hierarchy of decision rules.
- Random Forests: An ensemble method that builds many decision trees on different subsets of data and features, then combines their outputs (average for regression, majority vote for classification) to improve accuracy and reduce overfitting.
- Support Vector Machines (SVMs): A classification (and sometimes regression) method that finds the optimal boundary (hyperplane) between classes by maximizing the margin between data points of different classes.

#### Evaluation Metrics
Uses metrics based on prediction accuracy, such as accuracy, precision, recall, F1-score, and for probabilistic outputs, ROC curve and AUC (Area Under Curve).

## Key concepts in supervised learning
- Training data and labels: Inputs and their corresponding correct outputs.
- Model (hypothesis function): The mathematical representation that maps inputs to outputs.
- Loss function: A measure of how wrong the model’s predictions are compared to true values.
- Optimization: The process (e.g., gradient descent) of minimizing the loss to improve performance.
- Overfitting vs. underfitting: Balancing between memorizing the training data and generalizing well to new data.
- Evaluation metrics: Tools to assess performance (e.g., accuracy, precision, recall for classification; RMSE, MAE for regression).

# Unsupervised Learning
Unsupervised learning is a type of machine learning where the algorithm is given data without labeled outputs and must find hidden patterns or structure on its own. Unlike supervised learning, there is no “correct answer” provided during training. The model tries to group similar data points together or reduce the complexity of data while preserving important information. It’s often used for exploratory analysis, pattern discovery, and preprocessing.

## Types of unsupervised learning:
- Clustering: Grouping data points based on similarity (e.g., customer segmentation, k-means clustering).
- Dimensionality reduction: Compressing data into fewer features while retaining important information (e.g., PCA, t-SNE).
- Association rule learning: Discovering relationships or rules in data (e.g., market basket analysis: “people who buy X also buy Y”).

### Clustering
A method of unsupervised learning that groups data points into clusters based on similarity. Points within the same cluster are more similar to each other than to points in other clusters. 
Common algorithms include:
- K-means: A clustering algorithm that partitions data into k clusters by assigning points to the nearest cluster center and iteratively updating the centers until convergence. It works best for spherical, evenly sized clusters.
- Hierarchical Clustering: Builds a hierarchy of clusters either by agglomerative (bottom-up) merging or divisive (top-down) splitting. It produces a dendrogram showing how clusters are related at different levels.
- DBSCAN: Density-Based Spatial Clustering of Applications with Noise groups points that are closely packed together and marks points in low-density regions as outliers. It can find clusters of arbitrary shape and handles noise well.

#### Evaluation Metrics
Metrics like silhouette score, Davies–Bouldin index, or Calinski–Harabasz index measure how well the data points are grouped and separated without using labels.

### Dimensionality reduction
A technique that reduces the number of features in a dataset while preserving as much important information as possible. It helps with visualization, noise reduction, and faster computation. 
Common methods include
- Principal Component Analysis (PCA): A dimensionality reduction technique that transforms data into a smaller set of uncorrelated variables called principal components, capturing the most variance in the data.
- t-SNE: A nonlinear dimensionality reduction technique primarily used for visualization. It maps high-dimensional data into 2 or 3 dimensions while preserving local similarities between points.

#### Evaluation Metrics
Metrics like explained variance or reconstruction error assess how much information is preserved when reducing the number of features.

### Association rule learning
A method that finds interesting relationships or patterns between variables in a dataset. It often answers “if-then” questions, like “if a customer buys X, they are likely to buy Y.” 
Common algorithms include
- Apriori: An association rule learning algorithm that identifies frequent itemsets in transactional data and generates rules with minimum support and confidence. Commonly used in market basket analysis.
- Eclat: Another association rule learning algorithm that uses a vertical database layout and intersection-based computation to efficiently find frequent itemsets. Often faster than Apriori for dense datasets.

#### Evaluation Metrics
Metrics such as support, confidence, and lift evaluate how frequently items occur together and how strong the discovered rules are.

## Key concepts in unsupervised learning
- Similarity/distance measures: How “close” or “different” data points are (e.g., Euclidean distance, cosine similarity).
- Clusters and centroids: Group centers in clustering methods.
- High-dimensional data: Data with many features where patterns may be hard to see without reduction.
- Interpretability: Since there are no labels, evaluating results can be subjective and context-dependent.
- Evaluation methods: Metrics like silhouette score, Davies–Bouldin index, or reconstruction error (for dimensionality reduction).

# Reinforcment Learning
Reinforcement learning (RL) is a type of machine learning where an agent learns to make decisions by interacting with an environment. Instead of being given labeled examples, the agent receives feedback in the form of rewards or penalties based on its actions and aims to maximize the cumulative reward over time. RL is commonly used in scenarios where sequential decision-making is important, such as robotics, game playing, or self-driving cars.

## Types of reinforcement learning
- Model-free RL: The agent learns the best actions without building a model of the environment (e.g., Q-learning, SARSA).
- Model-based RL: The agent builds a model of the environment’s dynamics and uses it to plan optimal actions.
- Policy-based methods: Directly learn a policy that maps states to actions (e.g., REINFORCE, PPO).
- Value-based methods: Learn the value of each state or state-action pair to guide decision-making (e.g., Q-learning, Deep Q-Networks).
- Actor-Critic methods: Combine policy-based and value-based approaches for more efficient learning.

### Model-free RL
The agent learns how to act purely from experience, without trying to understand or model the environment’s dynamics. It relies on trial and error to improve its policy. Common algorithms: 
- Q-learning: A model-free, value-based algorithm where the agent learns a Q-value function that estimates the expected reward for each state-action pair. It chooses actions that maximize Q-values to improve its policy over time.
- SARSA: Similar to Q-learning, but updates the Q-values using the action actually taken in the next state, making it an “on-policy” method that tends to be more conservative.
- Deep Q-Networks (DQN): Extends Q-learning by using a neural network to approximate Q-values, allowing it to handle high-dimensional state spaces like images.

### Model-based RL
The agent builds or uses a model of the environment, predicting how states change in response to actions, and uses this model to plan optimal strategies. This can make learning more sample-efficient. 
Common algorithms: 
- Dyna-Q: A model-based extension of Q-learning that uses a learned model of the environment to generate simulated experiences, improving sample efficiency.
- Monte Carlo Tree Search (MCTS): A planning algorithm that explores possible future action sequences in a tree structure, using random simulations to estimate the value of actions, commonly used in games like Go or Chess.

### Policy-based methods
These directly learn a policy that maps states to actions, which is useful for problems with continuous action spaces or stochastic policies. They optimize the policy to maximize expected rewards. 
Common algorithms: 
- REINFORCE: A policy-based method that directly optimizes the probability of taking actions to maximize expected rewards, using the policy gradient approach.
- Proximal Policy Optimization (PPO): An advanced policy-based algorithm that updates policies in a stable way by limiting how much the new policy can deviate from the old one, improving training reliability.
- Trust Region Policy Optimization (TRPO): Similar to PPO, it ensures that policy updates stay within a “trust region” to prevent large, destabilizing updates.

### Value-based methods
Instead of learning a policy directly, the agent learns a value function estimating how good each state or action is in terms of expected future rewards. The policy is then derived by choosing actions with the highest value. 
Common algorithms: 
- Q-learning: A model-free, value-based algorithm where the agent learns a Q-value function that estimates the expected reward for each state-action pair. It chooses actions that maximize Q-values to improve its policy over time.
- SARSA: Similar to Q-learning, but updates the Q-values using the action actually taken in the next state, making it an “on-policy” method that tends to be more conservative.
- Deep Q-Networks (DQN): Extends Q-learning by using a neural network to approximate Q-values, allowing it to handle high-dimensional state spaces like images.

### Actor-Critic methods
These combine both approaches: the actor proposes actions according to a policy, and the critic evaluates them using a value function, allowing more stable and efficient learning. 
Common algorithms: 
- A2C (Advantage Actor-Critic): An actor-critic method where the actor proposes actions and the critic evaluates them using the advantage function to stabilize learning.
- A3C (Asynchronous Advantage Actor-Critic): A parallelized version of A2C where multiple agents explore different environments asynchronously, speeding up training and improving exploration.
- DDPG (Deep Deterministic Policy Gradient): An actor-critic method designed for continuous action spaces, combining deterministic policy gradients with deep neural networks for both actor and critic.

## Key concepts in reinforcement learning
- Agent, environment, state, and action: Core elements defining the RL setup.
- Reward function: Signals the agent receives to evaluate its actions.
- Policy: A strategy that the agent follows to choose actions.
- Value function: Estimates how good a state or action is in terms of expected future rewards.
- Exploration vs. exploitation: Balancing trying new actions to discover rewards versus using known actions that give high rewards.
- Discount factor: Determines the importance of future rewards compared to immediate rewards.