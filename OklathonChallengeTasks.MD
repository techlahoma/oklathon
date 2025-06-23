Of course\! This is an excellent idea. Using a specific, provided API makes the challenges much more focused and relevant to the resources you have on hand.

The documentation you linked for the **NRP AI Managed LLM API** is perfect. The most important detail is that it's **compatible with the OpenAI API specification**. This makes it incredibly easy to use with standard tools and code examples.

Here is a set of beginner-friendly, 1-hour challenges tailored specifically to use this API.

-----

### **Getting Started: The Code for Every Challenge**

For every task below, you will start with the following Python code in a [Google Colab](https://colab.research.google.com/) notebook. This code handles the connection to the NRP AI API.

**Instructions for Participants:**

1.  Copy this code into a new Google Colab notebook.
2.  Get the `NRP_API_KEY` from an event organizer.
3.  In Colab, click the "🔑" (Secrets) icon on the left, create a new secret named `NRP_API_KEY`, and paste your key there. This keeps it secure.

<!-- end list -->

```python
import os
import requests
from google.colab import userdata

# --- Configuration ---
# This safely gets your API key from Colab's Secret Manager.
try:
    NRP_API_KEY = userdata.get('NRP_API_KEY')
except userdata.SecretNotFoundError:
    raise RuntimeError("API Key not found. Please add it to Colab Secrets as 'NRP_API_KEY'.")

NRP_API_BASE = "https://llm.nrp.ai/v1"

# You can change this to any model available in the NRP AI docs!
# Let's start with a fast and capable one.
MODEL_NAME = "meta-llama/Llama-3-8b-chat-hf"

headers = {
    "Authorization": f"Bearer {NRP_API_KEY}"
}

def ask_the_ai(system_prompt, user_prompt, model=MODEL_NAME):
    """A function to call the NRP AI chat completions endpoint."""
    data = {
        "model": model,
        "messages": [
            {"role": "system", "content": system_prompt},
            {"role": "user", "content": user_prompt}
        ]
    }

    try:
        response = requests.post(
            f"{NRP_API_BASE}/chat/completions",
            headers=headers,
            json=data
        )
        response.raise_for_status()  # Raise an exception for bad status codes
        return response.json()['choices'][0]['message']['content']
    except requests.exceptions.RequestException as e:
        return f"An error occurred: {e}\nResponse: {response.text}"

# --- Your Challenge Code Goes Below This Line ---

```

-----

### **Challenge Board: 1-Hour NRP AI API Tasks**

#### **Task 1: The First Connection**

  * **Your 1-Hour Goal:** Successfully connect to the API and get your first response from the AI.
  * **Step-by-Step Guide:**
    1.  After the boilerplate code, add these two lines and run the cell:
        ```python
        response = ask_the_ai("You are a helpful assistant.", "Tell me a fun fact about the history of Oklahoma City.")
        print(response)
        ```
  * **What You Learned:** How to make a basic API call and receive a generated response. You're officially an AI developer\!

#### **Task 2: The AI Persona Switch**

  * **Your 1-Hour Goal:** Make the AI adopt two different personalities using the `system_prompt`.
  * **Step-by-Step Guide:**
    1.  First, make the AI a pirate. Run this code:
        ```python
        system_prompt = "You are a sarcastic, world-weary pirate. You answer all questions as such."
        user_prompt = "What should I bring to a hackathon?"
        print(f"PIRATE:\n{ask_the_ai(system_prompt, user_prompt)}")
        ```
    2.  Now, make the AI a cheerful robot. Run this code and compare the answers:
        ```python
        system_prompt = "You are BEEP-BOOP, a cheerful robot assistant. You are enthusiastic and helpful."
        user_prompt = "What should I bring to a hackathon?"
        print(f"\nROBOT:\n{ask_the_ai(system_prompt, user_prompt)}")
        ```
  * **What You Learned:** How the **System Prompt** is the most powerful tool for controlling an AI's behavior, tone, and personality.

#### **Task 3: The Slogan Generator**

  * **Your 1-Hour Goal:** Use the AI as a creative partner to brainstorm tourism slogans.
  * **Step-by-Step Guide:**
    1.  Define a creative role for the AI.
    2.  Ask it to generate 5 catchy slogans for Oklahoma's Route 66.
        ```python
        system_prompt = "You are a world-class advertising executive. You create short, memorable slogans."
        user_prompt = "Generate 5 catchy tourism slogans for the historic Route 66 in Oklahoma."
        print(ask_the_ai(system_prompt, user_prompt))
        ```
  * **What You Learned:** How to use LLMs for creative brainstorming and text generation.

#### **Task 4: The Code Explainer**

  * **Your 1-Hour Goal:** Get the AI to explain a piece of Python code in plain English.
  * **Step-by-Step Guide:**
    1.  Give the AI the role of an expert teacher.
    2.  Paste a small piece of code into the user prompt and ask for an explanation.
        ````python
        system_prompt = "You are an expert Python programmer who excels at explaining code to beginners."
        code_snippet = "short_names = [name for name in names if len(name) < 5]"
        user_prompt = f"Please explain what this line of Python code does in simple terms: ```{code_snippet}```"
        print(ask_the_ai(system_prompt, user_prompt))
        ````
  * **What You Learned:** How to use an LLM as a powerful learning and programming assistant.

#### **Task 5: The Data Extractor**

  * **Your 1-Hour Goal:** Turn a messy sentence into a clean, structured JSON object.
  * **Step-by-Step Guide:**
    1.  Instruct the AI to *only* respond with JSON. This is key.
    2.  Give it a sentence and ask it to extract the information.
        ```python
        system_prompt = "You are a data processing robot. You only respond with perfectly formatted JSON. You never add extra commentary."
        user_prompt = "From this text: 'The project showcase for Oklathon starts Sunday, July 20th at 9:00 AM', extract the event name, day, date, and time."
        print(ask_the_ai(system_prompt, user_prompt))
        ```
  * **What You Learned:** How to use LLMs for structured data extraction, a very common and powerful real-world application.

#### **Task 6: The Model Taster**

  * **Your 1-Hour Goal:** Compare the responses from two different AI models available on the NRP platform.
  * **Step-by-Step Guide:**
    1.  Look at the NRP AI documentation and find another model name besides `meta-llama/Llama-3-8b-chat-hf` (for example, a model from Mistral or another family).
    2.  Ask both models the same creative question and compare their "personalities."
        ```python
        user_prompt = "Write a four-line poem about a summer thunderstorm in Oklahoma."

        print("--- Calling Llama 3 ---")
        print(ask_the_ai("You are a poet.", user_prompt, model="meta-llama/Llama-3-8b-chat-hf"))

        # Replace 'ANOTHER_MODEL_NAME' with a real model name from the docs
        print("\n--- Calling Another Model ---")
        print(ask_the_ai("You are a poet.", user_prompt, model="mistralai/Mistral-7B-Instruct-v0.2"))
        ```
  * **What You Learned:** That not all models are the same\! They have different training, styles, and creative abilities.
