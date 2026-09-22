# 🐍 Python Tutor ChatBot

An AI-powered **Python Tutor ChatBot** that helps beginners learn Python programming through simple explanations, examples, code snippets, code explanations, and key points.

The project uses the **Qwen2.5-3B-Instruct** Large Language Model with prompt engineering and provides an interactive **Gradio web interface**.

---

## 📌 Project Overview

Learning Python can be difficult for beginners when concepts are explained using complex technical terminology.

The **AI Python Tutor ChatBot** is designed to solve this problem by providing beginner-friendly explanations for Python programming questions.

A user can enter a Python question such as:

> What is a Python for loop?

The AI tutor generates an answer following a structured format:

1. Definition
2. Simple Explanation
3. Example
4. Python Code
5. Code Explanation
6. Key Point

The system always attempts to provide a small and understandable Python code example.

---

## 🎯 Objectives

The main objectives of this project are:

* Build an AI-powered Python learning assistant.
* Explain Python concepts in simple English.
* Help beginners understand programming concepts.
* Automatically generate Python examples.
* Explain generated Python code.
* Provide an interactive chatbot-style interface.
* Use a lightweight quantized LLM that can run efficiently in a Colab environment.

---

## ✨ Features

### 1. 🤖 AI-Powered Python Tutor

The chatbot uses **Qwen2.5-3B-Instruct** to answer Python-related questions.

### 2. 📖 Simple Explanations

The model is instructed to explain concepts using beginner-friendly language.

### 3. 💻 Python Code Examples

Every response is instructed to include a small Python code example.

### 4. 🔍 Code Explanation

The generated code is followed by an explanation so beginners can understand how the code works.

### 5. 📝 Structured Answers

The prompt forces the model to follow a consistent learning structure:

```text
Definition
Simple Explanation
Example
Python Code
Code Explanation
Key Point
```

### 6. 🎨 Gradio Interface

The project provides an easy-to-use web interface using **Gradio**.

### 7. ⚡ 4-bit Quantization

The Qwen model is loaded using 4-bit quantization to reduce memory usage.

### 8. 🧪 Example Questions

The interface provides example questions such as:

* What is a Python variable?
* What is a for loop?
* What is a Python function?
* What is a list?
* What is a dictionary?
* What is inheritance?
* What is exception handling?

---

## 🏗️ Project Architecture

```text
                 ┌───────────────────┐
                 │       USER        │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │ Python Question   │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │ Prompt Engineering│
                 └─────────┬─────────┘
                           │
                           ▼
              ┌─────────────────────────┐
              │ Qwen2.5-3B-Instruct     │
              │ Large Language Model    │
              └────────────┬────────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │ AI Generated      │
                 │ Answer            │
                 └─────────┬─────────┘
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
       Definition       Example         Code
                                         │
                                         ▼
                                Code Explanation
                                         │
                                         ▼
                                  Key Point
                                         │
                                         ▼
                              ┌─────────────────┐
                              │   Gradio UI     │
                              └─────────────────┘
```

---

## 🛠️ Technologies Used

| Technology                | Purpose                               |
| ------------------------- | ------------------------------------- |
| Python                    | Main programming language             |
| PyTorch                   | Deep learning framework               |
| Hugging Face Transformers | Loading and running the LLM           |
| Qwen2.5-3B-Instruct       | AI language model                     |
| BitsAndBytes              | 4-bit model quantization              |
| Accelerate                | Efficient model execution             |
| Gradio                    | Web-based user interface              |
| Google Colab              | Development and execution environment |

---

## 🤖 AI Model

### Qwen2.5-3B-Instruct

The project uses:

```text
Qwen/Qwen2.5-3B-Instruct
```

This is an instruction-following language model used to understand the user's Python question and generate a suitable response.

The model is loaded using:

```python
model_name = "Qwen/Qwen2.5-3B-Instruct"
```

---

## ⚡ Model Quantization

The project uses **4-bit quantization**:

```python
quant_config = BitsAndBytesConfig(
    load_in_4bit=True,
    bnb_4bit_compute_dtype=torch.float16,
    bnb_4bit_quant_type="nf4",
    bnb_4bit_use_double_quant=True
)
```

### Why use 4-bit quantization?

Quantization reduces the memory required to load the model.

This makes the project more suitable for environments such as **Google Colab**, where GPU memory can be limited.

The project uses:

* 4-bit loading
* FP16 computation
* NF4 quantization
* Double quantization

---

## 📦 Installation

Install the required libraries:

```bash
pip install -q transformers accelerate bitsandbytes gradio
```

The main dependencies are:

```text
transformers
accelerate
bitsandbytes
gradio
torch
```

---

## 📚 Import Libraries

```python
import torch

from transformers import (
    AutoTokenizer,
    AutoModelForCausalLM,
    BitsAndBytesConfig
)

import gradio as gr
```

---

## 🔄 Loading the Model

The tokenizer is loaded from Hugging Face:

```python
tokenizer = AutoTokenizer.from_pretrained(model_name)
```

The model is loaded using the quantization configuration:

```python
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    quantization_config=quant_config,
    device_map="auto"
)
```

The `device_map="auto"` allows the Transformers library to automatically determine where to place the model.

---

# 🧠 Python Tutor Logic

The main function of the project is:

```python
def python_tutor(question):
```

It receives a Python question from the user and generates an AI response.

---

## 📝 Prompt Engineering

The system prompt instructs the model to behave as an expert Python tutor:

```text
You are an expert Python tutor for beginners.

Your job is to explain Python concepts in very simple English.

Always follow this structure:

1. Definition
2. Simple Explanation
3. Example
4. Python Code
5. Code Explanation
6. Key Point

Always provide a small, correct Python code example.
Keep explanations beginner-friendly.
```

This prompt is important because it gives the model a specific role and response format.

---

## 🔄 Chat Template

The project uses the tokenizer's chat template:

```python
prompt = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True
)
```

This converts the system and user messages into the format expected by the instruction-tuned model.

---

## 🧮 Tokenization

The generated prompt is converted into tensors:

```python
inputs = tokenizer(
    prompt,
    return_tensors="pt"
).to(model.device)
```

These tensors are then provided to the model.

---

## 🤖 Text Generation

The model generates the answer using:

```python
outputs = model.generate(
    **inputs,
    max_new_tokens=400,
    temperature=0.7,
    top_p=0.9,
    do_sample=True
)
```

### Generation parameters

| Parameter        | Value | Purpose                          |
| ---------------- | ----: | -------------------------------- |
| `max_new_tokens` |   400 | Limits generated response length |
| `temperature`    |   0.7 | Controls randomness              |
| `top_p`          |   0.9 | Controls token sampling          |
| `do_sample`      |  True | Enables sampling                 |

---

## 📤 Decoding the Answer

The generated tokens are decoded into readable text:

```python
answer = tokenizer.decode(
    generated_tokens,
    skip_special_tokens=True
)
```

The final answer is returned:

```python
return answer
```

---

# 🧪 Testing the Model

The project tests the tutor with questions such as:

```python
question = "What is a Python for loop?"

answer = python_tutor(question)

print(answer)
```

Other test questions include:

```python
python_tutor("What is a Python list?")
```

```python
python_tutor("Explain Python functions")
```

```python
python_tutor("What is inheritance in Python?")
```

```python
python_tutor(
    "What is the difference between a list and tuple?"
)
```

---

# 💬 Command-Line Testing

The notebook also includes an interactive loop:

```python
while True:
    question = input("Ask a Python question: ")
    answer = python_tutor(question)
    print(answer)

    if question == "exit":
        break

print("good bye")
```

This allows the user to continuously ask Python questions until entering:

```text
exit
```

---

# 🎨 Gradio Interface

The project uses Gradio to create a web-based interface.

```python
def tutor_interface(question):

    if not question.strip():
        return "Please enter a Python question."

    return python_tutor(question)
```

The interface is created using:

```python
demo = gr.Interface(...)
```

---

## 🖥️ Gradio Interface Features

The interface contains:

### Input

A textbox where the user can enter a Python question.

### Output

A textbox displaying the generated AI answer.

### Title

```text
🐍 AI Python Tutor
```

### Description

The interface explains that the tutor provides:

* Simple explanation
* Example
* Python code
* Code explanation
* Key point

### Example Questions

The UI provides ready-made questions that users can click and test.

---

# ▶️ Running the Project

## Step 1: Open Google Colab

Open the notebook in Google Colab.

## Step 2: Install Dependencies

Run:

```python
!pip install -q transformers accelerate bitsandbytes gradio
```

## Step 3: Import Libraries

Run the import cell.

## Step 4: Load the Model

Run the Qwen model loading cell.

The first execution may take some time because the model needs to be downloaded.

## Step 5: Create the Python Tutor

Run the `python_tutor()` function.

## Step 6: Test the Model

Try:

```python
python_tutor("What is a Python variable?")
```

## Step 7: Launch Gradio

Run the final Gradio cell:

```python
demo.launch()
```

Gradio will provide an interface where users can enter Python questions.

---

# 📂 Project Structure

A simple GitHub repository can be organized as:

```text
Python-Tutor-ChatBot/
│
├── Python_Tutor_ChatBot.ipynb
│
├── README.md
│
└── requirements.txt
```

### `Python_Tutor_ChatBot.ipynb`

Contains the complete implementation of the project.

### `README.md`

Contains project documentation.

### `requirements.txt`

Contains the required Python packages.

Example:

```text
torch
transformers
accelerate
bitsandbytes
gradio
```

---

# 🔄 Project Workflow

```text
User enters Python question
          ↓
Question sent to Python Tutor
          ↓
System prompt defines tutor behavior
          ↓
Qwen2.5-3B-Instruct processes question
          ↓
Model generates response
          ↓
Generated tokens are decoded
          ↓
AI answer returned
          ↓
Gradio displays answer
```

---

# 💡 Example Interaction

### User

```text
What is a Python for loop?
```

### AI Tutor

```text
Definition:
A for loop is used to repeat a block of code for each item in a sequence.

Simple Explanation:
It allows us to execute the same code multiple times.

Example:
We can use a for loop to print numbers.

Python Code:

for i in range(5):
    print(i)

Code Explanation:
range(5) produces numbers from 0 to 4.
The loop runs once for each number.

Key Point:
A for loop is useful when you want to repeat code for multiple values.
```

---

# 🎓 Learning Topics

The tutor can be used to ask questions about topics such as:

### Python Basics

* Variables
* Data types
* Operators
* Input and output
* Type conversion

### Control Flow

* `if`
* `elif`
* `else`
* `for` loops
* `while` loops
* `break`
* `continue`

### Data Structures

* Lists
* Tuples
* Sets
* Dictionaries
* Strings

### Functions

* Function definition
* Parameters
* Arguments
* Return values
* Lambda functions

### Object-Oriented Programming

* Classes
* Objects
* Inheritance
* Encapsulation
* Polymorphism

### Exception Handling

* `try`
* `except`
* `finally`
* `raise`

---

# 🔐 Important Note

This project is an educational AI assistant. Generated answers should be checked when learning concepts, especially for advanced or complex programming problems.

The model can sometimes generate incorrect or incomplete code because language models may produce inaccurate outputs.

---

# ⚠️ Limitations

The current project has some limitations:

* It focuses primarily on Python learning.
* It does not execute the generated Python code.
* It does not automatically verify whether generated code is correct.
* The quality of answers depends on the language model.
* Large model downloads can take time.
* GPU resources may be required for comfortable execution.
* The current interface does not maintain a persistent conversation history.

---

# 🚀 Future Enhancements

The project can be improved by adding:

### 1. 💬 Chat History

Store previous questions and answers so users can maintain a conversation.

### 2. ▶️ Python Code Execution

Allow users to execute generated Python code safely.

### 3. 🐞 Code Debugger

Users could submit Python code and receive:

* Error identification
* Error explanation
* Corrected code
* Explanation of the correction

### 4. 📊 Difficulty Levels

Add:

```text
Beginner
Intermediate
Advanced
```

The explanation could change according to the selected level.

### 5. 📝 Quiz Generator

Generate Python quizzes automatically.

### 6. 🎯 Practice Questions

Generate coding exercises based on the topic being studied.

### 7. 📈 Learning Progress

Track the topics learned by the user.

### 8. 🌐 Multilingual Support

Provide explanations in languages such as:

* English
* Tamil
* Hindi
* Telugu
* Malayalam

### 9. 📚 RAG-Based Learning

Add a Python documentation knowledge base using Retrieval-Augmented Generation (RAG) to provide answers based on trusted learning materials.

### 10. 🎤 Voice Interaction

Allow users to ask Python questions using voice input.

---

# 🔮 Future Architecture

An extended version could use:

```text
                 USER
                   │
          ┌────────┴────────┐
          │                 │
       Text Input       Voice Input
          │                 │
          └────────┬────────┘
                   ▼
             Python Tutor
                   │
                   ▼
            RAG / Knowledge
              Retrieval
                   │
                   ▼
          Qwen Language Model
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
    Explanation   Code      Quiz
        │          │          │
        └──────────┼──────────┘
                   ▼
              Gradio UI
                   │
                   ▼
                 USER
```

---

# 📌 Use Cases

This project can be useful for:

* Python beginners
* College students
* CSE students
* Programming learners
* Interview preparation
* Python practice
* Quick concept revision
* Coding education

---

# 🎓 Academic Project

### Project Title

**AI Python Tutor ChatBot Using Qwen2.5-3B-Instruct**

### Domain

```text
Artificial Intelligence
Generative AI
Natural Language Processing
Python Programming
Large Language Models
```

### Project Type

```text
AI-based Educational Application
```

---

# 📊 Key Technologies

```text
Python
   ↓
PyTorch
   ↓
Hugging Face Transformers
   ↓
Qwen2.5-3B-Instruct
   ↓
4-bit Quantization
   ↓
Prompt Engineering
   ↓
Gradio
   ↓
AI Python Tutor
```

---

# 👨‍💻 How the Project Works

The project follows these main steps:

**Step 1:** The user enters a Python question.

**Step 2:** The question is combined with a system prompt that defines the AI's tutoring behavior.

**Step 3:** The prompt is converted into the format expected by Qwen2.5-3B-Instruct.

**Step 4:** The quantized Qwen model generates an answer.

**Step 5:** Generated tokens are decoded into readable text.

**Step 6:** The answer is returned to the user.

**Step 7:** Gradio displays the response through a simple web interface.

---

# ⭐ Project Highlights

* ✅ AI-powered Python tutor
* ✅ Qwen2.5-3B-Instruct
* ✅ 4-bit quantization
* ✅ Prompt engineering
* ✅ Beginner-friendly explanations
* ✅ Automatic Python examples
* ✅ Code explanations
* ✅ Interactive Gradio interface
* ✅ Google Colab compatible
* ✅ Easy to extend

---

# 📜 License

This project is intended for educational and learning purposes.

The Qwen model and its associated components are subject to their respective licenses and terms. Check the model's official documentation before using the project for commercial purposes.

---

# 🙏 Acknowledgement

This project uses open-source technologies including:

* Hugging Face Transformers
* Qwen2.5-3B-Instruct
* PyTorch
* BitsAndBytes
* Accelerate
* Gradio

---

# 👤 Author

**G.T. Vasanthaprabhu**

**AI / Python / Generative AI Project**

---

## ⭐ Conclusion

The **AI Python Tutor ChatBot** demonstrates how a modern instruction-following Large Language Model can be combined with prompt engineering and Gradio to create an interactive educational application.

The system transforms a simple Python question into a structured learning response containing a definition, explanation, example, Python code, code explanation, and key point.

It provides a strong foundation for developing a more advanced **AI-powered programming tutor** with code execution, debugging, quizzes, RAG, multilingual support, and personalized learning in future versions.
