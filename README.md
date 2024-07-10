# Creating ML models to predict Sberbank stock prices using news analysis.


# File	Description

|file|desc|
|-----|------|
|bot_sber.ipynb |	Jupyter notebook for running the bot|
|Main_notebook.ipynb |	Jupyter notebook for training models and data processing|
|final.csv	| Processed Dataset|
|news.csv|Dataset containing news articles|
|train.csv	|Dataset containing stock quotes|
-----------

# Idea and goals
I am interested in this theme because i'm an economist and i will  I will be participating in the AI Challenge (Sber Investments case).
The goal of this project is to make trading easier for new traders who may struggle to evaluate stock markets and news to make informed decisions. Therefore, they need a helper that predicts stock quotes.

# News analysis
I had 3 variants to do it:
- LLM Fine-tuning
- Promt engineering
- Vectorizing and using Classifier

So everything was much more easier than i thought. I've found model that defines sentiment of financial news on Hugging Face. 'cometrain/moexT5'
Using this model obtained sentiment of each news article in the news.csv Dataset. 
For simplicity, i considered positive news as 1, neutral as 0, negative as -1.
![image](https://github.com/iwance/sber_stocks_prediction/assets/165042795/39e40a91-0c8c-4735-acc6-2c144d715dc6)

The impact of news has lags, so we need to check several periods.
For each row in train.csv Dataset i created these features: mean sentiment of news in last X hours/days/weeks, also i filtered news and added features specifically about Sber.

I also added info about distribution of stock quotes, like mean/median/mim/max price, std. And day of week, date. 

# Training models
The SOTA models for this task are gradient boosting and RNN. I tried LSTM model, but Gradient Boosting is better for this data.
I chose XGBoost for the final model.
To prepare the data, I used a rolling window of 56 hours and flattened it into one vector. 
Then using Optuna, I found the best hyperparameters and trained the XGBoost model.
![image](https://github.com/iwance/sber_stocks_prediction/assets/165042795/e86a08bb-53c2-490b-ab06-dee889f38d8c)
![image](https://github.com/iwance/sber_stocks_prediction/assets/165042795/fe84d20a-2c2c-46d4-9044-135f18711821)
|Metric| Value|
|----|----|
|MSE| 1.89|
|RMSE| 1.37|
|MAE| 1.13|
|MAPE| 0.36|
|DA| 48.10|
-----

# New function
I defined func that simulates trading, and calculates % of profit. With comission 0.015% the result was +4% profit (during 80 hours), with 0.03% comission - +1% profit.

# Inference
I created telegram bot (bot_sber.ipynb) that regularly sends predictions and parses stock quotes. This is just MVP.
![image](https://github.com/iwance/sber_stocks_prediction/assets/165042795/14b04ff9-f3c3-4384-aa36-5ebbf3bc8a6d)




