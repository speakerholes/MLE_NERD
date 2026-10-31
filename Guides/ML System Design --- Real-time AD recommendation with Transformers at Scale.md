
This note will teach you the thought process of moving from the most basic version of AD prediction to a scalable system that can work on millions to billions of daily requests. If you want to go straight to the most optimized solution please skip to this #TODO (add a link). 

An ad recommendation system chooses which ads to show a user. To do this, it can use information about the ads, the user’s past activity, and the current request.

We’ll focus on predicting how likely a user is to click on an ad or take an action afterward, such as making a purchase. These predictions help the system decide which ads to show.

The challenge is doing this within a strict latency budget while handling many requests at once. Let’s start with a straightforward design: for each request, score every eligible ad using the user’s full history. Then we’ll look at where this becomes expensive and how to improve it.
## Brute-force


