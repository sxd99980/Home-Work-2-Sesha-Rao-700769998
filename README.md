README — Homework 2 (CS5760 Natural Language Processing)
Student: Duggineni Sesha Rao
Course: CS5760 — Fall 2025
Assignment: Homework 2
# CS5760 - Natural Language Processing  
## **Q1. Bayes’ Rule Applied to Text**

### **1. Meaning of Terms**
- **P(c): Prior Probability of Class**  
  How likely a class is before looking at the document.  
  Example: If 70% of emails are spam → P(spam) = 0.7.

- **P(d|c): Likelihood**  
  Probability of seeing document *d* if it belongs to class *c*.  
  Example: Probability of seeing "discount" if an email is spam.

- **P(c|d): Posterior**  
  Probability that document *d* belongs to class *c* after seeing its contents.  
  This is what we maximize to pick the best class.

### **2. Why Ignore P(d)?**
P(d) is the same for all classes, so it does not affect which class has the highest probability.  
We just maximize **P(c) × P(d|c)**.

---

## **Q2. Add-1 Smoothing**

### **1. Denominator**
For the negative class:  
Total tokens = 14, Vocabulary size = 20  
**Denominator = 14 + 20 = 34**

### **2. P(predictable|−)**
Count = 2  
**P(predictable|−) = (2+1)/34 = 3/34 ≈ 0.0882**

### **3. P(fun|−)**
Count = 0  
**P(fun|−) = (0+1)/34 = 1/34 ≈ 0.0294**

---

## **Q3. Document Classification ("predictable no fun")**

### **Negative Class**
Formula:  
P(−) × P(predictable|−) × P(no|−) × P(fun|−)

Step-by-step:  
1. (3/5) × (3/34) = 9/170 ≈ 0.05294  
2. (9/170) × (1/34) = 9/5780 ≈ 0.00156  
3. (9/5780) × (1/34) = 9/196,520 ≈ 0.0000458  

**Score(−) ≈ 0.0000458**

### **Positive Class**
Formula:  
P(+) × P(predictable|+) × P(no|+) × P(fun|+)

Step-by-step:  
1. (2/5) × (1/34) = 1/85 ≈ 0.01176  
2. (1/85) × (1/34) = 1/2890 ≈ 0.000346  
3. (1/2890) × (1/34) = 1/98,260 ≈ 0.00001018  

**Score(+) ≈ 0.00001018**

### **Decision**
Since Score(−) > Score(+), **assign NEGATIVE class**.

---

## **Q4. Harms of Classification**

1. **Representational Harm:**  
   When a system reinforces stereotypes or misrepresents a group.  
   *Example:* Kiritchenko & Mohammad (2018) showed certain demographic names get more negative scores.

2. **Risk of Censorship:**  
   Toxicity classifiers often block reclaimed words used by marginalized groups (Dixon et al., 2018).

3. **Performance Gap:**  
   Models trained mostly on Standard English perform worse on African American English or Indian English because of vocabulary/grammar differences.

---

## **Q5. Confusion Matrix Metrics**

### **Per-Class Metrics**
| Class   | TP | FP | FN | Precision | Recall |
|--------|----|----|----|-----------|-------|
| Cat    |  5 | 15 | 15 | 0.25 | 0.25 |
| Dog    | 20 | 25 | 25 | 0.4444 | 0.4444 |
| Rabbit | 10 | 15 | 15 | 0.4 | 0.4 |

### **Macro-Averaging**
- Precision = (0.25 + 0.4444 + 0.4)/3 = **0.3648**  
- Recall = (0.25 + 0.4444 + 0.4)/3 = **0.3648**

### **Micro-Averaging**
- Total TP = 35, FP = 55, FN = 55  
- Precision = 35/(35+55) = **0.3889**  
- Recall = 35/(35+55) = **0.3889**

**Interpretation:**  
- Macro treats all classes equally.  
- Micro is dominated by frequent classes.

---

## **Q6. Bigram Probabilities & Zero-Probability Problem**

### **Sentence Probabilities**
1. **S1: `<s> I love NLP </s>`**
   (2/3) × 1 × (1/2) × 1 = **1/3 ≈ 0.3333**

2. **S2: `<s> I love deep learning </s>`**
   (2/3) × 1 × (1/2) × 1 × (1/2) = **1/6 ≈ 0.1667**

**S1 is more probable.**

### **Zero-Probability Problem**
- P(noodle|ate) = 0 → sentence probability becomes 0 → undefined perplexity.

### **Laplace Smoothing**
P(noodle|ate) = (0+1)/(12+10) = **1/22 ≈ 0.04545**

---

## **Q7. Backoff Model**

1. **P(cats|I,like) = 1/2 = 0.5**
2. **P(dogs|You,like):** trigram count=0 → back off to bigram:  
   P(dogs|like) = 1/3 ≈ 0.3333
3. **Why Backoff?**  
   Avoids zero probabilities by using lower-order n-grams.

---

## **Q8. Programming**
Implement:
1. Read training corpus.
2. Count unigrams & b
