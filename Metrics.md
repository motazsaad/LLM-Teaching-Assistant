# Evaluation Metrics for LLM-Based Essay Grading

## 1. Pearson or Spearman Correlation – To Check How Well the LLM’s Scores Align with Human Scores  
These metrics measure the relationship between LLM-generated scores and human-annotated scores.  

### **Pearson Correlation Coefficient (r)**  
- Measures the **linear relationship** between two sets of numerical scores (LLM vs. human scores).  
- Values range from **-1 to 1**:  
  - **1** → Perfect positive correlation (LLM scores increase exactly like human scores).  
  - **0** → No correlation (LLM scores are random).  
  - **-1** → Perfect negative correlation (LLM scores decrease when human scores increase).  
- **Formula:**  
  \[
  r = \frac{\sum (x_i - \bar{x}) (y_i - \bar{y})}{\sqrt{\sum (x_i - \bar{x})^2} \cdot \sqrt{\sum (y_i - \bar{y})^2}}
  \]
  where \( x \) = human scores, \( y \) = LLM scores, and \( \bar{x}, \bar{y} \) are their means.  

### **Spearman Rank Correlation (ρ)**  
- Measures how well the **ranking** of scores matches between LLM and human.  
- Works for **nonlinear** relationships and ordinal data.  
- Formula is similar to Pearson, but it uses **ranked** values instead of raw scores.  

#### **Use case:**  
- **Pearson** is better if you expect LLM and human scores to have a linear relationship.  
- **Spearman** is better if scores follow an **ordinal** pattern (e.g., essay grading rubrics).  

---

## 2. Mean Absolute Error (MAE) or RMSE – To Quantify the Difference Between LLM and Human Scores  
These metrics measure how far LLM scores deviate from human scores.  

### **Mean Absolute Error (MAE)**
- Measures the **average absolute difference** between LLM and human scores.  
- **Formula:**  
  \[
  MAE = \frac{1}{n} \sum_{i=1}^{n} | y_i - \hat{y}_i |
  \]
  where \( y_i \) is the human score, \( \hat{y}_i \) is the LLM score, and \( n \) is the total number of essays.  

- **Pros:**  
  - Easy to interpret.  
  - Doesn’t penalize large errors more than small ones.  

### **Root Mean Squared Error (RMSE)**  
- Similar to MAE but **squares the errors**, giving more weight to larger mistakes.  
- **Formula:**  
  \[
  RMSE = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}
  \]

- **Pros:**  
  - Penalizes **larger mistakes** more, making it useful when bigger grading errors are unacceptable.  
- **Cons:**  
  - More sensitive to outliers than MAE.  

#### **Use case:**  
- **MAE** is better when all errors should be treated equally.  
- **RMSE** is better if **larger grading mistakes** should be penalized more.  

---

## 3. ROUGE or BERTScore – To Measure How Similar LLM Explanations Are to Human Explanations  
Since LLM provides textual explanations, we need NLP-based similarity measures.  

### **ROUGE (Recall-Oriented Understudy for Gisting Evaluation)**  
- Measures **word overlap** between LLM explanations and human explanations.  
- **ROUGE-N**: Based on n-gram (word sequence) overlap.  
- **ROUGE-L**: Uses longest common subsequence (LCS) to measure similarity.  
- **Formula (ROUGE-1 for unigrams):**  
  \[
  \text{ROUGE-1 Recall} = \frac{\text{\# of overlapping unigrams}}{\text{\# of unigrams in reference explanation}}
  \]

#### **Use case:**  
- Good when human explanations have **high lexical overlap** with correct explanations.  

### **BERTScore**  
- Uses **BERT embeddings** to compare the semantic similarity of LLM and human explanations.  
- Doesn’t rely on exact word overlap like ROUGE.  
- **Formula:**  
  \[
  BERTScore = \frac{1}{n} \sum_{i=1}^{n} \text{cosine similarity}(\text{LLM token}, \text{Human token})
  \]
- More accurate for essays because it captures **meaning** rather than exact word matches.  

#### **Use case:**  
- Better than ROUGE when explanations use **paraphrasing** rather than exact words.  

---

## **Summary of Use Cases**
| Metric | Purpose | Best When |  
|--------|---------|------------|  
| **Pearson Correlation** | Measures **linear** similarity of scores | LLM scores should be proportional to human scores |  
| **Spearman Correlation** | Measures **rank order** similarity | Scores follow an ordinal rubric (e.g., grading scale) |  
| **MAE** | Measures **absolute grading error** | All grading mistakes should be treated equally |  
| **RMSE** | Penalizes **large mistakes more** | Some mistakes (e.g., scoring 2 instead of 6) are worse |  
| **ROUGE** | Measures **word overlap in explanations** | LLM and human explanations use similar words |  
| **BERTScore** | Measures **semantic similarity** | LLM explanations paraphrase human ones |  

---

