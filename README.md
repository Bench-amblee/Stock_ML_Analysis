# Reddit vs. the News
## Which is more accurate at predicting stock movement? 

For code go to [Notebooks](#/notebooks)
For raw data go to [Data](#/data)

## Overview
This project investigates whether sentiment analysis of social media discussions and news articles can effectively predict stock market movements. Using natural language processing and machine learning techniques, we analyze sentiment patterns from popular subreddits (such as r/wallstreetbets, r/investing, and r/stocks) and compare them with sentiment extracted from financial news sources to determine which better correlates with subsequent stock price changes.
Built entirely in Python, this project sources data from the Reddit API and NewsAPI all related to the "Magnificent 7" Stocks over the last year (March 2024 to March 2025). This approach combines sentiment analysis, feature engineering, hyperparameter optimization, and various machine learning models to identify predictive patterns in text data that might precede market movements, potentially offering investors an alternative signal for decision-making.
![Process map](/images/process-map.png)
*High-level visualization of the process overview*

## Thought Process
The thought process was to build a classifier based on compound sentiment score and predicted direction. 
- compound sentiment: a combination of the positive and negative words calculated by NLTK VADER
- predicted direction: a classification variable that assigns three values that evaluate a stock's movement: "up", "down", or "flat" based on the sentiment of the Reddit post of news article.
  
### Initial Approach
- This approach was based on similar studys by [Shan Zhong and David B. Hitchcock](https://arxiv.org/abs/2108.10826) and by [Taylan Kabbani and Faith Enes Usta](https://arxiv.org/abs/2201.12283)
- I wanted to make the study unique by comparing Reddit to the News to see if a noticable difference could be found in overall accuracy
  
### Key Design Decisions
- Decision 1: [Creating a classification variable: prediction_direction to match the stock's direction. This process involved evaluating the sentiment and then adding an additional classification variable. My initial concern was that this might have resulted in overfitting but it proved to be very useful while evaluating models]
- Decision 2: [Weighted Ensemble Model: While one model significantly outperformed the other in terms of accuracy, using both models with a weighted bias towards the more accurate one yielded better overall results]
- Decision 3: [Random Forest Classifier: this model was selected because it outperformed other classification models and worked well with using mixed features from the numerical sentiment score and categorical prediction classifier]

## Challenges Faced

### Technical Challenges
1. **Sourcing Social Media Data**: My original project plan involved using data from Twitter (X), Seeking Alpha, and various other social media sites. However their APIs were either too expensive or difficult to use with the free tier.
   - This changed my original goal of comparing multiple platforms, I had to adapt to just using Reddit and NewsAPI since they were the only ones that worked for me.
   - If given more time, I could have either used the free tier to slowly collect data over multiple days or potentially outsource this collection process.
   - Social media data is becoming more valuble so it makes sense that it's becoming more difficult to source.

2. **Initially Low Accuracy**: Prior to any optimization or hyperparameter tuning, both models performed very poorly. Even despite optimization the model's final accuracy was relatively low.
   - While reading similar studies, I found that similar projects had an accuracy around 60-70% which is exactly what the final optimized model had. So it did perform well relatively but it was difficult at first to work with low accuracy scores.
     
## Results & Interpretation

### Key Metrics
- Reddit Model Accuracy : [43.8%]  - Even with optimized features and hyperparameters, the best performing reddit sentiment model was less accurate than a coin flip.
- News Model Accuracy: [63.5%] - Turns out reading the news might make you a much better investor (not financial advice) this tells us that in general the news' sentiment lines up significantly more with the actual stock market.
- Final Weighted Ensemble Model Accuracy: [66.5%] - Combining both sources gives us the most accurate model!

### Analysis
The final weighted ensemble model, combining the strengths of both data sources, achieved 66.5% accuracy - this doesn't sound like a lot, but relative to other professional studies, which typically achieve 60-70% accuracy in similar prediction tasks, it seems to be performing well! This performance is particularly promising given the inherent volatility and unpredictability of financial markets.

![Results confusion matrix](/images/confusion-matrix.png)
![Results confusion matrix](/images/model-accuracy.png)
*Visualization of a confusion matrix of the final optimized model and a comparison of each model vs. the final weighted ensemble model*


## Next Steps

### Short-term Improvements
- Refined Feature Engineering: [Refine the process of prediction features, possibly incorporating confidence levels or additional lables like "up_likely" and "up_very_likely" for more details.]
- Additional Data Sources: [Expanding the training data to include other sources like Twitter (X), Seeking Alpha, or any other sources that may improve accuracy.]
- Subreddit Analysis: [Reddit's poor performance might be the result of poor subreddit selection - a seperate analysis could be performed that removes WallStreetBets or tests other relevant subreddits.]

## Deployment Considerations

### Requirements
- Implement a pipeline to retrain the model periodically (weekly/monthly) as new data becomes available. Regularly retrain the model to improve overall accuracy especially as new stock data becomes available.
- Real-time Sentiment: Develop a real-time sentiment analysis system to continuously update sentiment features.
- Prediction Integration: Utilize APIs from the Data Collection Notebook and cleaning steps from the Data Cleaning notebook to automatically ingest predictions from Reddit and NewsAPI sources.
- For cloud deployment (GCP) a sample workflow could involve:
    - Cloud Functions to run python scripts for data sourcing and cleaning
    - Cloud Dataflow for processing
    - Vertex AI Pipelines for ML Pipeline development
    - Vertex AI for Model Deployment and Re-Training
    - Cloud Monitoring for real time updates and alerts

#### Thank You :)
