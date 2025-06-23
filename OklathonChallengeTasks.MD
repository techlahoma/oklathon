# **Oklathon 2025: API Challenge Board**

Welcome to the **Oklathon 2025 "Climb the Challenge Board"** track\! These short, 1-hour challenges are designed to get you started with the NRP AI API.

### **Before you begin:**

1. Get your **NRP\_API\_KEY** from an event organizer.  
2. Go to [colab.research.google.com](https://colab.research.google.com "null") and click **"New notebook"**.
3. In your new notebook, you must add your API key as a **'secret'** to keep it secure. In Colab, click the key icon (**🔑**) on the left sidebar, click '**Add a new secret**', name it **NRP\_API\_KEY**, paste your key in the '**Value**' field, and ensure the '**Notebook access**' toggle is on.

```python
# ==============================================================================
# Oklathon 2025 - NRP AI API Challenge Boilerplate
#
# Instructions for Participants:
# 1.  Copy this entire code block into a new Google Colab notebook.
# 2.  Get the NRP_API_KEY from an event organizer.
# 3.  In Colab, click the "🔑" (Secrets) icon on the left panel,
#     create a new secret named `NRP_API_KEY`, and paste your key there.
#     Make sure the "Notebook access" toggle is on.
# ==============================================================================

import os
import requests # We need this for listing the models
from openai import OpenAI, APIError

# --- Configuration ---
# This safely gets your API key from Colab's Secret Manager.
try:
    from google.colab import userdata
    NRP_API_KEY = userdata.get('NRP_API_KEY')
except (ImportError, ModuleNotFoundError):
    # Fallback for running locally if you set an environment variable
    NRP_API_KEY = os.environ.get("NRP_API_KEY")
    if not NRP_API_KEY:
      raise RuntimeError("API Key not found. Please set the NRP_API_KEY environment variable or run in Google Colab with secrets.")
except userdata.SecretNotFoundError:
    raise RuntimeError("API Key not found. Please add it to Colab Secrets as 'NRP_API_KEY'.")

# Define the base URL and initialize the OpenAI client
BASE_URL = "https://llm.nrp-nautilus.io/v1"
client = OpenAI(
    api_key=NRP_API_KEY,
    base_url=BASE_URL
)

# --- Challenge Code Goes Below This Line ---

```

### **Task 0: See Your Available Models (IMPORTANT: RUN THIS FIRST\!)**

**Goal:** Ask the API which models your key has access to. You'll need one of these model names for all the other tasks.

**Why this is important:** Not all API keys have access to all models. This task lets you see your personalized list so you know what you can use.

 <details>
     <summary>Spoiler warning! (It's OK to peek at this one!)</summary>

[▶️ **Run this task in Google Colab**](https://colab.research.google.com/notebooks/basic.ipynb)


```python
# ==============================================================================
# Task 0: See Your Available Models (IMPORTANT: RUN THIS FIRST!)
# Goal: Ask the API which models your key has access to.
# ==============================================================================
print("--- Task 0: See Your Available Models ---")
try:
    # The 'openai' library has a method for this, let's use it!
    models_list = client.models.list()
    print("Your API key has access to the following models:")
    for model in models_list.data:
        print(f"- {model.id}")
except APIError as e:
    print(f"An API error occurred while trying to list models: {e}")
except Exception as e:
    print(f"An unexpected error occurred: {e}")
```

</details>

### **General Purpose & Chat Models**

These are your all-rounders, great for conversation, coding, and creative tasks.

* **`llama3` / `llama3-sdsc`**: This is **Llama 3** from Meta, one of the most powerful and popular open models available. It's an excellent choice for almost any task, from writing code to creative text generation. The `sdsc` version is likely the same model hosted by the San Diego Supercomputer Center.
    * **Best for:** A high-quality, reliable default for almost any task.

* **`phi3`**: This is **Phi-3** from Microsoft. Its main advantage is being incredibly powerful for its small size. It can run on smaller devices but still provides very high-quality, coherent answers.
    * **Best for:** Getting fast, quality responses without needing the absolute largest model.

* **`gemma3`**: This is likely a typo for Google's **Gemma** family of models. Similar to Llama and Phi, they are strong, open models from a major lab. If you meant the original Gemma, it's known for being a solid, reliable performer.
    * **Best for:** Another strong alternative to Llama 3 for general tasks.

* **`kimi`**: A model from a company called Kimi AI (or Moonshot AI). Its claim to fame is having an extremely large context window, meaning it can "remember" and reason over very long documents or conversations.
    * **Best for:** Summarizing long articles, asking questions about a large PDF, or maintaining a long, complex conversation.

* **`olmo`**: This is a truly open language model from the Allen Institute for AI (AI2). "Open" here means the training data and methods are also available, which is important for researchers.
    * **Best for:** General tasks, with a focus on reproducibility and research transparency.

* **`DeepSeek-R1-Distill-Qwen-32B` / `deepseek-r1-qwen-qualcomm`**: These are from the **DeepSeek** family, which is particularly famous for being excellent at coding tasks.
    * The long name tells a story: It's likely a **distilled** (made smaller and faster) version of a larger model, potentially combined with techniques from the **Qwen** model family, and has **32 billion** parameters. The `qualcomm` version is likely optimized to run well on Qualcomm hardware (like modern phone chips).
    * **Best for:** Code generation, debugging, and programming-related questions.

---

### **Specialized & Tool-Use Models**

These models are designed for specific jobs beyond just chatting.

* **`groq-tools`**: This isn't a model name itself, but a reference to a model running on **Groq's** hardware. Groq makes special chips (LPUs) that run AI models incredibly fast. A `-tools` model is specifically fine-tuned to be good at using external tools, like calling functions or APIs.
    * **Best for:** Building applications where the AI needs to interact with other software or APIs at very high speed.

* **`gorilla`**: A model from UC Berkeley that is an expert at calling APIs. It's often more accurate than general-purpose models at figuring out which API to call and how to format the request correctly.
    * **Best for:** Any task that requires reliable API calls or function execution.

* **`llava-onevision`**: **LLaVA** is a multimodal model. This means it can understand **both text and images**. You can give it an image and ask questions about it.
    * **Best for:** Describing what's in a picture, answering questions based on a chart, or creating text related to an image.

* **`embed-mistral`**: This is **not a chat model**. It’s an **embedding model**. Its only job is to turn text into a list of numbers (a "vector"). These numbers represent the semantic meaning of the text.
    * **Best for:** Powering semantic search (finding documents by meaning, not just keywords) or Retrieval-Augmented Generation (RAG) systems. Think of it as a librarian that organizes documents by topic so a chat model can find them easily.

* **`watt`**: This is a less common public name. It could be an internal name for a specialized model or a fine-tuned version of another model available on the NRP platform. Without more context, its specialty is unclear.


**Add  the below code to your task, and specify which model will work the best.**

```python
# IMPORTANT: Replace the model name below with one from your list from Task 0 that best suits your purpose!
MODEL_TO_USE = "llama3" # <-- CHANGE THIS IF NEEDED
```


### **Task 1: The First Connection**

**Goal:** Successfully connect to the API and get your first response from the AI.

**What you'll learn:** How to make a basic API call using the openai library and receive a generated response from the NRP AI endpoint. This is the "Hello, World\!" of using this API.

**Instructions:**

1. Run **Task 0** first and get a model name from the output.  
2. Copy that model name and paste it into the **MODEL\_TO\_USE** variable in the code below.

 <details>
     <summary>Spoiler warning</summary>

[▶️ **Run this task in Google Colab**](https://colab.research.google.com/notebooks/basic.ipynb?code=import+os%0Afrom+openai+import+OpenAI,+APIError%0A%0A%23+---+Configuration+---%0A%23+This+safely+gets+your+API+key+from+Colab's+Secret+Manager.%0Atry:%0A++++from+google.colab+import+userdata%0A++++NRP_API_KEY+%3D+userdata.get\('NRP_API_KEY'\)%0Aexcept+\(ImportError,+ModuleNotFoundError\):%0A++++NRP_API_KEY+%3D+os.environ.get\(%22NRP_API_KEY%22\)%0A++++if+not+NRP_API_KEY:%0A++++++raise+RuntimeError\(%22API+Key+not+found.+Please+set+the+NRP_API_KEY+environment+variable+or+run+in+Google+Colab+with+secrets.%22\)%0Aexcept+userdata.SecretNotFoundError:%0A++++raise+RuntimeError\(%22API+Key+not+found.+Please+add+it+to+Colab+Secrets+as+'NRP_API_KEY'.%22\)%0A%0A%23+Initialize+the+OpenAI+client+to+point+to+the+NRP+AI+server%0Aclient+%3D+OpenAI\(%0A++++api_key%3DNRP_API_KEY,%0A++++base_url%3D%22https://llm.nrp-nautilus.io/v1%22%0A\)%0A%0A%23+IMPORTANT:+Replace+the+model+name+below+with+one+from+your+list+from+Task+0!%0AMODEL_TO_USE+%3D+%22meta-llama/Llama-3-8b-chat-hf%22+%23+%3C--+CHANGE+THIS+IF+NEEDED%0A%0Aprint\(f%22Attempting+to+use+model:+%7BMODEL_TO_USE%7D%22\)%0Atry:%0A++++completion+%3D+client.chat.completions.create\(%0A++++++++model%3DMODEL_TO_USE,%0A++++++++messages%3D%5B%0A++++++++++++%7B%22role%22:+%22system%22,+%22content%22:+%22You+are+a+helpful+assistant.%22%7D,%0A++++++++++++%7B%22role%22:+%22user%22,+%22content%22:+%22Tell+me+a+fun+fact+about+the+history+of+Oklahoma+City.%22%7D%0A++++++++%5D%0A++++\)%0A++++print\(completion.choices%5B0%5D.message.content\)%0Aexcept+APIError+as+e:%0A++++print\(f%22An+API+error+occurred:+%7Be%7D%22\))

```python
# ==============================================================================
# Task 1: The First Connection
# Goal: Successfully connect to the API and get your first response.
# ==============================================================================
print("\n--- Task 1: The First Connection ---")

print(f"Attempting to use model: {MODEL_TO_USE}")
try:
    completion = client.chat.completions.create(
        model=MODEL_TO_USE,
        messages=[
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": "Tell me a fun fact about the history of Oklahoma City."}
        ]
    )
    print(completion.choices[0].message.content)
except APIError as e:
    print(f"An API error occurred: {e}")
```

</details>

### **Task 2: The AI Persona Switch**

**Goal:** Make the AI adopt two different personalities using the **system** prompt. This shows how you can control the AI's tone and behavior.

 <details>
     <summary>Spoiler warning</summary>

[▶️ **Run this task in Google Colab**](https://colab.research.google.com/notebooks/basic.ipynb?code=import+os%0Afrom+openai+import+OpenAI,+APIError%0A%0A%23+---+Configuration+---%0A%23+This+safely+gets+your+API+key+from+Colab's+Secret+Manager.%0Atry:%0A++++from+google.colab+import+userdata%0A++++NRP_API_KEY+%3D+userdata.get\('NRP_API_KEY'\)%0Aexcept+\(ImportError,+ModuleNotFoundError\):%0A++++NRP_API_KEY+%3D+os.environ.get\(%22NRP_API_KEY%22\)%0A++++if+not+NRP_API_KEY:%0A++++++raise+RuntimeError\(%22API+Key+not+found.+Please+set+the+NRP_API_KEY+environment+variable+or+run+in+Google+Colab+with+secrets.%22\)%0Aexcept+userdata.SecretNotFoundError:%0A++++raise+RuntimeError\(%22API+Key+not+found.+Please+add+it+to+Colab+Secrets+as+'NRP_API_KEY'.%22\)%0A%0A%23+Initialize+the+OpenAI+client+to+point+to+the+NRP+AI+server%0Aclient+%3D+OpenAI\(%0A++++api_key%3DNRP_API_KEY,%0A++++base_url%3D%22https://llm.nrp-nautilus.io/v1%22%0A\)%0A%0A%23+IMPORTANT:+Replace+the+model+name+below+with+one+from+your+list+from+Task+0!%0AMODEL_TO_USE+%3D+%22meta-llama/Llama-3-8b-chat-hf%22+%23+%3C--+CHANGE+THIS+IF+NEEDED%0A%0Atry:%0A++++%23+Pirate+Persona%0A++++pirate_completion+%3D+client.chat.completions.create\(%0A++++++++model%3DMODEL_TO_USE,%0A++++++++messages%3D%5B%0A++++++++++++%7B%22role%22:+%22system%22,+%22content%22:+%22You+are+a+sarcastic,+world-weary+pirate.+You+answer+all+questions+as+such.%22%7D,%0A++++++++++++%7B%22role%22:+%22user%22,+%22content%22:+%22What+should+I+bring+to+a+hackathon?%22%7D%0A++++++++%5D%0A++++\)%0A++++print\(%22PIRATE+SAYS:%22\)%0A++++print\(pirate_completion.choices%5B0%5D.message.content\)%0A%0A++++%23+Robot+Persona%0A++++robot_completion+%3D+client.chat.completions.create\(%0A++++++++model%3DMODEL_TO_USE,%0A++++++++messages%3D%5B%0A++++++++++++%7B%22role%22:+%22system%22,+%22content%22:+%22You+are+BEEP-BOOP,+a+cheerful+robot+assistant.+You+are+enthusiastic+and+helpful.%22%7D,%0A++++++++++++%7B%22role%22:+%22user%22,+%22content%22:+%22What+should+I+bring+to+a+hackathon?%22%7D%0A++++++++%5D%0A++++\)%0A++++print\(%22%5CnROBOT+SAYS:%22\)%0A++++print\(robot_completion.choices%5B0%5D.message.content\)%0Aexcept+APIError+as+e:%0A++++print\(f%22An+API+error+occurred:+%7Be%7D%22\))
```python
# ==============================================================================
# Task 2: The AI Persona Switch
# Goal: Make the AI adopt two different personalities using the "system" prompt.
# ==============================================================================
print("\n--- Task 2: The AI Persona Switch ---")
try:
    # Pirate Persona
    pirate_completion = client.chat.completions.create(
        model=MODEL_TO_USE,
        messages=[
            {"role": "system", "content": "You are a sarcastic, world-weary pirate. You answer all questions as such."},
            {"role": "user", "content": "What should I bring to a hackathon?"}
        ]
    )
    print("PIRATE SAYS:")
    print(pirate_completion.choices[0].message.content)

    # Robot Persona
    robot_completion = client.chat.completions.create(
        model=MODEL_TO_USE,
        messages=[
            {"role": "system", "content": "You are BEEP-BOOP, a cheerful robot assistant. You are enthusiastic and helpful."},
            {"role": "user", "content": "What should I bring to a hackathon?"}
        ]
    )
    print("\nROBOT SAYS:")
    print(robot_completion.choices[0].message.content)
except APIError as e:
    print(f"An API error occurred: {e}")
```
</details>

### **Task 3: The Code Explainer**

**Goal:** Get the AI to explain a piece of Python code in plain English. This is a great way to use AI as a learning and debugging tool.

 <details>
     <summary>Spoiler warning</summary>

[▶️ **Run this task in Google Colab**](https://colab.research.google.com/notebooks/basic.ipynb?code=import+os%0Afrom+openai+import+OpenAI,+APIError%0A%0A%23+---+Configuration+---%0A%23+This+safely+gets+your+API+key+from+Colab's+Secret+Manager.%0Atry:%0A++++from+google.colab+import+userdata%0A++++NRP_API_KEY+%3D+userdata.get\('NRP_API_KEY'\)%0Aexcept+\(ImportError,+ModuleNotFoundError\):%0A++++NRP_API_KEY+%3D+os.environ.get\(%22NRP_API_KEY%22\)%0A++++if+not+NRP_API_KEY:%0A++++++raise+RuntimeError\(%22API+Key+not+found.+Please+set+the+NRP_API_KEY+environment+variable+or+run+in+Google+Colab+with+secrets.%22\)%0Aexcept+userdata.SecretNotFoundError:%0A++++raise+RuntimeError\(%22API+Key+not+found.+Please+add+it+to+Colab+Secrets+as+'NRP_API_KEY'.%22\)%0A%0A%23+Initialize+the+OpenAI+client+to+point+to+the+NRP+AI+server%0Aclient+%3D+OpenAI\(%0A++++api_key%3DNRP_API_KEY,%0A++++base_url%3D%22https://llm.nrp-nautilus.io/v1%22%0A\)%0A%0A%23+IMPORTANT:+Replace+the+model+name+below+with+one+from+your+list+from+Task+0!%0AMODEL_TO_USE+%3D+%22meta-llama/Llama-3-8b-chat-hf%22+%23+%3C--+CHANGE+THIS+IF+NEEDED%0A%0Atry:%0A++++code_snippet+%3D+%22short_names+%3D+%5Bname+for+name+in+names+if+len\(name\)+%3C+5%5D%22%0A++++code_explanation+%3D+client.chat.completions.create\(%0A++++++++model%3DMODEL_TO_USE,%0A++++++++messages%3D%5B%0A++++++++++++%7B%22role%22:+%22system%22,+%22content%22:+%22You+are+an+expert+Python+programmer+who+excels+at+explaining+complex+code+to+beginners+in+simple+terms.%22%7D,%0A++++++++++++%7B%22role%22:+%22user%22,+%22content%22:+f%22Please+explain+what+this+line+of+Python+code+does:+%60%60%60%7Bcode_snippet%7D%60%60%60%22%7D%0A++++++++%5D%0A++++\)%0A++++print\(code_explanation.choices%5B0%5D.message.content\)%0Aexcept+APIError+as+e:%0A++++print\(f%22An+API+error+occurred:+%7Be%7D%22\))
```python
# ==============================================================================
# Task 3: The Code Explainer
# Goal: Get the AI to explain a piece of Python code in plain English.
# ==============================================================================
print("\n--- Task 4: The Code Explainer ---")
try:
    code_snippet = "short_names = [name for name in names if len(name) < 5]"
    code_explanation = client.chat.completions.create(
        model=MODEL_TO_USE,
        messages=[
            {"role": "system", "content": "You are an expert Python programmer who excels at explaining complex code to beginners in simple terms."},
            {"role": "user", "content": f"Please explain what this line of Python code does: ```{code_snippet}```"}
        ]
    )
    print(code_explanation.choices[0].message.content)
except APIError as e:
    print(f"An API error occurred: {e}")
```
</details>

### **Task 4: The Data Extractor (JSON Mode)**

**Goal:** Turn a messy sentence into a clean, structured JSON object. This is one of the most powerful and practical uses for LLMs.

**Heads-up:** This uses response\_format={"type": "json\_object"}, which not all models support. meta-llama/Llama-3-8b-chat-hf is a good choice if it's on your list.
  
 <details>
     <summary>Spoiler warning</summary>
  
[▶️ **Run this task in Google Colab**](https://colab.research.google.com/notebooks/basic.ipynb?code=import+os%0Afrom+openai+import+OpenAI,+APIError%0A%0A%23+---+Configuration+---%0A%23+This+safely+gets+your+API+key+from+Colab's+Secret+Manager.%0Atry:%0A++++from+google.colab+import+userdata%0A++++NRP_API_KEY+%3D+userdata.get\('NRP_API_KEY'\)%0Aexcept+\(ImportError,+ModuleNotFoundError\):%0A++++NRP_API_KEY+%3D+os.environ.get\(%22NRP_API_KEY%22\)%0A++++if+not+NRP_API_KEY:%0A++++++raise+RuntimeError\(%22API+Key+not+found.+Please+set+the+NRP_API_KEY+environment+variable+or+run+in+Google+Colab+with+secrets.%22\)%0Aexcept+userdata.SecretNotFoundError:%0A++++raise+RuntimeError\(%22API+Key+not+found.+Please+add+it+to+Colab+Secrets+as+'NRP_API_KEY'.%22\)%0A%0A%23+Initialize+the+OpenAI+client+to+point+to+the+NRP+AI+server%0Aclient+%3D+OpenAI\(%0A++++api_key%3DNRP_API_KEY,%0A++++base_url%3D%22https://llm.nrp-nautilus.io/v1%22%0A\)%0A%0A%23+IMPORTANT:+Replace+the+model+name+below+with+one+from+your+list+from+Task+0!%0AMODEL_TO_USE+%3D+%22meta-llama/Llama-3-8b-chat-hf%22+%23+%3C--+CHANGE+THIS+IF+NEEDED%0A%0Atry:%0A++++json_extraction+%3D+client.chat.completions.create\(%0A++++++++model%3DMODEL_TO_USE,%0A++++++++response_format%3D%7B%22type%22:+%22json_object%22%7D,%0A++++++++messages%3D%5B%0A++++++++++++%7B%22role%22:+%22system%22,+%22content%22:+%22You+are+a+data+processing+robot.+Extract+information+from+the+user's+text+and+respond+only+with+a+valid+JSON+object.%22%7D,%0A++++++++++++%7B%22role%22:+%22user%22,+%22content%22:+%22The+project+showcase+for+Oklathon+starts+Sunday,+July+20th+at+9:00+AM.+It's+located+at+StarSpace46.%22%7D%0A++++++++%5D%0A++++\)%0A++++print\(%22Extracted+JSON+data:%22\)%0A++++print\(json_extraction.choices%5B0%5D.message.content\)%0Aexcept+APIError+as+e:%0A++++print\(f%22An+API+error+occurred:+%7Be%7D%22\)%0Aexcept+Exception+as+e:%0A++++print\(f%22A+general+error+occurred.+The+model+might+not+support+JSON+mode.+Error:+%7Be%7D%22\))
```python
# ==============================================================================
# Task 5: The Data Extractor (JSON Mode)
# Goal: Turn a messy sentence into a clean, structured JSON object.
# ==============================================================================
print("\n--- Task 5: The Data Extractor (JSON Mode) ---")
try:
    # Note: For JSON mode to work reliably, you must instruct the model in the prompt!
    # Not all models on the server may support JSON mode. Llama 3 is a good choice.
    json_extraction = client.chat.completions.create(
        model=MODEL_TO_USE,
        response_format={"type": "json_object"}, # This powerful feature asks the AI to only output valid JSON
        messages=[
            {"role": "system", "content": "You are a data processing robot. Extract information from the user's text and respond only with a valid JSON object."},
            {"role": "user", "content": "The project showcase for Oklathon starts Sunday, July 20th at 9:00 AM. It's located at StarSpace46."}
        ]
    )
    print("Extracted JSON data:")
    print(json_extraction.choices[0].message.content)
except APIError as e:
    print(f"An API error occurred: {e}")
except Exception as e:  
```
</details>
