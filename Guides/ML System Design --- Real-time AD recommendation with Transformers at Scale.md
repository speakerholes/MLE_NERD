## Overview

This note will teach you the thought process of moving from the most basic version of AD prediction to a scalable system that can work on millions to billions of daily requests. If you want to go straight to the most optimized solution please skip to this #TODO (add a link). 

An ad recommendation system chooses which ads to show a user. To do this, it can use information about the ads, the user’s past activity, and the current request.

We’ll focus on predicting how likely a user is to click on an ad or take an action afterward, such as making a purchase. These predictions help the system decide which ads to show.

The challenge is doing this within a strict latency budget while handling many requests at once. Let’s start with a straightforward design: for each request, score every eligible ad using the user’s full history. Then we’ll look at where this becomes expensive and how to improve it.
## Brute-force

Before we start getting to more advanced scaling topics, I believe it's best to start from the bare bones brute-force solution so that we can get an intuition on how we can improve systems from the ground up. 

Lets first get a visual understanding of what brute-force may look like: 

![[brute-force-ad-recommendation.excalidraw|center|700]]

The diagram above shows our starting design. Before discussing its bottlenecks, let’s walk through how training and inference work.
### Training
1. Read historical training data from storage.
2. Process the data into the features and labels needed for training.
3. Train the ranking model on the processed data.
4. Save the model artifact so the serving service can load it for inference.
### Inference
1. Receive a request to recommend an ad to a user.
2. Retrieve the user’s stored information and build their features.
3. Load all eligible ads and their features.
4. Use the loaded model to score each eligible ad using user, ad, and request features.
5. Select the highest-scoring ad and return it to the user.

# Growth Dimensions

There are many areas that our system can grow. 
- Requests per second
- Eligible ads per request
- User history length
- Model size
- Training examples

## Bottleneck: User features

Within the bruteforce diagram there is a box that is scoring every ad.  If we peer into the box we will see there is a model that encodes the user feature history. We could possible have multiple requests from that same user and also that for each ad we have to recompute these user feature over and over again. This is a clear area for optimization. 




