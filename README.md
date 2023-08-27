# fantasy_sports

**How to win at PrizePicks**

Oct 29, 2021
For the uninitiated, PrizePicks is a sports betting parlay site where you bet anywhere from 2 to 5 leg parlays, mostly on player props. You can bet a “power play” where you need to get all your bets right, or a “flex play” where you get some money back if you miss on a leg or two.
Payouts are typically worse than parlays at traditional sportsbooks (e.g. 3-to-1 on a 2-leg parlay, 5-to-1 on a 3-leg parlay, etc.) and you will go broke quickly if you make breakeven bets.
 
Nevertheless, the game can be beat easily for the following reasons:
1. Player props themselves are relatively easy to beat, much easier than more prominent spreads and totals.
2. PrizePicks doesn’t move their lines quickly (although they have gotten better recently)
3. They allow you to bet on correlated parlays (e.g. QB with his WR) and don’t change the payouts*
4. When you combine multiple positive expected value (EV) bets into a parlay you get even more EV, akin to leverage, as Ed Miller and Matthew Davidow explain in their excellent book
…parlays are not in any way sucker bets. They’re just a way to increase betting volume. And if you have some winning bets you’d like to make, a sneaky way to increase volume can be very handy.
5. The 5-leg flex play appears to be mispriced
On that last point, I’ve seen a lot of commentary directing people to bet power plays since they are higher EV than the flex plays. This is bad advice. While it is certainly true that the 2/3/4 leg power plays are higher EV than their flex play counterparts, the best bet is the 5-leg flex play, and it isn’t close. Let me show you.
The probability of each parlay winning can be modeled using the binomial distribution. Essentially what you want to calculate is “if I make 5 independent bets, what are the chances that I will win exactly 3 of them, or exactly 4 of them, or all 5 of them.”
•	The payout for power plays is fairly straightforward: the probability of hitting n legs, each having p probability of hitting, is simply p^n.
•	The math for the flex plays is slightly more complicated, but can be calculated using the binmo.dist function in Excel, or on many an online calculator.
I have calculated this for each bet type. Results are in this google sheet. When you plot ROI by the probability of any one leg of your parlay winning, you can plainly see how dominant the 5-leg flex play is.
 
Notice the curvature of the Flex 5 line and how quickly it separates from the pack — This is due to the “parlays give you leverage” concept stated above
Not only is the 5-leg flex play the highest ROI for every point above 50% (again, 50% in this case means that each leg of your parlay is 50% to win on its own), it breaks even at a lower percentage, about 54% vs. 56% for the 4 leg power play, AND it has lower variance due to getting money back when you miss 1 or 2 of your 5 bets. And missing 1 or 2 of your 5 bets will happen A LOT.
Note that these probabilities assume independence. If you are making correlated bets — which you should be on PrizePicks — the ROIs are even more favorable than shown
Let’s hope they don’t wise up anytime soon.
*Always verify payouts when betting. Occasionally PrizePicks will decrease your payout due to positive correlation, but they usually don’t.
P.S. Note that I’m not showing you how to find +EV bets. But downloading projections from your favorite site and comparing is a good start.


**How to win at PrizePicks — Part 2**

Aug 16, 2023
** Note: This is an extension of a previous article. It is strongly recommended that you familiarize yourself with that piece first. **
Since last writing about them almost two years ago, Prizepicks has exploded in popularity. Stories abound about how much casual bettors love the site and other sites like them (e.g. Underdog), and their dubious logic of being legally declared fantasy sports and definitely totally not sports betting.
This year they also introduced the six-leg parlay as a betting option as well as removed the 2-leg flex.
Let’s go ahead and update the main graph from the previous article to reflect these changes. As a reminder, this shows your ROI achieved on the vertical axis by the various wager types assuming each leg of your parlay wins at the probability shown on the horizontal axis, and each bet is independent from one another.
 
Google Sheet with calculations
If you are familiar with the math of parlays you could have correctly guessed that the 6-leg Flex is now the best wager on the site. However, anyone who is remotely limited on Prizepicks is not able to bet nearly as much on a 6-leg parlay compared to a 5-leg parlay (due to limits on the amount you can win). Therefore in practice you are usually better off still playing the 5-leg Flex.
Correlation
One topic touched upon last time was correlation. I mentioned that it is in the user’s benefit to correlate your plays since Prizepicks does not change the payout structure. Up until this point, however, that statement was made purely on theoretical grounds. Of course we know that correlating is +EV, but by exactly how much? And when we correlate, is it possible the optimal wager type changes from a 5-leg Flex to a 4-leg Power Play? (My intuition has always told me yes). Well, I finally got down to running the numbers to answer those question.
First, a word on the process. Like before, I assumed a base win probability for one leg of a parlay. This time, instead of assuming each bet was independent however, I assumed the user would enter pairs of bets that were correlated together (e.g. QB-WR on the same team, two batters on the same MLB team, etc.). Therefore I needed to adjust the probability of winning the second bet of each “correlated pair” depending on if the previous bet won or lost.
The process was as follows:
1.) Assume a base case win probability, say 56%
2.) Assume a correlation adjustment for how much the subsequent bet increases or decreases in probability if the previous bet won or lost. For example, a factor of 10% would mean that if the previous bet won, the next bet increases in win probability ten percentage points from 56% to 66%, and likewise if the previous bet lost the next bet decreases from 56% to 46% probability.
3.) Simulate 100,000 binomial distributed random variables using the assumed base case win probability as the probability of success to represent the first bet in each “correlated pair.”
4.) Simulate 100,000 uniform random numbers to represent the second bet in each “correlated pair”
5.) Match the two vectors to each other. If the first bet won, declare the second bet a winner if its random number is below the base probability plus the correlation adjustment (in this example .56 + .10 = .66). If the first bet lost, only declare the second bet a winner if its random number is below the base probability *minus* the correlation adjustment (in this example .56-.10 = .46)
6.) If needed, repeat steps 3–5 for another “correlated pair”
7.) If needed, add in another leg to the parlay that is uncorrelated. For example, a 5-leg flex would be comprised of two correlated pairs and one uncorrelated bet.
8.) Calculate your ROI: the probabilities of all legs of the parlay winning in the case of power plays, all legs and some legs winning in the case of flex plays, and match to the corresponding payoff that PrizePicks offers.
9.) Repeat all steps for various base case win probabilities and correlation adjustments
The below graph summarizes the ROIs of each bet type at a 10% correlation.
 
A few takeaways:
•	As seen above, the Flex 6 dominates
•	The Flex 5 and Power 4 are now nearly identical at all win probabilities
•	Compared to the first chart, breakeven rates are higher. If you are betting flex 5s and power 4s, you only need to win at about ~52% to make money (was about 56–57% when no correlation). If you are betting flex 6s, you can more or less throw darts (i.e. bet at 50%) and you will still be OK.
•	ROIs get really positive, really quickly. If you can pick 56% winners (easily doable in player props), you can achieve a 34% ROI betting flex 5s or power 4s. If you can pick 58% winners, you can achieve a 53% ROI.
Of course the above conclusions are dependent on the correlation adjustment being 10%. If you assume instead 5% or 15%, you get the below graphs.
 
 
How much does correlation matter?
A complementary way of viewing things is to assume a win percentage, and compare the different correlation levels in one picture. Here you can see that the line is steeper for power plays — meaning that correlation (naturally) affects them more. You can raise your ROI from ~0% to ~40% if you bet Power 4s and the correlation is 10 percentage points. Similarly, for Flex 5s your ROI increases from 12% to 34% as correlation increases from zero to 10 percentage points.
Lastly, per my intuition, eventually the Power 4 does overtake the Flex 5 as the superior wager (again, excluding Flex 6s) at around 8% correlation or so.
 
Additional graphs showing ROIs at various win probabilities and correlation levels for Power 4s and Flex 5s:
 
 
Lessons:
•	Do not bet Power 2s, Power 3s, Flex 3s, and to a lesser extent Flex 4s (but we knew this already).
•	You *definitely* want to be correlating your plays, especially when betting power plays. ROI increases significantly in doing so. That being said, you do not want to start playing -EV plays just to sneak in correlation. I always try to maintain both plays in my “correlated pairs” as +EV on their own.
•	If you have an unlimited account and can bet Flex 6s with 3 correlated pairings, by all means go ahead and smash it.

