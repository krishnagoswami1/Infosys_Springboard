# MileStone 2 : Code generation with Interactive UI's and Visualizations

This Jupyter notebook demonstrates how to **evaluate and analyze large language models (LLMs)** such as those hosted on **Hugging Face Hub**. It combines the process of **loading, running, and benchmarking text-generation models** with **code quality and maintainability assessment** using `radon`.




## 🌟 Overview

This project provides a comprehensive toolkit for evaluating and comparing small-to-medium-sized Large Language Models on code generation tasks. It moves beyond simple output checks by employing quantitative software metrics to deliver an objective analysis of model performance.

The notebook features:

  * **🧪 Head-to-Head Comparison**: Benchmarks four different code-generation models: `DeepSeek-Coder`, `Phi-2`, `Gemma`, and `Stable-code`.
  * **💻 Interactive UIs**: Two user interfaces built with `ipywidgets` allow for real-time, on-demand testing.
  * **📊 Advanced Code Metrics**: Utilizes the `radon` library to analyze generated code for Cyclomatic Complexity, Maintainability Index, and Lines of Code.
  * **⏱️ Performance Analysis**: Measures and compares the inference speed (generation time) of each model.
  * **📈 Automated Testing & Visualization**: A full test suite runs 16 standard coding prompts across all models and generates plots for a clear, high-level comparison.


## 🚀 Notebook Workflow

The Jupyter Notebook is structured to guide the user from setup to final analysis in a logical sequence.

| Step | Section                               | Description                                                                                                                                                                                                            |
| :--: | ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1** | **Setup & Authentication** | Installs all required libraries (`transformers`, `torch`, `radon`, etc.) and authenticates with Hugging Face to download the model weights.                                                                     |
| **2** | **Core Engine** | Defines the essential functions for the entire process: cleaning model output, validating Python syntax with `ast`, calculating code metrics with `radon`, and the main `generate_code` function.              |
| **3** | **Model Pre-loading** | To ensure a smooth user experience, all four models and their tokenizers are pre-loaded into GPU memory using memory-efficient `bfloat16` precision.                                                            |
| **4** | **Interactive Dashboards** | Deploys two `ipywidgets` UIs: one for benchmarking a prompt against all models at once, and another for comparing a custom selection of models.                                                                   |
| **5** | **Automated Benchmark** | Executes a predefined suite of 16 coding prompts against every model. This process gathers robust data for an objective, multi-faceted comparison.                                                              |
| **6** | **Visualization** | After the automated tests are complete, the notebook aggregates the results and generates a series of bar plots to visually compare the average performance of each model across all key metrics.               |

-----

## 🤖 Models Under the Microscope

The notebook evaluates four prominent models, each with unique characteristics.

### 1\. DeepSeek-Coder-1.3B

  * **Developer**: DeepSeek AI.
  * **Description**: DeepSeek Coder is part of a family of models specialized in code. It was trained from scratch on an enormous 8 trillion token dataset, with a significant portion (2 trillion tokens) dedicated to code from over 80 programming languages. Its architecture is based on LLaMA, and it's fine-tuned to excel at following programming-related instructions.

### 2\. Phi-2-2.7B

  * **Developer**: Microsoft Research.
  * **Description**: Phi-2 is a small model that punches well above its weight class, achieving performance comparable to models many times its size. Its key advantage lies in its training data, which is described as "textbook-quality." By focusing on high-quality, curated educational and synthetic data instead of unfiltered web scrapes, Phi-2 has developed strong reasoning and language understanding capabilities.

### 3\. Gemma-2B-IT

  * **Developer**: Google.
  * **Description**: Gemma is a lightweight, open-source model family built from the same research that created the powerful Gemini models. The **-IT** suffix means this version is **Instruction Tuned**. It has undergone extensive fine-tuning to become adept at following user commands and prompts, making it highly suitable for tasks like generating code from a natural language description.

### 4\. Stable-code-3b

  * **Developer**: Stability AI.
  * **Description**: Stable Code 3B is a 3-billion-parameter model specifically engineered for code-related tasks and designed to run efficiently on commodity hardware without requiring a dedicated GPU. It was trained on a diverse dataset of software engineering data, making it a strong candidate for code completion and generation.


## 📊 Benchmarking Methodology Explained

The comparison is grounded in four objective metrics that evaluate the generated code from different perspectives: **Speed**, **Complexity**, **Maintainability**, and **Verbosity**.



## ⏱️ 1. Generation Time

**Generation Time** is the most straightforward performance metric. It measures the real-world time, in seconds, that a model takes to process a prompt and produce a complete response.

  * **What it Measures**: The model's inference speed and computational efficiency.
  * **Why it Matters**: In practical applications like real-time code assistants or pair programming tools, low latency is crucial for a good user experience. A model that generates code quickly is more useful than a slow one, even if their code quality is similar.
  * **The Mathematics**: It's a simple calculation of the difference between two timestamps.
    > $$\text{Generation Time} = \text{End Time} - \text{Start Time}$$
  * **Interpretation**: A **lower** value is always better, indicating a faster, more efficient model.



## 🧠 2. Cyclomatic Complexity (CC)

**Cyclomatic Complexity** is a software metric used to indicate the complexity of a program. It directly measures the number of linearly independent paths through a program's source code. Think of it as counting the number of "decision points" in your code.

  * **What it Measures**: The structural or logical complexity of the code.

  * **Why it Matters**: Code with high complexity is often harder for humans to understand, test, and maintain. Each independent path represents a test case needed to fully cover the code's logic. A lower complexity score generally implies simpler, more manageable code.

  * **The Mathematics**: The complexity, denoted as $M$, is calculated from the code's control-flow graph (a graph of all paths a program might take). The formula is:

    > $$M = E - N + 2P$$
    > Where:

    >   * **$E$** is the number of edges (transfers of control).
    >   * **$N$** is the number of nodes (computational statements).
    >   * **$P$** is the number of connected components (e.g., a single function or program).

  * **Interpretation**: A **lower** score is generally better.

      * **1-10**: Simple, well-structured, low-risk code.
      * **11-20**: Moderately complex code.
      * **\>20**: Highly complex, potentially difficult to test and maintain.



## 🛠️ 3. Maintainability Index (MI)

The **Maintainability Index** is a composite metric that provides a single, easy-to-understand score for how maintainable a piece of code is. It combines several other metrics to produce a score, typically on a scale from 0 to 100.

  * **What it Measures**: The overall ease of understanding, modifying, and supporting the code.

  * **Why it Matters**: This is arguably the most holistic measure of "code quality" in this benchmark. High maintainability means that a developer can work with the code more easily, reducing the likelihood of introducing new bugs when making changes.

  * **The Mathematics**: The `radon` library uses a formula adapted from the original SEI (Software Engineering Institute) definition. The original formula is:

    > $$MI = 171 - 5.2 \times \ln(V) - 0.23 \times G - 16.2 \times \ln(L)$$
    > Where:

    >   * **$V$** is the **Halstead Volume**, a measure of the program's size based on its vocabulary (unique operators and operands).
    >   * **$G$** is the **Cyclomatic Complexity**.
    >   * **$L$** is the **Lines of Code**.

  * **Interpretation**: A **higher** score is always better.

      * **85-100**: Excellent maintainability.
      * **65-84**: Good maintainability.
      * **\<65**: Poor maintainability (the code is likely difficult to work with).



## 📏 4. Lines of Code (LOC)

**Lines of Code** is the most basic measure of a program's size. The `radon` library specifically calculates the logical lines of code, meaning it ignores comments and blank lines to focus only on executable statements.

  * **What it Measures**: The verbosity or succinctness of the generated code.
  * **Why it Matters**: While not a direct measure of quality, LOC provides insight into a model's coding "style." Some models might produce very verbose, step-by-step solutions, while others might favor more concise, modern syntax (like list comprehensions or ternary operators). Comparing LOC can reveal which models generate more compact and potentially more readable code.
  * **The Mathematics**: A simple count of executable lines.
  * **Interpretation**: The ideal LOC is context-dependent. Sometimes a few extra lines improve clarity, while other times, conciseness is better. In this benchmark, it's used as a comparative metric to analyze the **verbosity** of each model's solution.

## 📊 Observations: 
* **Cyclomatic Complexity**
* <img width="1086" height="505" alt="image" src="https://github.com/user-attachments/assets/c112a2c6-9ba0-4133-8601-bb0733d6e398" />
* **Maintainability Index**
* <img width="1099" height="508" alt="image" src="https://github.com/user-attachments/assets/779ff2af-b9d0-4b84-9afe-6fd8fca5ba96" />
* **Avg Lines of Code**
* <img width="1101" height="516" alt="image" src="https://github.com/user-attachments/assets/151b5172-9737-4381-9838-ce5d1e4b2fb2" />
* **Lines of Code generated per second**
* <img width="1105" height="513" alt="image" src="https://github.com/user-attachments/assets/1cc9526b-6d77-486e-a7bf-d788e3d9e454" />

## Conclusions: 
