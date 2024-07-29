# (MCM/ICM)The Best Investment Strategies For Gold And Bitcoin


# 前言

美赛论文，由于不能在md中方便的插入图片与数学公式，所以我选择删掉对应的地方。

英文译制版本。

论文配套的代码均为本人编写，感谢组员组长的配合论文撰写。

完整材料详见博客相关资源

# 正文

#### 2022

#### MCM/ICM

# The Best Investment Strategies For Gold And Bitcoin


### Summary

With the rapid development of the economy, financial investment has become more and more
common. But how to accurately grasp the market trend so as to make reasonable trading choices is
a difficult problem. Our team uses historical price data to formulate a portfolio investment strategy
of gold and bitcoin to help traders invest safely and make long-term profits.

For question 1, to formulate the best investment strategy, it is necessary to determine the best
investment ratio and trading timing. For this reason, we establish an **online portfolio trading
model with commissions**. Based on historical price data, risk assessment and expected returns,
and introducing **risk preference coefficients** , a **multi-objective planning model** is constructed to
obtain the best investment ratio of the day too. We use the "Double Time Period Confirmation
Principle" to improve the " **Dow Theory** ", and through the monitoring of historical prices, we use
the Dow model to analyze the trend of asset prices to determine the long-term and medium-term
trends of asset prices. Finally, take the breakout point of the medium-term trend as a suitable date
and invest with the best investment ratio. We choose a risk preference coefficient of 0.6. Taking
the commission ratio required by the question into the model calculation, we get the result that by
the end of the transaction in 2021, the total income will reach $72,087.61, and the investment rate
of return will be 7108.7%.

For question 2, through the comparative analysis of the **profitability** , **risk** , **Sharpe coeffi-
cient** and **capital liquidity of the investment** , we obtained the conclusion that the model has
higher returns, better liquidity, the best performance of risk control and Sharpe coefficient. After
comprehensive analysis , it shows that the strategy provided by this model is the best investment
strategy.

For question 3, we conducted a **sensitivity analysis** on transaction costs,then came to the con-
clusion that transaction costs have a greater impact on gold investment, less on Bitcoin investment,
and mainly affect the number of transactions and investment ratio. After analysis,transaction fees
have a direct impact on transaction frequency, transaction selection and transaction amount, and
it affects the benefits and risks in actual transactions by changing the coefficients of the objective
function and constraints in the model.

Finally, we analyze the shortcomings of the model and the direction of optimization, and attach
a memo.

**Keywords** : Dow Theory; Online Portfolio Investment; risk preference coefficient; Sharpe ratio


## Contents

- 1 Introduction
   - 1.1 Background and restatement of the problem
   - 1.2 Our works
- 2 Model preparation
   - 2.1 Assumptions
   - 2.2 Abbreviation and definition
   - 2.3 Data processing
- 3 Online portfolio trading model with commission
   - 3.1 Model introduction
   - 3.2 Establishment of the portfolio trading model
   - 3.3 The solution of portfolio transaction planning model
- 4 Trading strategies based on Dow Theory
   - 4.1 Overview of theoretical principles
   - 4.2 Formation of trading strategy
   - 4.3 Analysis of the effect and rationality of the strategy
- 5 Overall trading plan
   - 5.1 Division of trading periods
   - 5.2 Confirmation of the transaction plan
   - 5.3 Analysis of transaction results
- 6 Analysis of model advantages
   - 6.1 Profitability perspective
   - 6.2 Risk perspective
   - 6.3 Risk-return composite indicator - Sharpe ratio
   - 6.4 Liquidity perspective
- 7 Sensitivity of the strategy to transaction costs
- 8 Model advantages and disadvantages and optimization solutions
- 9 Memorandum


## 1 Introduction

### 1.1 Background and restatement of the problem

In the context of today’s rapid financial development, traders can choose from numerous
financial investment products and trade the volatile assets they invest in to reap the desired returns.
Among them, gold and bitcoin are the financial assets selected in this paper. Bitcoin is traded every
day while gold is traded only on the open day, and traders need to pay a proportional commission
during the trade. Assume an investment activity with a starting capital of $1,000 from September
11, 2016 to September 10, 2021. And by trading or holding the assets in the portfolio of gold,
bitcoin, and U.S. dollars owned by the trader during this time, ultimately to maximize the final total
return. To achieve this goal, we need to do the following:

- Build an investment model for this scenario, providing strategies based only on price data up
    to that day. And estimate the asset value on September 10, 2021 (investment deadline).
- Prove that the strategy given in the model established above is optimal.
- Find out how sensitive the strategy is to transaction costs and how transaction costs affect the
    strategy and final outcome.

### 1.2 Our works

1. Analyze the historical price data of the two assets, and design a calculation model for the
    transaction ratio of the two assets.
2. According to the Dow theory and previous data, determine the price trend and transaction
    timing.
3. By judging the investment ratio and transaction date, formulate the best trading strategy to
    achieve higher returns and lower risks
4. Evaluate the investment strategy formulated in terms of profitability, risk, and liquidity , to
    prove that the model is the best model
5. Analyze the impact of commission on the model, and explore changes in investment strategies
    and returns under different commission rates.

## 2 Model preparation

### 2.1 Assumptions

Considering the solvability and simplification principle of the model, we have the following
assumptions:

**Assumption 1** : All raw data collections are reasonable and objective, and can reflect the real
situation.


**Assumption 2** : It is assumed that the transaction time has a good continuity, that is, once a
transaction is decided, it can be completed in an instant without interruption or delay.


**Assumption 3**: Individuals or institutions have limited influence on the trading market.

**Assumption 4** : There is no negative equity, that is, no loans.

### 2.2 Abbreviation and definition


### 2.3 Data processing

In the process of preprocessing the original data, we found that some data on the opening day
of the gold market were missing. The missing date distribution is discretized and the number is
small, so we use the Lagrangian interpolation method to fill in the missing data. The calculation
formula is as follows:

Wherelk(x)is the interpolation basis function, andf(xk)is the corresponding gold price on date
xk.

## 3 Online portfolio trading model with commission

Figure 1 is the schematic diagram of portfolio transaction. Before each transaction, we need
to determine the ratio of the two transaction shares. Due to the existence of market risks and
the dynamic volatility of risks, losses may reach unbearable conditions for traders, so we set the
maximum loss tolerance ratio of traders to 10%. Once the loss may exceed this ratio, measures
should be taken to minimize the loss as much as possible.


WhereP 0 iis the price when asset i is purchased, andP 1 iis the price when asset i is sold. It can be
seen thatriis a random variable due to market uncertainty.

### 3.1 Model introduction

The online portfolio trading model of commissions, selects different capitals for trading, and
maximizes the expected return of the investment portfolio through online portfolio investment and
reduces unsystematic trading risks. Based on real-time price information, investors dynamically
adjust the proportion of the investment portfolio according to the selected investment strategy,
while taking into account the commission expenses. This investment method utilizes computer
programs for dynamic position investment adjustment, which simplifies the difficulty of strategy
implementation.

Markowitz pointed out that a trader’s propensity to trade is determined by his attitude towards
risky trades, the average return of the project and the risk of the trade, that is, a function of the
average return and standard deviation of the trade portfolio. The average rate of return and risk for
class i capital is:
Ri=E(ri) (3)
σi=E(ri−Ri)^2 (4)

### 3.2 Establishment of the portfolio trading model

Letxibe the investment ratio of traders investing in assets of type i (i=1 represents gold,
i=2 represents Bitcoin, the following definitions are the same as here), andCiis the transaction
commission ratio of assets of type i. We set the observation period of risk and return as 60 days,


then the 60-day return on the combined trading of gold and Bitcoin is:

Their 60-day trading risks are:

Of course, traders’ betting fees on gold and bitcoin should not exceed their own dollar holdings,
namely:


whereD 0 is the current trader’s dollar holdings. Then, we initially established the following
combined trading model considering commissions:

In order to fit the reality and simplify the calculation and solution, we introduce the risk
preference coefficient m to unify the benefits and risks, and use the weight distribution method to
convert the multi-objective planning into a single-objective planning. Then, the above model can
be optimized into the following form:

### 3.3 The solution of portfolio transaction planning model

As mentioned above, the volatility of the price market is actually a reflection of the traders’
past, present, and future will, which leads to a positive trend between returns and risks. Therefore,
it is difficult to achieve the maximum benefit and the minimum risk at the same time. In order
to fully consider the situation of traders’ assessment of market risks and comprehensive judgment
of returns, we adopt the weight coefficient conversion method and introduce the risk preference


coefficient m to measure traders’ comprehensive consideration of returns and risks. The weighted
objective function is as follows:


The multi-objective programming model is transformed into a single-objective programming model.
Obviously, there are quadratic terms in the planning at this time, so it is a quadratic programming
model. The objective function is a strictly convex function (it can be judged by calculating its
Hessian matrix positive definite), and the feasible region (the range of constraints) is a convex set
(it can be judged according to the definition of a convex set). Therefore, the solution of the objective
belongs to the convex programming problem, and we use the simplex method to solve it, so that
the objective function can be solved optimally in the corresponding time. (respectively, the capital
investment for the purchase of gold and Bitcoin obtained at this time)

## 4 Trading strategies based on Dow Theory

### 4.1 Overview of theoretical principles

Dow Theory is a trend theory that predicts future price movements based on past price trends.
The length of the reference period can be divided into long-term, medium-term and short-term
trends. If in the trend of the trading price, each period of increase can push the price to break
through the previous high, but the lowest point of the downward trend between these two highs is
still higher than the previous low, that is, the high and lows are higher than the previous ones, a
movement known as an uptrend. Similarly, a price action in which both the high and the low are
lower than the previous one is called a downtrend.

Due to the continuity of trends, trend reversals may occur on trends of smaller time lengths,
and Dow Theory does not deny that a mid-term trend change may signal a longer-term trend
change. However, if traders only make decisions based on short-term trends, it is easy to fall into
the predicament of contrarian trading, and the risk of loss is even greater. The core of Dow Theory
is that traders should trade when the medium-term trend and the long-term trend are consistent,
that is, when they are in the same trend. At other times, they should wait more patiently and look
for opportunities.

Because the conclusions drawn by Dow Theory lag behind changes in market prices, the
signals sent by the theory have a lag. Only choosing to enter and exit the market according to
Dow Theory will reduce a lot of profits and trading space, which may cause the loss of trading
opportunities. If the judgment interval is shortened and the timing of entry and exit is judged
through multiple criteria, this problem will be solved to a great extent.

### 4.2 Formation of trading strategy

**4.2.1 Principle of "Dual Time Period Confirmation"**

In order to deal with the shortcomings of the hysteresis of Dow Theory and increase the invest-
ment profit range, we have improved the Dow Theory. Adopt the "Dual Time Period Confirmation
Principle" to judge the long-term and mid-cycle price trends by analyzing the previous prices. Take
the long-term trend as the macro judgment direction and the medium-term trend as the trading


basis. When the long-term trend is the same as the medium-term trend, grasp the best time to buy
and sell.

Due to the existence of commissions, the transactions of gold and bitcoin should not be too
frequent. The change of gold transaction price is relatively stable, which is suitable for long-term
investment. The price of bitcoin changes drastically, which is suitable for short- and medium-term
investment. In view of the differences in capital attributes, trading trends, and price peaks of gold
and bitcoin, we select different observation periods for gold and bitcoin to judge the price trend, so
as to choose the best trading period.

**4.2.2 Trend judgment**

1. Selection of observation period
    For gold, we use 20 days as the long-term trend observation period and 5 days as the medium-
    term trend observation period. For Bitcoin, we use 10 days as the long-term trend observation
    period and 4 days as the medium-term trend observation period.
2. The method of judging the trend
    Taking the long-term trend judgment of gold trading as an example, the observation period
    of the long-term trend is 20 days, and the period of medium-term trend observation is 5 days.
    Take historical transaction price data for analysis, and take 5 days as a small set. Take the
    5-day average transaction price as the price reference data, and draw a line chart. Every 4
    points is a large set, and the highest and lowest points are taken to form Dow highs and Dow
    lows, and rising highs and lows are determined as long-term uptrends, and falling highs and
    lows are to determine that the long-term period is a downward trend. The trend diagram is
    shown in Figure 2. If the highs and lows change irregularly, the price changes drastically
    during this period, the risk is high, and it is not suitable to invest. The judgment of the
    mid-cycle trend disk is similar to that of the long-term cycle.

**4.2.3 Selection of transaction time**

After consolidating and evaluating the current phase of the mid- to long-term trend, all a
trader has to do is time the trade. We refer to buying gold or bitcoin as entering the market and the
opposite as leaving the market.


First of all, for the entry into the market, it is required that both the long-term trend and
the medium-term trend are downtrends, at which time the price continues to move lower. In a
medium-term trend, the first reversal point from a downtrend to an uptrend is the time to enter
the market. For example, we assume that the long-term trend at this time is declining, and the
medium-term trend is also declining. The Dow high point of the previous mid-term trend is a, then
when the price exceeds a, it means that the situation has reversed, and it is suitable to buy the asset
at this time.

For the case of leaving the market, it is also judged by the reversal point in the medium-term
trend. For example, we assume that the mid-term trend and the long-term trend are both uptrends
at this time, the Dow high of the mid-term trend gradually rises, and then the most recent Dow high
is broken, indicating that the situation has reversed, and the asset is sold at this time.

For cases where gold is not bought and sold on Saturdays and Sundays but Bitcoin can be
bought and sold on Saturdays and Sundays, we choose to round up the gold transaction price on
those days to the price of the previous trading day. In the actual calculation, we use the uncompleted
raw data of gold to calculate the medium and long period, monitor the price trend of the long period
and the medium period, and avoid trading on Saturday and Sunday.The schematic diagram of
transaction timing is shown in Figure 3.

When determining the investment ratio of the two assets, the gold transaction data of gold on
the day before Saturday and Sunday can be completed, and it can be regarded that the price does
not rise or fall on Saturday and Sunday, and the asset does not trade. Use the gold-completion price
table and the bitcoin historical price table as data reference, and use the online portfolio trading
model to determine the best investment ratio for each day.


**4.2.4 Dow Theory Algorithm Structure**


Timing judgment process for exit and entry is shown in Table 2.
Algorithm Principle of Dow Theory is shown in Table 4.

Table 2: Timing judgment process for exit and entry
Algorithm:Loss control algorithms
Input : Historical transaction data,intraday price, medium or long period length (multiple
relationship between them)
Output : Judging the timing of entry and exit
1. Calculate the average transaction price for each mid-period
2. Mark the highest and lowest points of the daily trading price in each mid-cycle, and
mark the rise and fall of the mid-cycle of the corresponding point. (marked since the
second mid-cycle)
3. **If** both the high and the low are marked as an increase or both are marked as a decline:
Mark the mid-cycle as a rise or fall (same trend as the marked point)
**Elif** The highest and lowest points are different marks:
4. Take the medium cycle from the first long cycle
**If** this medium period is the second medium period in the long period:
Mark the ups and downs of the previous mid-cycle as the state of this mid-cycle
Else
**For** : one day in a mid-cycle
**If** the transaction record of the previous mid-period is empty:
**if** the ups and downs of the last mid-cycle are up:skip today
elif the ups and downs of the last mid-cycle are down And yesterday’s ups and downs
were up:
Buy, and no longer trade in this mid-cycle
**Elif** the transaction record of the previous period is not empty:
Operates according to the Dow Theory algorithm
**For** mid-cycle in other long cycle:
**If** no previous transaction And The ups and downs of the last medium cycle are down
And yesterday’s ups and downs were up:
Buy and out of this long cycle
**Else**
The operation is performed according to the Dow Theory algorithm. If there is an
operation that jumps out of this long cycle, if there is no operation, it will enter the next
medium cycle.


### 4.3 Analysis of the effect and rationality of the strategy

1. The judgment result of the trend: The Figure 4 and Figure 5 show the long-term and medium-
    term trends of gold and Bitcoin (1 means up, -1 means down, 0 means irregular price changes)
       Taking the trend chart of gold as an example, in the two long cycles of A and B, the price
    trend of gold is rising. Therefore, we judge that the long-term C is also an upward trend. The
    D interval corresponds to the long period C, and it can be seen that the trend in the D interval
    is a continuous rise. Next, we take the gold price in the long period C and draw the Figure 6
    as follows: Some of the Dow highs and Dow lows in this period are marked in the figure, and
    it is not difficult to see that the trend in this long period is upward. Therefore, our prediction



Table 3: Algorithm Principle of Dow Theory
Algorithm: Dow Theory Algorithm
Input : The ups and downs of the last mid-cycle, yesterday’s ups and downs, whether the
last trade was entry or exit
Output : Whether or not to operate today and what to do
If: the ups and downs of the last medium cycle are down And yesterday’s ups and downs
were up and the last transaction was an entry
Buy, no more trading in this mid-cycle Elif: the ups and downs of the last medium cycle
are up And yesterday’s ups and downs were down and the last transaction was an exit
Sell today, no more trading in this mid-cycle
Else: :
No operation today



of the trend is reasonable. Of course, there will be instances of misjudgment, which are
normal, as no prediction theory is 100% perfect. Even if the judgment is biased, losses may
occur in the short term, but long-term investment has a great probability of achieving better
profits.


2. rationality Analysis


Judging from the price trends of gold and bitcoin, whether from a long-term or short-term
perspective, the distribution of gold is relatively uniform, while the distribution of bitcoin
is very dense, basically showing a cluster distribution. This shows that the price volatility
of gold is more stable than that of Bitcoin. Therefore, gold is more suitable for long-term
trading, and bitcoin is more suitable for medium-term trading.
Table 4 lists some trading time judgment tables. According to the data in the table, it can be
seen that according to this investment strategy, it is generally a good trading plan to buy at
low points and sell at high points. Even on non-trading days for gold (gold is not open on the
day corresponding to #), the combination trade is proceeding normally.


## 5 Overall trading plan

### 5.1 Division of trading periods


A schematic diagram of the transaction date division is shown in Figure 7.

- Observation period - accumulating data
    At the beginning of the transaction, the lack of historical price data, the long-term trend and
    the medium-term trend cannot be judged. In order to avoid the blindness of transactions,
    we choose not to conduct transactions for the time being and see the direction of market
    transactions. The observation time is 20 days, so the market transaction prices of the
    previous 20 days are accumulated as data.
- Enture capital period - test the market
    After data accumulation, we can already predict the medium-term trend based on the obtained
    data. Only relying on the mid-cycle trend to guide the investment is risky, and it is easy to
    miss the best trading time. Although it will not achieve the maximum profit, it has a high
    probability of making a profit. At this point, we start investing (belonging to testing the
    market), predicting the mid-cycle Dow trend, monitoring the daily market prices of Bitcoin


and gold, and using the Dow theory trading strategy to capture the right time to buy and sell.
At the same time, monitor the long-term cycle. When the long-term market trend can be
judged, the investment plan will gradually mature and enter the investment income period.


- Investment income period - the model guides the investment and realizes the income
    With the further deepening of investment, historical transaction data gradually increased, and
    the grasp and judgment of long-term trends became more accurate.

### 5.2 Confirmation of the transaction plan


The investment strategy process is shown in Figure 8.The details are as follows:


1. According to the degree of personal risk preference, select the risk preference coefficient 0-1.
    Here, we take the risk preference degree of 0.6 as an example. The larger the value of the
    risk preference coefficient, the greater the investment risk value.
2. Monitor prices in real time, make a judgment every day based on the improved Dow in-
    vestment strategy, and make a decision whether it is suitable to buy or sell gold or Bitcoin
3. When a suitable transaction date is found, it will be based on the online portfolio investment
    strategy with transaction fees. then calculate the investment ratio, and get the optimal
    investment ratio (x 1 ,x 2 ).
4. Invest according to the investment ratio, assuming that it is suitable to buy gold today. At
    this time, the dollar amount is D, then buy D*x1 gold, The next amount is used to prepare
    for investing in Bitcoin and risk control.
5. Today’s trading ends, enter the next day’s observation.

### 5.3 Analysis of transaction results

According to this investment strategy, an investment plan for the five-year trading period from
September 11, 2016 to September 10, 2021 is formulated.Figure 9 and Figure 10 is its schematic
diagram .(Gold is bought in troy ounces ,the unit of bitcoin buying is bitcoin, and the selling
proceeds are all converted into US dollars)

In the portfolio investment strategy, observe Figure 10 and compare the purchase and sale
records of gold and Bitcoin. It can be seen that before 2018 , there were many transactions in
gold, with gold as the main investment object. After 2018 , for the increase in bitcoin transactions,


bitcoin has become the main investment object. As can be seen from Figure 10, before 2018 ,
by investing in gold, the price difference between gold purchases and sales was small, and stable
income was obtained. After 2018, The price difference between when Bitcoin is bought in 2020
and when it is sold in 2021 is huge. Assets will soar at the intersection of 2020 and 2021. The
portfolio investment strategy has seized the moment of the Bitcoin price surge while constantly
trading gold to ensure investment stability, achieving huge gains at a small cost.

## 6 Analysis of model advantages

In trading of the financial markets, there may be a variety of situations that need to be taken
into account. We comprehensively expound its rationality and superiority from the perspective of
model solving process and results of profitability, risk, Sharpe ratio and liquidity.

### 6.1 Profitability perspective

We get the trading strategy and income of only trading gold are Figure 12: (buy at the red dot,
sell at the blue dot)

The Figure 13 shows that only buying Bitcoin has the highest return, only gold has the lowest
return, and the case of combination trading is in the middle. Therefore, purely from the perspective
of profitability, the portfolio trading model is only better. However, the combination trading model
ensures the stability of the transaction. In the early stage of Bitcoin trading, there is a large decline
in the early stage, and there is also a large decline in the later stage of the transaction. At this time,
the price of gold rose steadily. It is a good time to enter gold, and the combination trading model
has seized this opportunity to ensure the controllable risks while maintaining high returns.

### 6.2 Risk perspective

**6.2.1 Control of the maximum loss amount**

In the process of formulating the trading strategy, the maximum loss limit is introduced, and
we set the maximum loss ratio that investors can bear for a single investment to be 10%. In each
day’s price monitoring, the price of the day is compared with the purchase price of the asset. When
the decline exceeds 10%, in order to avoid the abnormal collapse of the asset system, the asset will

be sold immediately and the loss will be stopped in time.The Figure 14 shows the change curve of
return with risk appetite coefficient.

**6.2.2 Free risk control**

By comparison, when m<0.4, the investment is relatively conservative and the benefit is small;
when 0.4<m<0.6, the risk is moderate and the benefit is the largest; when m>0.6, the risk increases,
the probability of investment loss increases, and the benefit decreases. Due to the continuous
soaring price of Bitcoin in recent years, risk lovers are more inclined to invest in Bitcoin in order
to obtain greater returns.

### 6.3 Risk-return composite indicator - Sharpe ratio

The Sharpe ratio is a classic indicator that takes both return and risk into account. It means
that investors can get a little bit of excess return for every extra risk they take; if it is positive, it
means that the underlying return is higher than the risk of volatility; if it is negative value, which
means that the underlying operational risk is higher than the rate of return. It represents the ratio
of investment return to risk taking, the higher the ratio, the better the portfolio.



In the formula,rpis the expected annualized rate of return of the portfolio,rfis the annualized
return of risk-free assets (there is no risk-free asset in this paper), andσqisthe standard deviation
of the annualized rate of return of the investment portfolio. In our investment selection, risk-free
investment is not included, that is,rf= 0.

By calculating the annual Sharpe coefficients of the four investment strategies of only investing
in gold, only investing in Bitcoin, gold and Bitcoin with a constant proportion of investment, and
online portfolio trading investment, the advantages and disadvantages of the four trading strategies
are compared and analyzed.

The Table 5 is the Sharpe coefficients of the four portfolios: According to the table below,
among the four investment strategies, online portfolio investment has the best performance in terms
of Sharpe coefficients, all of which are positive, and its Sharpe coefficient values are higher than
those of the other three.This kind of investment strategy is relatively large. In the case of considering
both the return and the risk, the online portfolio investment strategy can effectively control the risk
and improve the return. The table below shows the calculated Sharpe coefficients:

### 6.4 Liquidity perspective

According to the Table 6, the number of transactions of portfolio investment is 5 less than
that of only investing in gold, but the income is 9 times that of only investing in gold. Although
the return of portfolio investment is no better than that of only investing in gold, the number of
transactions is less than 10 times, which effectively avoids some risks in the process of Bitcoin
investment, so that the return has been in a relatively stable upward state.

By analyzing the investment results under the guidance of the online portfolio investment
model, this investment strategy has high profitability, and the risk preference coefficient is selected
as 0.6, which is a risk preference investment type. At the same time, the maximum loss amount can
be effectively controlled, the Sharpe coefficient has the best performance, and the capital liquidity
is relatively high. The above investment indicators are considered in a comprehensive comparison,
and the investment model is the optimal model.


## 7 Sensitivity of the strategy to transaction costs

1. Sensitivity of the strategy to transaction costs In a transaction, whether it is a profit or a loss,
    a corresponding proportion of the commission needs to be paid. Therefore, the commission
    value is a data that affects the transaction for a long time, and the higher the commission is,
    the profit of the trader will definitely decrease. However, due to the differences in the price
    fluctuations of gold and bitcoin markets, different commission ratios have different effects
    on them. In the title, the commissions for gold and bitcoin are 1% and 2% of the transaction
    amount, respectively. In order to analyze the influence of commission on the model, we take
    the commission ratio of gold to be approximately 0, 1%, 5%, 10%, and the commission ratio
    of Bitcoin to be approximately 0, 1%, 5%, 10%. According to the model established above,
    we solve these different commission ratios respectively, and obtain the corresponding final
    income values under the combination of these commission ratios, as shown in the Table 6 (the
    horizontal axis represents the change of gold commission, and the vertical axis represents
    the bit Coin commission changes): According to the analysis in 7, when the commission


ratio of Bitcoin remains unchanged and the commission ratio of gold increases, the final
income will drop rapidly; but when the commission ratio of gold remains unchanged and
the commission ratio of Bitcoin increases, the final income changes are not as drastic as in
the previous case. This makes perfect sense, since Bitcoin sometimes trades at extremely
high prices and the market price fluctuates so much that commissions affect it less than gold.
When the commissions of both are close to 0, the final profit price is nearly five thousand
dollars higher than the scenario set, and when the commissions of both are close to 10%, the
final profit will become very small, which also conforms to the general law that the income
changes with the commission ratio.
In conclusion, the commission ratio has a far greater impact on gold trading than on Bitcoin,
and has a huge impact on the returns of the portfolio investment model.


2. The impact of transaction costs on the model
    Figure 15 is a schematic diagram of the impact of transaction costs on investment strategies.In
    the model we established, transaction cost mainly affects the investment ratio and the actual
    amount of investment. In the determination of the investment ratio, an online portfolio
    investment model with transaction costs is used to construct a multi-objective programming
    algorithm. The transaction costs affect the constraints and the coefficients of the objective
    function. When the transaction cost changes, the rate of return changes. In order to achieve
    the maximum return and the minimum risk, the investment ratio is adjusted to find a new
    optimal solution.


As transaction fees increase, the amount actually used to purchase investment products will
decrease, and the return on a single investment will also decrease. When transaction costs
increase, the number of transactions will decrease accordingly. The decrease in the number
of transactions will have an uncertain impact on profitability, and investment risks will also
increase.

## 8 Model advantages and disadvantages and optimization solutions

## tions

1. Model Advantages The model makes full use of historical transaction data. First, the investor
    chooses the risk preference coefficient ( 0 < m < 1 ), and obtains the optimal solution of the
    investment ratio (x 1 ,x 2 ) through the online portfolio investment strategy including transaction
    costs. Combined with the classic Dow Theory in ,the investment market, this paper analyzes
    the shortcomings and deficiencies of the Dow Theory, and adopts the principle of "Dual Time
    Period Confirmation" to improve the Dow Theory. Successfully seized the best time to buy
    or sell an asset.
2. Disadvantages of the model The early investment judgment of the model is only based on the
    medium-term trend, and the Dow theory is used to monitor the timing of buying or selling
    assets. There is a certain lag in the grasp of the transaction date, resulting in less profit from
    the early investment. Only when the time exceeds 60 days, the long-term trend is gradually
    clear, and the investment enters the investment income period, it will have good returns.
3. Model optimization direction
    - Select a more suitable observation period. The difference of observation period has
       certain influence on the selection of transaction date. The shorter the observation
       period, the more sensitive it is to market price changes, the more transactions are
       recommended, and the greater the risk. The longer the observation period, the more
       likely it is to miss many suitable trading dates, resulting in reduced returns. Therefore,
       according to different assets, exploring an observation period that conforms to the law
       of market changes will allow you to have a better grasp of the timing of transactions
       and achieve greater profits and less risk.

- Selection of risk preference coefficient. The risk preference coefficient directly deter-
    mines the choice of investment ratio. Risk-loving people are keen to invest in Bitcoin,
    and risk-averse people are keen to invest in gold. Therefore, the choice of risk ap-
    petite coefficient plays a key role in investment decisions. In different trading periods,
    there may be different optimal risk factors. Therefore, the dynamic selection of risk
    coefficients according to market laws will help to achieve stable returns.
- Optimization algorithm structure. By optimizing the algorithm structure, conducting
    more index analysis on asset transaction prices, conducting joint analysis of different
    assets, and exploring their internal correlations, we will be able to grasp the market
    trend better, make more accurate investments, and obtain greater returns.

## 9 Memorandum

The formulation of our investment strategy is mainly the selection of the best investment ratio
and the grasp of the best trading date. The investment ratio and transaction date are based on the
analysis of historical transaction data. With the passage of time, the model is in a dynamic change
and has strong universality.

The models used in our investment strategies are mainly: online portfolio investment models
with transaction fees and trading strategies based on Dow Theory.

The online portfolio investment model is used to find the best investment ratio allocation in
real time. By analyzing the income and risk of the transaction data of the first 60 days, the multi-
objective planning model is used to pursue the maximum return and the minimum risk, and the risk
preference coefficient is introduced to integrate the risk and return according to the unification of
weights. It is transformed into a single-objective programming problem, and the simplex method
is used to find the optimal solution of the investment ratio.

Trading strategies based on Dow Theory are used to capture the best trading opportunities.
Through the detection and trend analysis of historical transaction data, as well as the monitoring
of the long-term and medium-term trends of Bitcoin and gold, the Dow highs, Dow lows and trend
trends can be judged, and combined with "dual time cycle confirmation" principles to determine
the best date to buy or sell.

The investment strategy is: using the transaction model based on Dow Theory, if a suitable
purchase opportunity is found, the online portfolio transaction model with transaction fees is used
to calculate the optimal investment allocation ratio at this time, and purchase the corresponding
assets proportionally. If it is judged that it is suitable to buy gold on that day, the investment ratio
is calculated as , and the dollar holdings are D. So D* is used to purchase gold, and the remaining
amount will not be invested for the time being to prepare for Bitcoin investment and risk control.
Using our investment model to guide traders to invest, traders only need to choose their preferred


risk preference coefficient ( 0 < m <1)). The larger the risk preference coefficient, the greater the
risk. We choose the risk preference degree as 0.6 to guided investments. After that, traders only
need to input the price data of gold and bitcoin in the past, and the model will output whether it is
recommended to trade today, the trading behavior, the type of trading assets, and the best trading
ratio. Traders make buy, hold, and sell operations based on model recommendations.

On Saturdays and Sundays, the price of gold can be regarded as unchanged and without profit.
Based on our Dow Theory model, we will not choose a day without profit for trading, but continue
to monitor the price trend of Bitcoin to determine whether it is suitable for trading. It effectively
solves the problem that the gold market does not open on Saturdays and Sundays, and Bitcoin can
be traded every day.

Using our investment model to guide traders to invest, traders only need to choose their
preferred risk preference coefficient ( 0 < m <1)). The larger the risk preference coefficient, the
greater the risk. We choose the risk preference degree as 0.6 to guided investments. After that,
traders only need to input the price data of gold and bitcoin in the past, and the model will output
whether it is recommended to trade today, the trading behavior, the type of trading assets, and the
best trading ratio. Traders make buy, hold, and sell operations based on model recommendations.

Guided by our model, the asset increased from $1,000 on September 11, 2016 to $72,087.61
on September 10, 2021. The investment rate of return is 7108.7%, the total number of transactions
is 16, the maximum loss ratio of a single investment does not exceed 10%, the average Sharpe
coefficient is 0.152, and the Sharpe coefficient is positive every year. The investment has good
profitability, and at the same time, it realizes a good control of risks, realizes the optimal combination
of income and risk, and achieves good investment results.

By calculating the five-year investment returns under different commission ratios, and com-
paring the return and risk data of the investment, we analyze the sensitivity of the investment model
to transaction costs. Transaction fees have a large impact on the expected return of the model.
Transaction costs mainly affect the investment ratio. With the increase of transaction costs, due to
the relatively stable change of gold price, the income of investing in gold decreases rapidly, so the
proportion of gold investment drops sharply. Due to the rapid price rise of Bitcoin in recent years,
the profitability is high. So the investment ratio is less affected by transaction fees.

## References

[1] Li Shuli.Research on Markowitz Portfolio Models of Mean and Variance ChangesEconomic
[J].Outlook of the Bohai Rim,2020(2),185-186

[2] TM. Universal Portfolios[J]. Mathmatical Finance, 1991,1(1):1-29

[3] Arpe W F. The Sharpe ratio Journalof Portfolio Management,1994,21(1) : 49-58

[4] Liu Bing, Zheng Chengli. Review and application of several online portfolio investment
strategies [J]. Journal of Shandong Agricultural Engineering Institute, 2020, 37(03): 63-69.
DOI: 10.15948/j.cnki.37-1500/ s.2020.03.011.

[5] Li Chulin. Research on portfolio investment considering transaction costs [J]. Forecast, 1998,

[6] Zhang Dingyuan. The establishment, evaluation and improvement of spot gold trading strategy
[D]. Xinjiang University of Finance and Economics, 2015.

[7] Fu Zhong. Defects of Dow Theory and Coping Methods [J]. Economic Research Guide,
2013(26):18-19.

[8] Chen Huayou, Xu Yisheng. Multi-objective planning model of portfolio investment with
transaction costs [J]. Operations Research and Management, 1999(03):57-60.

[9] Liu Shancun, Qiu Wanhua, Wang Shouyang. Pan-portfolio investment strategy with transac-
tion costs [J]. System Engineering Theory and Practice, 2003(01):22-25

[10] Lin Hongmei, Du Jinyan, Zhang Shaodong. Sharpe Ratio: Estimation Method, Ap-
plicability and Empirical Analysis [J]. Journal of Statistics, 2021, 2(06): 73-88. DOI:
10.19820/j.cnki.issn2096-7411.2021. 06.006.

[11] Deng Hongwu, Xing Kai, Wang Zhiyong, et al. An optimization method for model general-
ization ability based on time-invariant stability and Sharpe ratio [J]. Small Microcomputer
System, 2021: 1-12

[12] Song Hongyu. The application of Sharpe ratio in investment management [J]. Statistics and
Decision, 2006 (24): 107-109

[13] Operations Research Writing Group. Operations Research [M]. Beijing: Tsinghua University
Press, 1990, 2nd edition



