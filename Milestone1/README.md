## Milestone 1: Code Explainer
1. Prepare a minimum of 10 code snippets.
2. For each snippet:

   * Parse using AST (Abstract Syntax Tree).

   * Extract functions, classes, imports, and code patterns.

   * Tokenize the code.

3. Forward the processed snippets to the following pretrained models:

   * MiniLM

   * DistilRoBERTa

   * MPNet

4. Perform model comparisons and generate visualizations to illustrate differences in outputs and embeddings.



## 🎯 Objective

The primary goal is to perform a deep analysis of Python code snippets to understand not just *what* they are written, but *what they mean*. This is achieved by:

1.  **Parsing** the code to understand its grammatical structure and hierarchy.
2.  **Extracting** key syntactic features like functions, classes, imports, and common coding patterns.
3.  **Tokenizing** the code into its fundamental lexical units.
4.  **Generating semantic vector embeddings** using three different state-of-the-art transformer models. These embeddings translate the "meaning" of the code into a numerical format.
5.  **Comparing the models** by visualizing their outputs to understand their unique interpretations of code similarity, which is crucial for tasks like code search, plagiarism detection, and automated refactoring.


## ⚙️ Approach and Methodology
### **1. Code Corpus Preparation**
A diverse set of **10 Python code snippets** was created to serve as the analytical dataset. This corpus includes a variety of programming constructs such as function and class definitions, list comprehensions, exception handling, and loops to ensure a comprehensive evaluation and test how the models handle different logical structures.

### **2. Syntactic & Structural Analysis**
Before semantic interpretation, the code is deconstructed into its grammatical components. This step is about understanding the code's form and structure, independent of its ultimate meaning.

* **Abstract Syntax Tree (AST) Parsing**: Python's `ast` module is used to convert each code snippet into an **Abstract Syntax Tree (AST)**.
    * **What is an AST?** An AST is a tree representation of the abstract syntactic structure of source code. Think of it like a sentence diagram for a programming language. It strips away non-essential information like comments, whitespace, and parentheses, focusing purely on the code's logical hierarchy. Each node in the tree represents a construct in the code, such as a function call, a variable assignment, or a loop.
    * **Why is it useful?** By traversing this tree, we can programmatically and reliably extract high-level features like the names of **functions**, **classes**, **imported libraries**, and, crucially, identifiable **code patterns** like `List Comprehensions`, `Exception Handling` blocks, `With Statements`, and `For Loops`. This is far more robust than simple text searching. 

* **Tokenization**: The `tokenize` module breaks down the code into a linear sequence of tokens.
    * **What is a Token?** A token is the smallest individual unit of a program, like a keyword (`def`, `for`), an identifier (variable or function names), an operator (`+`, `=`), or a literal (`'hello'`, `123`).
    * **AST vs. Tokens**: While an AST represents the code's hierarchical structure, tokenization provides a flat, sequential view. For this analysis, non-essential tokens (like whitespace, newlines, and comments) are filtered out to create a clean, linear representation of the code's logic.

### **3. Semantic Embedding Generation 🧠**
This is the core Natural Language Processing (NLP) task where we move from syntax to semantics (meaning). The pre-processed code snippets are treated as text and fed into pre-trained **SentenceTransformer** models to convert them into high-dimensional numerical vectors, known as **embeddings**. These embeddings capture the "meaning" of the code in a way that allows for mathematical comparison.

Three distinct models were chosen for comparison, each with different strengths:

* **`all-MiniLM-L6-v2` (MiniLM)**: A fast and highly-efficient model with 23M parameters, ideal for general-purpose tasks where speed and low resource usage are critical.
  
   The "MiniLM" stands for "Mini Language Model," and it was created using a process called **distillation**, where a smaller model learns from a larger, more powerful one. The "L6" indicates it has 6 layers. **Its main advantage is speed**, making it ideal for real-time applications or processing large volumes of code where computational cost is a concern.

* **`distilroberta-base-paraphrase-v1` (DistilRoBERTa)**:A mid-sized 82M parameter model specialized in recognizing semantic equivalence, making it excellent for paraphrase and duplicate detection tasks.
  
   A distilled version of the RoBERTa model, which itself is an optimized version of the famous BERT architecture. This specific model has been **fine-tuned on massive datasets of paraphrases**. This makes it exceptionally good at recognizing that two pieces of text (or code) are semantically equivalent, even if their syntax and wording are very different. It's an excellent choice for tasks involving semantic search or identifying duplicate logic.

* **`all-mpnet-base-v2` (MPNet)**: A large 110M parameter model offering state-of-the-art performance, best suited for applications requiring the highest accuracy and nuanced semantic understanding.
  
   MPNet (Masked and Permuted Language Modeling) innovatively combines the pre-training methods of models like BERT and XLNet, allowing it to overcome their individual limitations and gain a richer contextual understanding. It is generally considered the **most powerful of the three**, providing the most nuanced embeddings, but at a higher computational cost.


### 📊 Quick Comparison Table

| Metric                  | `all-MiniLM-L6-v2`      | `distilroberta-base-paraphrase-v1` | `all-mpnet-base-v2`     |
| ----------------------- | ----------------------- | ---------------------------------- | ----------------------- |
| **Parameters** | ~22.7 Million           | ~82 Million                        | ~110 Million            |
| **Embedding Dimension** | 384                     | 768                                | 768                     |
| **Layers** | 6                       | 6                                  | 12                      |
| **Max Tokens** | 256                     | 512                                | 512                     |
| **Primary Use Case** | Speed & Efficiency      | Paraphrase & Semantic Equivalence  | Highest Quality & Nuance |

### **4. Visualization & Model Comparison**
To make the high-dimensional embeddings (which can have hundreds of dimensions) understandable to humans, two visualization techniques are employed.

* **PCA Scatter Plots**: **Principal Component Analysis (PCA)** is a dimensionality reduction technique used to project the complex embedding vectors down to just two dimensions. It does this by identifying the directions (principal components) in which the data varies the most. These 2D points are then plotted on a scatter plot, providing a high-level "map" of how each model organizes the snippets in its semantic space.

**Principal Component Analysis (PCA)** is a technique used to simplify complex, high-dimensional data into a lower-dimensional form, like a 2D plot, while retaining the most important information.

In the context of our project, the transformer models convert each Python code snippet into a numerical vector with **768 dimensions**. Humans cannot visualize data in 768 dimensions, making it impossible to see how the models are grouping similar code snippets. We need a way to reduce this complexity to something we can see, like a 2D scatter plot.

PCA solves this by finding the best possible way to "project" the data onto a 2D map. Think of it like creating the most recognizable 2D shadow of a 3D object. PCA does this mathematically by identifying the directions where the data points are most spread out.

1.  **Principal Component 1 (PC1)**: This is the first and most important direction, capturing the largest possible variance in the dataset. It becomes the x-axis of our plot. For our code snippets, PC1 might represent the primary difference the model sees, such as "code structure" vs. "algorithmic logic."

2.  **Principal Component 2 (PC2)**: This is the second-most important direction, capturing the most *remaining* variance at a right angle to PC1. This becomes the y-axis, representing another key aspect of the code, like "data handling" vs. "control flow."

By plotting each snippet's position along these two principal components, we get a 2D scatter plot that is a meaningful summary of the original 768-dimensional space.

When interpreting this plot:
* **Distance implies similarity**: Snippets that are close together are considered semantically similar by the model.
* **Clusters reveal patterns**: Tightly grouped points show how the model categorizes code (e.g., all function definitions might form one cluster).

Ultimately, PCA acts as an essential bridge, translating the complex, high-dimensional understanding of an AI model into a simple, visual map that allows us to easily interpret its results.

* **Cosine Similarity Heatmaps**: For a more detailed, quantitative view, we calculate the **cosine similarity** between every pair of snippet embeddings.
    * **What is Cosine Similarity?** It measures the cosine of the angle between two vectors. It determines if two vectors are pointing in roughly the same direction, ignoring their magnitude. In the context of embeddings, it measures semantic similarity. A value of **1** means the snippets are semantically identical, **0** means they are unrelated, and values in between indicate partial similarity.
    * **Formula**: The cosine similarity between two vectors $\mathbf{A}$ and $\mathbf{B}$ is calculated as:
  
        $$\text{similarity} = \cos(\theta) = \frac{\mathbf{A} \cdot \mathbf{B}}{\|\mathbf{A}\| \|\mathbf{B}\|} = \frac{\sum_{i=1}^{n} A_i B_i}{\sqrt{\sum_{i=1}^{n} A_i^2} \sqrt{\sum_{i=1}^{n} B_i^2}}$$
      
        Where $\mathbf{A} \cdot \mathbf{B}$ is the dot product of the vectors and $\|\mathbf{A}\|$ and $\|\mathbf{B}\|$ are their magnitudes (norms).
    * The resulting similarity scores are placed in a matrix and visualized as a heatmap, where color intensity directly corresponds to the similarity score.


## 📊 How to Interpret the Results

### **PCA Scatter Plots**
On these plots, **distance implies semantic difference**.
* **Clusters**: Tightly grouped snippets are considered highly similar by the model. Look for logical groupings, such as all function definitions forming a distinct cluster away from list comprehensions.
* **Spacing**: The distance between clusters or individual points reveals how different the model considers them. Large gaps imply significant semantic divergence.
* **Comparison**: By comparing the plots side-by-side, you can see how each model's "map" differs. One model might group snippets by their action (e.g., iteration vs. data transformation), while another might group them by their high-level structure (e.g., function definitions vs. standalone expressions).

### **Similarity Heatmaps**
The heatmap provides a quantitative, pairwise comparison matrix.
* **Color**: Bright, "hot" colors (like yellow) indicate high similarity (a score close to 1.0), while dark, "cold" colors (like purple or black) indicate low similarity.
* **The Diagonal**: The bright diagonal from top-left to bottom-right is always 1.0, as it represents each snippet being compared to itself.
* **Off-Diagonal Hotspots**: Bright squares off the diagonal are the most interesting, as they highlight distinct snippets that the model finds to be semantically related.
* **Comparison**: Notice where the heatmaps agree and disagree. A bright square in the same position on all three maps indicates a strong consensus that two snippets are similar. Differences in scores reveal each model's unique bias and depth of understanding.


## **Conclusion**
The key takeaway is that there is **no single "best" model**; each one develops a unique "understanding" of code based on its architecture, training data, and fine-tuning objective.

The visualizations clearly show that **MiniLM**, **DistilRoBERTa**, and **MPNet** produce different semantic spaces.
* **MiniLM** provides a fast, general overview.
* **DistilRoBERTa** excels at finding functional parallels despite syntactic differences.
* **MPNet** often offers the most nuanced and accurate semantic grouping.

By comparing their outputs, a developer can choose the most appropriate model for a specific task, whether it's for building a fast code search engine, a precise duplicate code detector, or an advanced code classification system.



