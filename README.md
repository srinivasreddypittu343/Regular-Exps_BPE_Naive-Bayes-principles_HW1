# Regular-Exps_BPE_Naive-Bayes-principles_HW1

# CS5760 - Natural Language Processing
## Homework Assignment 1

---

## 👤 Student Information
* **Student Name:** Srinivas Reddy Pittu
* **Course:** CS5760 - Natural Language Processing
* **Department:** Department of Computer Science & Cybersecurity
* **University:** University of Central Missouri
* **Term:** Fall 2026

---

## 📌 Overview
This repository contains the full source code, manual calculations, and reflections for **Homework 1** in Natural Language Processing. The assignment covers fundamental NLP concepts including regular expressions, Byte-Pair Encoding (BPE), Naive Bayes classification principles, add-1 smoothing calculations, manual word tokenization vs. automated tools, and multiword expression (MWE) processing.

---

## 📂 Repository Structure

```text
.
├── main.ipynb           # Python script containing Q1, Q2, and Q5 implementations
├── README.md            # Comprehensive project documentation and report
```

---

## 📄 Questions & Work Explanation

### Q1: Regular Expressions (Regex)
This section implements regular expression patterns for text processing tasks in Python using the `re` module:
1. **U.S. ZIP Codes:** Matches standard 5-digit ZIP codes and ZIP+4 formats (hyphen or space allowed) using strict word boundaries `\b`.
2. **Words Not Starting with Capital Letters:** Captures words beginning with lowercase letters, preserving internal hyphens/apostrophes (e.g., `don’t`, `state-of-the-art`).
3. **Rich Number Extraction:** Matches integers, decimals, thousands-comma separators, signs, and scientific notations (e.g., `1.23e-4`).
4. **Email Spelling Variants:** Case-insensitive search for variations like `email`, `e-mail`, or `e mail` (including en-dash).
5. **Interjection Matching:** Matches variations of `go`, `goo`, `gooo!` with optional trailing punctuation.
6. **Sentence Endings:** Identifies lines ending with a question mark followed by optional closing quotes or brackets.

---

### Q2: Manual & Automated Byte-Pair Encoding (BPE)

#### 2.1 Manual BPE Execution
Given the toy corpus occurrences (`low:5`, `lowest:2`, `newer:6`, `wider:3`, `new:2`):
* **Initial Vocabulary $V_0$:** `{_, d, e, i, l, n, o, r, s, t, w}` (Size: 11)
* **Merge 1:** `(e, r) -> er` (Count: 9) $\rightarrow$ $V_1$ Size: 12
* **Merge 2:** `(er, _) -> er_` (Count: 9) $\rightarrow$ $V_2$ Size: 13
* **Merge 3:** `(e, w) -> ew` (Count: 8) $\rightarrow$ $V_3$ Size: 14

#### 2.2 Mini-BPE Learner (Python)
A custom BPE trainer was written in Python to simulate subword tokenization.
* **Segmentation Results:**
  * `new` $\rightarrow$ `['new_']`
  * `newer` $\rightarrow$ `['newer_']`
  * `lowest` $\rightarrow$ `['lowest_']`
  * `widest` $\rightarrow$ `['w', 'i', 'd', 'est_']`
  * `newestest` $\rightarrow$ `['new', 'est', 'est_']`

* **OOV Problem Explanation:** BPE mitigates out-of-vocabulary (OOV) issues by breaking unseen full words into subword pieces that exist in the trained subword vocabulary. For instance, `widest` was absent in training but could still be encoded via base characters `w`, `i`, `d` and subword `est_`. Subwords like `er_` naturally align with English comparative/agent morphemes.

#### 2.3 Subword Segmentation on Paragraph
Trained 30 merges over an English paragraph:
* **Top 5 Frequent Merges:** `(s, _)`, `(e, _)`, `(e, n)`, `(e, a)`, `(e, r)`
* **Top 5 Longest Subwords Learned:** `student`, `angu`, `stud`, `age_`, `and_`
* **Sample Word Segmentations:**
  * `students` $\rightarrow$ `student | s_`
  * `microarchitecture` $\rightarrow$ `m | i | c | r | o | ar | ch | i | t | ec | t | u | r | e_`

---

### Q3: Bayes' Rule Applied to Text Classification

$$c_{MAP} = \arg\max_{c \in C} P(c) P(d \mid c)$$

1. **Term Definitions:**
   * **$P(c)$ (Prior Probability):** The baseline probability of observing class $c$ before reading the document content.
   * **$P(d \mid c)$ (Likelihood):** The probability of observing the specific text content in document $d$ given class $c$.
   * **$P(c \mid d)$ (Posterior Probability):** The updated probability that document $d$ belongs to class $c$ after evaluating its text.
2. **Ignoring Denominator $P(d)$:** The term $P(d)$ is constant across all candidate classes for a given document $d$. Because it scales all class likelihood scores by the exact same positive denominator, removing it does not alter the relative ranking or the outcome of the $\arg\max$ operation.

---

### Q4: Add-1 (Laplace) Smoothing Calculations

Given $P(-)=3/5$, $P(+)=2/5$, $V=20$, and Total Token Count $N_- = 14$:
* **Smoothed Denominator:** $N_- + V = 14 + 20 = 34$
* **$P(\text{predictable} \mid -)$** (Occurs 2 times):
  $$\frac{2 + 1}{14 + 20} = \frac{3}{34} \approx 0.0882$$
* **$P(\text{fun} \mid -)$** (Occurs 0 times):
  $$\frac{0 + 1}{14 + 20} = \frac{1}{34} \approx 0.0294$$

---

### Q5: Tokenization, Tool Comparison, and Multiword Expressions (MWEs)

#### 1. Tokenization Comparison
* **Naïve Space-Based Tokenization:** Splitting by whitespace produced **37 tokens**, leaving punctuation attached to words (e.g., `app,`, `testing.`, `students'`).
* **Manual Morphological/Punctuation Tokenization:** Separated punctuation, possessive markers, contractions, and inflectional endings, producing **54 tokens** (e.g., `student` + `s` + `'`, `work` + `ing`, `does` + `n't`).
* **NLTK `TreebankWordTokenizer`:** Produced **46 tokens**.

#### 2. Diff Analysis
The primary differences between NLTK and the manual policy stem from inflectional suffixes: NLTK retains full inflected tokens like `working`, `testing`, `helped`, and `enjoyed`, whereas the manual exercise separated suffixes like `-ing` and `-ed`. Both approaches successfully isolated clitics (`'re`, `n't`), punctuation, and possessives.

#### 3. Multiword Expressions (MWEs)
Identified and integrated MWEs using NLTK's `MWETokenizer`:
* `New York` $\rightarrow$ `New_York` (Proper Location)
* `break the ice` $\rightarrow$ `break_the_ice` (Idiom)
* `ice cream` $
ightarrow$ `ice_cream` (Compound Noun)

---

## 📚 References
1. Python Software Foundation. *re — Regular expression operations.* [https://docs.python.org/3/library/re.html](https://docs.python.org/3/library/re.html)
2. NLTK Project. *TreebankWordTokenizer Documentation.* [https://www.nltk.org/api/nltk.tokenize.TreebankWordTokenizer.html](https://www.nltk.org/api/nltk.tokenize.TreebankWordTokenizer.html)
3. Sennrich, R., Haddow, B., & Birch, A. (2016). *Neural Machine Translation of Rare Words with Subword Units.* [https://aclanthology.org/P16-1162/](https://aclanthology.org/P16-1162/)
