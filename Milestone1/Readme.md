
## 🎯 Objective

The primary goal is to perform a deep analysis of Python code snippets by:
1.  **Parsing** the code to understand its structure.
2.  **Extracting** key syntactic features like functions, classes, imports, and common coding patterns.
3.  **Tokenizing** the code into its fundamental units.
4.  **Generating semantic vector embeddings** using three different state-of-the-art transformer models.
5.  **Comparing the models** by visualizing their outputs to understand their unique interpretations of code similarity.

---
## ⚙️ Approach and Methodology
### **1. Code Corpus Preparation**
A diverse set of **10 Python code snippets** was created to serve as the analytical dataset. This corpus includes a variety of programming constructs such as function and class definitions, list comprehensions, exception handling, and loops to ensure a comprehensive evaluation.

### **2. Syntactic & Structural Analysis**
Before semantic interpretation, the code is deconstructed into its grammatical components.

* **Abstract Syntax Tree (AST) Parsing**: Python's `ast` module is used to convert each code snippet into a tree-like structure that represents its syntax. By traversing this tree, we programmatically extract high-level features like the names of **functions**, **classes**, **imported libraries**, and, crucially, identifiable **code patterns** like `List Comprehensions`, `Exception Handling` blocks, `With Statements`, and `For Loops`.

* **Tokenization**: The `tokenize` module breaks down the code into a linear sequence of tokens. Non-essential tokens (like whitespace and comments) are filtered out to create a clean representation of the code's logic.

### **3. Semantic Embedding Generation 🧠**
This is the core NLP task. The code snippets are treated as text and fed into pre-trained **SentenceTransformer** models to convert them into high-dimensional numerical vectors (embeddings). These embeddings capture the "meaning" of the code.

Three distinct models were chosen for comparison:
* **`all-MiniLM-L6-v2` (MiniLM)**: A fast and efficient general-purpose model.
* **`distilroberta-base-paraphrase-v1` (DistilRoBERTa)**: A model fine-tuned for recognizing semantic similarity, even with different syntax.
* **`all-mpnet-base-v2` (MPNet)**: A high-performance model that often sets the standard for semantic search tasks.

### **4. Visualization & Model Comparison**
To make the high-dimensional embeddings understandable, two visualization techniques are employed.

* **PCA Scatter Plots**: **Principal Component Analysis (PCA)** is used to reduce the embedding vectors from hundreds of dimensions down to just two. These 2D points are plotted on a scatter plot, providing a high-level "map" of how each model organizes the snippets in its semantic space.

* **Cosine Similarity Heatmaps**: For a more detailed view, we calculate the **cosine similarity** between every pair of snippet embeddings. This results in a similarity matrix which is then visualized as a heatmap. The color of each cell indicates the degree of similarity (from 0.0 to 1.0) between two snippets.

---
## 📊 How to Interpret the Results

### **PCA Scatter Plots**
On these plots, **distance implies semantic difference**.
* **Clusters**: Tightly grouped snippets are considered highly similar by the model. Look for logical groupings, such as all function definitions forming a distinct cluster.
* **Spacing**: The distance between clusters or individual points reveals how different the model considers them.
* **Comparison**: By comparing the plots, you can see how each model's "map" differs. One model might group snippets by their action (e.g., iteration), while another might group them by their structure (e.g., function definitions).



### **Similarity Heatmaps**
The heatmap provides a quantitative, pairwise comparison.
* **Color**: Bright, "hot" colors indicate high similarity (close to 1.0), while dark, "cold" colors indicate low similarity.
* **The Diagonal**: The bright diagonal from top-left to bottom-right is always 1.0, as it compares each snippet to itself.
* **Off-Diagonal Hotspots**: Bright squares off the diagonal are the most interesting, as they show which distinct snippets the model finds to be semantically related.
* **Comparison**: Notice where the heatmaps agree and disagree. A bright square in the same position on all three maps indicates a strong consensus. Differences in scores reveal each model's unique bias and understanding.



---
## **Conclusion**
The key takeaway is that there is **no single "best" model**; each one develops a unique "understanding" based on its architecture and training data.

The visualizations clearly show that **MiniLM**, **DistilRoBERTa**, and **MPNet** produce different semantic spaces. By comparing their outputs, a developer can choose the most appropriate model for a specific task, whether it's for code search, automated documentation, or code classification.
