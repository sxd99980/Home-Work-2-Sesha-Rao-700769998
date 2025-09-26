README — Homework 2 (CS5760 Natural Language Processing)
Student: Duggineni Sesha Rao
Course: CS5760 — Fall 2025
Assignment: Homework 2
Q1 – Bayes Rule Applied to Text

P(c): Prior probability of class c (likelihood of c before seeing the document).

P(d|c): Likelihood (chance of seeing document d if it belongs to class c).

P(c|d): Posterior probability (chance that document d belongs to c after looking at its content).

Why ignore P(d): P(d) is constant for all classes, so it doesn’t affect which class has the highest probability — we just maximize P(c) × P(d|c).

Q2 – Add-1 (Laplace) Smoothing

Denominator: Add vocabulary size to token count.
14 + 20 = 34

P(predictable | −): (2+1)/34 = 3/34 ≈ 0.0882

P(fun | −): (0+1)/34 = 1/34 ≈ 0.0294

Explanation: Add-1 smoothing avoids zero probabilities by giving every word at least a small chance.

Q3 – Worked Example Document Classification

Document: “predictable no fun”

Negative class:
(3/5) × (3/34) × (1/34) × (1/34) ≈ 0.0000458

Positive class:
(2/5) × (1/34) × (1/34) × (1/34) ≈ 0.0000102

Decision: Negative score > Positive score → Classify as Negative.
Reason: Negative words occur more often in negative training data.

Q4 – Harms of Classification

Representational harm: When a system reinforces stereotypes or misrepresents groups.
Example: Kiritchenko & Mohammad (2018) found sentiment lexicons rated some demographic-related words more negatively, reinforcing bias.

Risk of censorship: Toxicity classifiers may over-block harmless speech (e.g., flagging reclaimed slurs, LGBTQ+ discussions).

Performance gaps on AAE/Indian English: Training data is mostly Standard English → fewer examples → more mistakes and unfair treatment.

Q5 – Evaluation Metrics

Confusion Matrix Results:

Cat: Precision = 0.25, Recall = 0.25

Dog: Precision = 0.4444, Recall = 0.4444

Rabbit: Precision = 0.4, Recall = 0.4

Macro-Average: Precision = Recall ≈ 0.3648
Micro-Average: Precision = Recall ≈ 0.3889

Interpretation:

Macro treats all classes equally (good for imbalance).

Micro weights by total predictions (reflects overall accuracy).

Q6 – Bigram Probabilities & Zero-Probability Problem

P(S1: <s> I love NLP </s>): 1/3 ≈ 0.3333

P(S2: <s> I love deep learning </s>): 1/6 ≈ 0.1667

More probable sentence: S1

Zero-Probability Problem:

P(noodle|ate) = 0 → makes any sentence with “ate noodle” have probability 0.

Solution: Add-1 smoothing: (0+1)/(12+10)=1/22≈0.04545 so no sentence gets probability 0.

Q7 – Backoff Model

P(cats | I, like): 1/2 = 0.5

P(dogs | You, like): Trigram unseen → backoff to bigram:
P(dogs | like) = 1/3 ≈ 0.3333

Why backoff: Avoids zero probabilities for unseen trigrams by using lower-order (bigram) probabilities, giving more reliable estimates.

Q8 – Bigram Language Model (Concept)

Read and tokenize training corpus.

Compute unigram and bigram counts.

Estimate P(w₂|w₁) using MLE.

Define a function to multiply probabilities for a sentence.

Test on <s> I love NLP </s> and <s> I love deep learning </s>.

Choose the sentence with the higher probability as the model’s prediction.
Reason: It represents a sequence more likely under training data.
