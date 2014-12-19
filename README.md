I decided I wanted to roll my own AB testing app for Django (https://github.com/crobertsbmw/RobertsAB) when I was finished, I came across this:

http://stevehanov.ca/blog/index.php?id=132

Which is a very convincing article on why AB testing sucks and with a few extra lines, you can improve your algorithm to select the best test so you never go back and update your code (yeah right...)

I then thought, how many tests does this thing have to run to truly figure out which is best?

So, I have 4 tests with probability of success equalling 1/2, 1/4, 1/5, 1/6 respectively and found that for this algorithm to settle on the best success rate (1/2), it took on average, 91 hits, with a max of 876 tests.

I ran the same test using a standard AB algorithm. Basically, picking whichever test has been tested the least and run that test. It took on average 32 tests to figure out which test performed the best with a maximum of 363 tests to figure out which performed the best. On average 3 times better than the greedy epsilon method.

I thought with such difference between success ratios that the epsilon greedy method would perform a lot better. I was very surprised. There is some tweaking that you can do with changing the random number from 1/10 to 1/8 which will help you find your best test faster, but the tradeoff is that once you have found the "best test" you continue to try the inferior test one out of eight times.

I then tried tweaking my success ratios to something a little less dramatic. 1/10, 1/11, 1/12, 1/13. Over 1000 tests: 

Epsilon Greedy:
7452.141 tests to find the right solution on average, but the max number of tests hit 14999 (I only ran 15000 AB tests) so the average could be much higher.


Standard AB method:
5419.16 tests to find the right solution on average, again, the max number of tests hit 14999 so the average could be higher, but the averages of the AB method vs the Epsilon greedy method is still a valid comparable.


I like the epsilon greedy method. It is simple and witty. 
The only problem is that in reality you don't know what the best solution is so you can never know if you have gotten to the "actual" best solution yet. The epsilon greedy method will eventually get there (although you will never know if it has). And if you are using the standard AB method you will never know if you have arrived at the best option either, especially when we are talking about the difference between 1/20 clicks versus 1/21 clicks.

Moral of the story -- AB testing is probably a waste of time.

This requires python3: https://github.com/crobertsbmw/EpsilonGreedy