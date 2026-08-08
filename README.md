# GLADandMACEensemble
Ensemble model I experimented with - Generative model of Labels, Abilities, and Difficulties, and Multi-Annotator Competence Estimation. Dive into best way to approximate true labels when a solid ground truth doesn't exist and given annotators may have inherent bias. Combines expectation maximisation algorithm with Bayesian methods

## Output
Total annotations with different rounds - each round is a pass of the alternating optimisation, where first the continuous parameters get updated and the spam label habitats get updated. The negative log likelihood dropping is the model fitting the data better each round aka convergence. 

If you collapse the model's soft distribution to the most likely label per item and compare the true to generated labels, it yielded 82% - however this number only existed from the synthetic test where we knew what our ground truth was which in the real world we don't usually. However from this we can approximate the reliability of soft distributions.

### Annotator Diagnostics
- annotators skilled/diligent (0-4): ability 7.5-9.8, diligence 0.99-1.00 meaning the model correctly identified these annotators as accurate and almost always in a diligent state
- annotators diligent but mediocre (5-9): ability 1-2 (lower but positive), diligence 0.7-0.97 meaning the diligence state was positive but the model compensated knowing they were trying just not good enough
- annotators that were biased spammers (10-12): diligence that collapsed to 0.00-0.19 which the model correctly caught as them disengaging
- annotators that spammed uniformly (13-14): diligence came high 0.92-0.66 even though they were spamming which is the identifiable limit that needs to be flagged. **Limitation** *A person spamming with a uniform random guess and a diligent person who's just bad produce statistically identical label patterns which can be a limitation to this model if uniform-spam detection matters*

### Item uncertainty
easy items get confident and low entropy predictions whereas hard items get higher uncertainty where annotators disagree more and the model reflects that honestly instead of trying to force a fake confident answer

### Adversarial vs Confident Items
Contested items e.g. item 199 have probability spread pretty much evenly across classes which is the model saying it doesn't know and is yielding the shape of its uncertainty instead of picking one arbitrarily. Confident items get a spike in one class and their ease values are a lot higher signifying the model knows they're easy items with clear signals.

### Baseline comparison
This was just a plain majority vote where you just take the label most annotators gave, and it got 76.5% which was our baseline to compare. The hybrid model which weighted annotators via estimated skill and diligence got 82%- this 5.5% gap is significant throughout this modelling, it's not aggregating raw votes but weighted by who's trustworthy.
