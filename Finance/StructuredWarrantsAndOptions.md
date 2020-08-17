# Structured Warrants and Options - SGX SIP Online Education Module

Structured Warrants and Options give the holder the right to buy or sell the underlying asset at a pre-determined specified price (the exercise price or strike price) on or by a specified date (the expiry or exercise date)

### Different types
* Call warrant/option: The right to buy the underlying asset. It will rise in value when the price of the underlying asset increases
* Put warrant/option: The right to sell the underlying asset. It will rise in value when the price of the underlying asset decreases

### Exercise price / Strike price
* This is the pre-determined price, which is fixed in advance before the structured warrant or option is listed. It is a price at which the holder of the structured warrant or option can buy (for a call warrant or option) or sell (for a put warrant or option) the underlying asset.

### Exercise styles
* European style: The warrant/option is a contract that may only be exercised on the expiry/exercise date (most warrants currently listed on SGX are European style)
* American style: The warrant/option is a contract that may be exercised on any trading day on or before the expiry/exercise date

### Expiry date or exercise date
* The expiry date or exercise date is the final date of the product’s life. After expiry, structured warrants and options no longer have value and will not be tradable anymore as the right to buy or sell no longer exists

### Conversion Ratio
* Conversion ratios are only applicable to structured warrants and are also known as ‘entitlement ratios’. It is the number of structured warrants that a holder will need to hold to have the right to buy (or sell) one unit of the underlying asset.
* For example, a structured warrant with a conversion ratio of 5:1 means that 5 structured warrants are needed to convert to 1 unit of the underlying asset.

The **intrinsic value** of the structured warrant or option is the difference between the strike price and the price of the underlying asset.

**Time value** is the amount of premium above the intrinsic value. It is the portion of a warrant/option’s value that is attributable to the amount of time remaining until the warrant/option expires. It decreases over the life of the warrant/option and is 0 on expiry date.

Value of a structured warrant = Intrinsic Value + Time Value
Intrinsic Value:
* Call: (Underlying Asset Price - Strike Price) or 0, whichever is greater
* Put: (Strike Price - Underlying Asset Price) or 0, whichever is greater

### Moneyness
You may have heard that a structured warrant can be “In-The-Money” or “Out-of-the-Money”. This refers to “Moneyness”.

“Moneyness” is determined by whether the price of the underlying asset is higher or lower than the exercise price or strike price of the structured warrant.

For a call warrant:
* If the strike price < the underlying asset price, that call warrant will be “In-The-Money”. Intrinsic value = positive
* If the strike price > the underlying asset price, it will be “Out-Of-The-Money”. IV = No value
* If the strike price = the underlying asset price, it will be “At-The-Money”. IV = No value

For a put warrant, the opposite is true:
* SP > AP => “In-The-Money”. IV = positive
* SP < AP => “Out-Of-The-Money”. IV = no value
* SP == AP => “At-The-Money”. IV = no value

### Factors which Influence Warrant Price
#### Current Price of the Underlying Asset
* An appreciation in the underlying asset will cause the call warrant price to rise as the probability that the warrant will expire In-The-Money has increased, while the put warrant price will fall as its probability to expire “In-The-Money” has decreased.
* In contrast, a weakening in the price of the underlying asset will attribute to an increase in the price of the put warrant but a decrease in the price of the call warrant.

#### Exercise Price of the Structured Warrant
A call warrant with a high exercise price has a lower probability of expiring In-The-Money while a lower exercise price will increase that probability. Hence, a call warrant on the same underlying asset and the same expiry date but with a higher exercise price will be priced cheaper than one with a lower exercise price.

A high exercise price will increase the probability of a put warrant expiring In-The-Money while a low exercise price will reduce that probability.

#### Implied or Perceived Future Volatility of the Underlying Asset
Implied volatility is the market’s expectation of the underlying asset price fluctuation over the remaining life space of the warrant. The higher the implied warrant, the wider the expected price fluctuation and hence, the higher the chance that the warrant will expire In-The-Money.
In addition, a higher implied volatility also attributes higher hedging risks to structured warrant issuers. Hence, warrants with high implied volatility will be priced higher for both call and put warrants.

#### Lifespan of the Structured Warrant
If the time to expiry for a structured warrant is longer, it will have more time for the structured warrant to move in the direction favourable to the structured warrant holder. Hence, the price of a structured warrant is higher if it has a longer time to expiry.

Warrants and options are closely linked to stocks, and so investors sometimes forget that warrants and options have expiry dates. They have no value after expiry.

### Structured Warrants vs. Options
* Issuer and Market
    * Structured Warrant: The issuer is usually a financial institution like an investment bank; traded in the securities market
    * Options: Developed by an exchange; traded in the derivatives market
* Trading Mechanism
    * SW: Investors can only be buyers (no writing of positions)
        * Positive view: investors can buy a call warrant
        * Negative view: investors can buy a put warrant
    * Options:
        * Writing a call or put options means that you are selling a call or put option. If you sell a call then you are obliged to sell the underlying asset at the strike price at a future date. If you sell a put then you are obliged to buy the underlying asset at the strike price at a future date.
        * Positive view: Investors can buy a call option
        * Negative view: Investors can buy a put option
* Product Features
    * SW: Wide range of exercise prices and expiry dates which are determined by the issuer
    * Options: Contracts are standardised with limited expiry dates and exercise prices
* Maximum liability/maximum loss
    * SW: Losses capped at total investment sum and transaction fees
    * Options: Potentially unlimited liability/losses for writing an option (short selling)
* Margin Requirements
    * SW: None
    * Options: Applicable to an option writer but not an option buyer
* Liquidity
    * SW: A designated market maker (DMM) is required under Exchange Rules to provide competitive bids and offer quotations on a continuous basis
    * Options: Only commercial market makers (CMMs) provide liquidity. A CMM is not regulated under Exchange Rules but will enjoy incentives if they meet certain obligations in terms of providing liquidity and bid-ask quotation)

### Decipher the Name of a Structured Warrant
* ABC XXX eCW180226A
    * Name of underlying asset; Issuer; ‘e’ - European/Americal style; CW/PW: call or put warrant; Expiry date YYMMDD; Last letter distinguishes between multiple warrants issued under same underlying and expiry date
* ABC#####XXXeCW171228
    * Underlying asset; exercise price; issuer; European/American; CW/PW; Expiry date

### Risks
Investors should read the relevant listing docs for full list of risks involved in trading structured warrants/options.
* Market Risks
    * Market price affected by many factors including but not limited to, the level of volatility of the underlying asset, time remaining to expiry and interest rate
    * Loss of entire investment: If the structured warrant/option expires At-The-Money or Out-Of-The-Money, investors holding on to these products will lose their entire investment capital
* Product Risks
    * Leverage Risk
        * As structured warrants/options are leveraged instruments, gains and losses will be magnified. Movement in the underlying asset’s price will have a greater percentage impact on the price of the structured warrant/option
    * Trading may be suspended or halted if the underlying stock is suspended or halted
* Liquidity Risks
    * The secondary market may be illiquid: The DMM appointed for structured warrants may be the only market participant buying and selling the warrants. Hence, the investor may not be able to realise the value of the warrants. Hence, the investor may not be able to realise the value of the warrants. Do note that the bid-ask spread increases with illiquidity.
* Issuer Risks
    * Exposed to credit risk of issuer
        * Structured warrant holders are unsecured creditors of the issuer. They have no preferential claim to any assets that an issuer may hold in the event the issuer is unable to fulfil its obligations

---
