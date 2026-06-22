# Amazon Personalize <!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Recipes](#2-recipes)

# 1. Introduction

- Fully managed ML-service to build apps with real-time personalized recommendations.
- **Example:** Personalized product recommendations/re-ranking, customized direct marketing.
  - **Example:** User bought gardening tools, provide recommendations on the next one to buy.
- Same technology used by Amazon.com.
- Integrates into existing websites, applications, SMS, email marketing systems, ...
- Implement in days, not months (we don't need to build, train, and deploy ML solutions).
- **Use cases:** Retail stores, media and entertainment...

TODO DIAGRAM

# 2. Recipes

- Algorithms that are prepared for specific use cases.
- We must provide the training configuration on top of the recipe.
- **Example recipes**
  - Recommending items for users (`USER_PERSONALIZATION` recipes).
    - User-Personalization-v2.
  - Ranking items for a user (`PERSONALIZED_RANKING` recipes).
    - Personalized-Ranking-v2.
  - Recommending trending or popular items (`POPULAR_ITEMS` recipes).
    - Trending-Now, Popularity-Count.
  - Recommending similar items (`RELATED_ITEMS` recipes).
    - Similar-Items.
  - Recommending the next best action (`PERSONALIZED_ACTIONS` recipes).
    - Next-Best-Action.
  - Getting user segments (`USER_SEGMENTATION` recipes).
    - Item-Affinity.
- **NOTE:** Recipes and personalize are for recommendations.
