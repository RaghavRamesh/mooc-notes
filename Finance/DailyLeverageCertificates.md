# Daily Leverage Certificates (DLCs) - SGX Online SIP Module

DLCs offer fixed leverage on the daily performance of the underlying asset (e.g. market indices or single stocks). For e.g., if the underlying asset moves by 1% from its closing price of the previous trading day, the value of a 3x DLCs will move by 3% and that of 7x DLCs will move by 7%.

DLCs are designed to be traded over short periods of time, predominantly on an intra-day basis. There are 2 types of DLCs:
1. Long DLC - Rises in value as the underlying asset rises
2. Short DLC - Rises in value as the underlying asset falls

### The Effects of Compounding
If your trading horizon is over a few days, it is important to note that the performance of the DLC may vary from the the leverage factor of the DLC. This is because the performance of the underlying asset and the DLC is reset at the end of each trading day.

When markets open the next day, the performance of the underlying asset and the DLC will be measured from the closing levels recorded on the previous trading day. What this means is the performance each day is locked in and any subsequent returns are based on what was achieved the day before. Over the period of more than one day, the profits or losses are thus compounded.

The effects of compounding can work for you or against you. Therefore as an investor, it is important for you to understand the effects of compounding.

#### Compounding in a Clear Upward Trend
If an investor holds a long position for a week and the underlying asset registers 5 consecutive days of gains, the 3x Long DLC would in fact return more than 3 times the performance of the underlying asset. This is due to the daily reset of the leverage factor.

#### Compounding in a Clear Downward Trend
If an investor holds a 3x Long DLC for a week and the underlying asset registers 5 consecutive days of losses, the 3x Long DLC would fall less than 3 time the performance of the underlying asset. Each day the loss is calculated based on a progressively smaller amount.

#### Compounding in a Sideways Trend
Compounding effect of a DLC may not provide favourable returns when the price of the underlying asset moves in a sideways pattern. This is why these products are not designed to be traded where the market is moving sideways or to be held for long periods of time

### The Airbag Mechanism
An airbag mechanism is built into the DLC to slow the rate of loss in the value of the DLC in extreme market conditions. Each DLC listed will have a pre-set trigger for the airbag mechanism. For e.g., for a 7x DLC, the trigger is usually activated upon a 10% movement in the underlying asset (based on the underlying asset’s closing price in the previous trading day).

The performance of the DLC after the suspension will be based on the New Observed Level instead of the previous day close. The New Observed Level is determined as the lowest value of the underlying asset for the Long DLC or the highest value of the underlying asset for the Short tDLC during the 15 mins period after the airbag mechanism is triggered.

If the airbag mechanism has been triggered and the underlying asset were to continue falling, losses to the DLC would be smaller than without airbag activation.

However, if the underlying asset were to rebound, the airbag mechanism would reduce the investor’s ability to recoup his or her losses as any subsequent gains are now based on this New Observed Level which is a lower value of the underlying asset as compared to the previous day’s closing price.

The trigger levels for each DLC will vary depending on the leverage factor and underlying asset.

The airbag mechanism will only be triggered upon movements of the underlying asset that goes **against** the direction of the product. In this case, James and Grace hold short DLCs benefiting from a market decline and thus, the airbag mechanism will not be triggered.

### Risks
#### Investor may lose the entire investment
In the event of a strong trending market or an adverse market movement, the underlying asset may fall to levels such that the value of the DLC is calculated to be less than or equal to 0. If the value of a DLC reaches 0/becomes worthless, the issuer may request that the DLC be suspended and subsequently apply for the to be de-listed.

#### Leveraged Risks
Losses will be amplified depending on the particular leverage. Consequently, the investor could lose more than he would if he invested directly in the underlying asset.

#### Compounding Effect
Gains and losses are compounded over periods of more than 1 trading day, and as such will deviate from the leveraged performance of the underlying index. This difference maybe amplified in a volatile market with a sideways trend, where market movements are not clear in direction, and could generate significant losses for investors.

### Naming convention
#### Index DLC
* DLC SG5xShortHSI 200714
    * DLC; Issuer; Leverage factor; Long/Short; Underlying index; Expiry date in YYMMDD
#### Single Stock DLC
* DLC SG5xShortDBS A
    * DLC; Issuer; Leverage factor; Long/Short; Underlying stock; Serial number for additional issues by the same issuer with the same underlying asset, director and leverage factor
    * Note: the counter name for a single stock DLC will not include the expiry date. However, similar to other DLCs, the DLC still has an expiry of approx. 3 years.

