# awesome-abtesting

## Glossary
- *Type 1 error* - rejecting the null hypothesis when it is actually correct, or roughly, claiming that a behavior change is seen when in reality there is no change between the sampled populations.
- *Type 2 error* - accepting the null hypothesis when it is actually not correct, or in other words, claiming that we don't see a change between the buckets when a difference exists between sampled population
- *A/A test* test where to treatment is applied to the "treatment" bucket, and effectively results in having two control groups. Very useful for validating testing environment and verify overall correctness of the tooling: to seee if there are no obvious issues with outliers and pre-filtering. A/A traffic can be used to determine the amount of traffic necessary to detect the predicted metric changes. The A/A/B test helps to reduce the false positive rate due to the "biased" buckets. 


### Multiple control tests

- *Implications of use of multiple controls in an A/B test* [link](https://blog.twitter.com/engineering/en_us/a/2016/implications-of-use-of-multiple-controls-in-an-ab-test.html)
 - A/A/B test is one in which in addition to the provided control (A1), an experimenter would add another control group (A2) to their experiment. Create two control groups (A1 and A2).  There are two approaches - first one to use A2 for validation - if treatment shows significance difference against both control A and control A2, it is "real", otherwise it's descarded as a false positive. Another approach is to use A2 as a replacement control when A1 bucket is believed to be a poor representation of the underlying population
 - Doubling the size of a single control bucket is preferred over implementing two separate t-tests using two control buckets. Using two controls exposes the experimenter to a high risk of confirmation bias. If the experimenter resists the bias and only uses the second control as a validation tool, the experiment suffers from higher rate of false negatives than if the controls were pooled