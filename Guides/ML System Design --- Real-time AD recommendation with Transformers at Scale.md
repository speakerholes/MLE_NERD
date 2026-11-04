## Overview

This note will teach you the thought process of moving from the most basic version of AD prediction to a scalable system that can work on millions to billions of daily requests. If you want to go straight to the most optimized solution please skip to this #TODO (add a link). 

An ad recommendation system chooses which ads to show a user. To do this, it can use information about the ads, the user’s past activity, and the current request.

We’ll focus on predicting how likely a user is to click on an ad or take an action afterward, such as making a purchase. These predictions help the system decide which ads to show.

The challenge is doing this within a strict latency budget while handling many requests at once. Let’s start with a straightforward design: for each request, score every eligible ad using the user’s full history. Then we’ll look at where this becomes expensive and how to improve it.
## Brute-force

Before we start getting to more advanced scaling topics, I believe it's best to start from the bare bones brute-force solution so that we can get an intuition on how we can improve systems from the ground up. 

Lets first get a visual understanding of what brute-force may look like: 

![[brute-force-ad-recommendation.excalidraw|center|700]]

Lets imagine that the model takes about 20ms to score a single ad. That means for this diagram that it would take us 100ms to score all five ads. Lets start to think in terms of scale. 

One day we get a huge influx in businesses wanting our service to serve their ads. We now have 1000 ads to serve. Now at our current model runtime it would take around 20 seconds to serve an ad to a user. Your first thought might be to optimize the model more but even if you were to bring the model down to 1ms we still wouldn't be able to handle more scale. If we scaled to 1 million ads our brute force system would take approximately 20,000 seconds. As we can see this is note a scalable solution and you would be looking in the wrong place if you think optimizing the model is the best way to accomplish a scalable system. 



