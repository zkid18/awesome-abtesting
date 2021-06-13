# awesome-abtesting

## Glossary

- *Type 1 error* - rejecting the null hypothesis when it is actually correct, or roughly, claiming that a behavior change is seen when in reality there is no change between the sampled populations.
- *Type 2 error* - accepting the null hypothesis when it is actually not correct, or in other words, claiming that we don't see a change between the buckets when a difference exists between sampled population
- *A/A test* test where to treatment is applied to the "treatment" bucket, and effectively results in having two control groups. Very useful for validating testing environment and verify overall correctness of the tooling: to seee if there are no obvious issues with outliers and pre-filtering. A/A traffic can be used to determine the amount of traffic necessary to detect the predicted metric changes. The A/A/B test helps to reduce the false positive rate due to the "biased" buckets.
- *Peeking problem* If you run A/B tests on your website and regularly check ongoing experiments for significant results, you might be falling prey to what statisticians call repeated significance testing errors. As a result, even though your dashboard says a result is statistically significant, there’s a good chance that it’s actually insignificant. (c) [Evan Miller](https://www.evanmiller.org/)
- *Statistical significance*  Assuming there is no underlying difference between A and B, how often will we see a difference like we do in the data just by chance

### Multiple control tests

*Implications of use of multiple controls in an A/B test* [link](https://blog.twitter.com/engineering/en_us/a/2016/implications-of-use-of-multiple-controls-in-an-ab-test.html)

- A/A/B test is one in which in addition to the provided control (A1), an experimenter would add another control group (A2) to their experiment. Create two control groups (A1 and A2).  There are two approaches - first one to use A2 for validation - if treatment shows significance difference against both control A and control A2, it is "real", otherwise it's descarded as a false positive. Another approach is to use A2 as a replacement control when A1 bucket is believed to be a poor representation of the underlying population
- Doubling the size of a single control bucket is preferred over implementing two separate t-tests using two control buckets. Using two controls exposes the experimenter to a high risk of confirmation bias. If the experimenter resists the bias and only uses the second control as a validation tool, the experiment suffers from higher rate of false negatives than if the controls were pooled

#### Bootstrap

*“Quantile Bootstrap”: A Solution for Understanding Movement in Parts of a Distribution* [link1](https://netflixtechblog.com/data-compression-for-large-scale-streaming-experimentation-c20bfab8b9ce)
[link1](https://netflixtechblog.com/streaming-video-experimentation-at-netflix-visualizing-practical-and-statistical-significance-7117420f4e9a)

- The quantile function is the inverse of the cummulative distribution function for a given random variable
- Compare differences in quantile functions between the treatment and production experiences
- To estimate the variability within the production experience, we bootstrap that cell against itself by resampling (with replacement) twice, estimating the quantile function in each case, and then calculating the delta quantile function.
- If the p-values for the quantiles of interest are small, they can be assured that the newly developed encodes do in fact improve quality in the treatment experience
- Approximate for fast bootsrapping in a big dataset using a compressed data object with a limited number of unique values. The data for each test cell is then represented as a set of (value, count) pairs, and we can bootstrap on the counts using draws from a multinomial.

#### Bayesian A/B Testing

*Is Bayesian A/B Testing Immune to Peeking? Not Exactly* [link](http://varianceexplained.org/r/bayesian-ab-testing/)

- Bayesian A/B testing is not “immune” to peeking and early-stopping. Just like frequentist methods, peeking makes it more likely you’ll falsely stop a test. The Bayesian approach is, rather, more careful than the frequentist approach about what promises it makes.

#### Peeking problem

*How Not To Run an A/B Test* [link](https://www.evanmiller.org/how-not-to-run-an-ab-test.html)

- Repeated significance testing always increases the rate of false positives, that is, you’ll think many insignificant results are significant (but not the other way around).
- The more you peek, the more your significance levels will be off.
- If you run experiments: the best way to avoid repeated significance testing errors is to not test significance repeatedly

#### Switchback tests

*How to Optimize your Switchback A/B Test Configuration* [link](https://towardsdatascience.com/how-to-optimize-your-switchback-a-b-test-configuration-791a28bee678)

- Switchback experiments, also known as time split experiments, employ sequential reshuffling of control/treatments to remove bias inherent to certain data. Used in two-sided marketplaces, Uber, DoorDash, Upwork, Lyft. They allow experimentations with finite resources. A/B testing frameworks that run many versions of a single experiment over time.
- Carryover effect, the time it takes for our finite resource to replenish, to minimize the variance of our experiment.  One example for rideshare markets is the number of time splits it takes for drivers to become available for another ride. From our freelancer example above, when a freelancer is hired, they are removed from the candidate pool for some time while they complete the project. The duration that it takes for a candidate to reenter the pool is our carryover duration.
- Encode the Risk Function to Minimize. The function that minimized will provide the best randomization points for our specified carryover effect order (m) and experiment length (K).
- Develop optimized control/treeatment sequence
- Run the experiment and analyze 

*Switchback Tests and Randomized Experimentation Under Network Effects at DoorDash* [link](https://medium.com/@DoorDash/switchback-tests-and-randomized-experimentation-under-network-effects-at-doordash-f1d938ab7c2a)

- Given the systemic nature of many of our products, simple A/B tests are often ineffective due to network effects. To be able to experiment in the face of network effects, we use a technique known as switchback testing, where we switch back and forth between treatment and control in particular regions over time
- The problem here is the network effect — both sets of Consumers share the same Dasher fleet — so adding a Consumer to the treatment group also affects the experience of Consumers in the control group and we can not establish independence between the two groups.
- Use 30 minutes carry-over
- Traditional A/B testing has an assumption of independent units, which means that if we know something about the behavior of one unit, it does not tell us anything about the behavior of the second. This assumption is clearly violated in the case of time-region units as the performance of one time-region is highly correlated and can influence the performance of the next. 
- To understand the interplay between natural variation and number of samples, we ran a series of long-term A/A tests. These tests allowed us to look at how margin of error differed for the average value of a certain metric in a time-region unit as we switched back and forth more quickly or more slowly. 




 




#### Sequnetial AB testing

*Simple Sequential A/B Testing* [link] (https://www.evanmiller.org/sequential-ab-testing.html)

- Sequential sampling allows the experimenter to stop the trial early if the treatment appears to be a winner;
- At the beginning of the experiment, choose a sample size 𝑁.
- Assign subjects randomly to the treatment and control, with 50% probability each.
- Track the number of incoming successes from the treatment group. Call this number 𝑇.
- Track the number of incoming successes from the control group. Call this number 𝐶.
- If 𝑇−𝐶 reaches 2sqrt(𝑁), stop the test. Declare the treatment to be the winner.
- If 𝑇+𝐶 reaches 𝑁, stop the test. Declare no winner.
