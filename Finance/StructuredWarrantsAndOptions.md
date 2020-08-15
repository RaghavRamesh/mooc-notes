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
_to be continued__

