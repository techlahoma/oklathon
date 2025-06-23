# **Oklathon 2025: API Challenge Board**

Welcome to the **Oklathon 2025 "Climb the Challenge Board"** track\! These short, 1-hour challenges are designed to get you started with the NRP AI API.

### **Before you begin:**

1. Get your **NRP\_API\_KEY** from an event organizer.  
2. When you open a Google Colab link, you must add your API key as a **'secret'** to keep it secure. In Colab, click the key icon (🔑) on the left sidebar, click '**Add a new secret**', name it **NRP\_API\_KEY**, paste your key in the '**Value**' field, and ensure the '**Notebook access**' toggle is on.

### **Task 0: See Your Available Models (IMPORTANT: RUN THIS FIRST\!)**

**Goal:** Ask the API which models your key has access to. You'll need one of these model names for all the other tasks.

**Why this is important:** Not all API keys have access to all models. This task lets you see your personalized list so you know what you can use.

 <details>
     <summary>Spoiler warning! (It's OK to peek at this one!)</summary>

[▶️ **Run this task in Google Colab**](https://colab.research.google.com/notebooks/basic.ipynb?code=import+os%0Afrom+openai+import+OpenAI,+APIError%0A%0A%23+---+Configuration+---%0A%23+This+safely+gets+your+API+key+from+Colab's+Secret+Manager.%0Atry:%0A++++from+google.colab+import+userdata%0A++++NRP_API_KEY+%3D+userdata.get\('NRP_API_KEY'\)%0Aexcept+\(ImportError,+ModuleNotFoundError\):%0A++++NRP_API_KEY+%3D+os.environ.get\(%22NRP_API_KEY%22\)%0A++++if+not+NRP_API_KEY:%0A++++++raise+RuntimeError\(%22API+Key+not+found.+Please+set+the+NRP_API_KEY+environment+variable+or+run+in+Google+Colab+with+secrets.%22\)%0Aexcept+userdata.SecretNotFoundError:%0A++++raise+RuntimeError\(%22API+Key+not+found.+Please+add+it+to+Colab+Secrets+as+'NRP_API_KEY'.%22\)%0A%0A%23+Initialize+the+OpenAI+client+to+point+to+the+NRP+AI+server%0Aclient+%3D+OpenAI\(%0A++++api_key%3DNRP_API_KEY,%0A++++base_url%3D%22https://llm.nrp-nautilus.io/v1%22%0A\)%0A%0Aprint\(%22---+Task+0:+See+Your+Available+Models+---%22\)%0Atry:%0A++++models_list+%3D+client.models.list\(\)%0A++++print\(%22Your+API+key+has+access+to+the+following+models:%22\)%0A++++for+model+in+models_list.data:%0A++++++++print\(f%22-+%7Bmodel.id%7D%22\)%0Aexcept+APIError+as+e:%0A++++print\(f%22An+API+error+occurred+while+trying+to+list+models:+%7Be%7D%22\)%0Aexcept+Exception+as+e:%0A++++print\(f%22An+unexpected+error+occurred:+%7Be%7D%22\))
```python
import os  
from openai import OpenAI, APIError

\# \--- Configuration \---  
\# This safely gets your API key from Colab's Secret Manager.  
try:  
    from google.colab import userdata  
    NRP\_API\_KEY \= userdata.get('NRP\_API\_KEY')  
except (ImportError, ModuleNotFoundError):  
    NRP\_API\_KEY \= os.environ.get("NRP\_API\_KEY")  
    if not NRP\_API\_KEY:  
      raise RuntimeError("API Key not found. Please set the NRP\_API\_KEY environment variable or run in Google Colab with secrets.")  
except userdata.SecretNotFoundError:  
    raise RuntimeError("API Key not found. Please add it to Colab Secrets as 'NRP\_API\_KEY'.")

\# Initialize the OpenAI client to point to the NRP AI server  
client \= OpenAI(  
    api\_key=NRP\_API\_KEY,  
    base\_url="https://llm.nrp-nautilus.io/v1"  
)

print("--- Task 0: See Your Available Models \---")  
try:  
    models\_list \= client.models.list()  
    print("Your API key has access to the following models:")  
    for model in models\_list.data:  
        print(f"- {model.id}")  
except APIError as e:  
    print(f"An API error occurred while trying to list models: {e}")  
except Exception as e:  
    print(f"An unexpected error occurred: {e}")
```

</details>

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

import os  
from openai import OpenAI, APIError

\# \--- Configuration \---  
try:  
    from google.colab import userdata  
    NRP\_API\_KEY \= userdata.get('NRP\_API\_KEY')  
except (ImportError, ModuleNotFoundError):  
    NRP\_API\_KEY \= os.environ.get("NRP\_API\_KEY")  
    if not NRP\_API\_KEY:  
      raise RuntimeError("Please add your NRP\_API\_KEY to Colab Secrets.")  
except userdata.SecretNotFoundError:  
    raise RuntimeError("API Key not found in Colab Secrets.")

\# Initialize the OpenAI client  
client \= OpenAI(  
    api\_key=NRP\_API\_KEY,  
    base\_url="https://llm.nrp-nautilus.io/v1"  
)

\# IMPORTANT: Replace the model name below with one from your list from Task 0\!  
MODEL\_TO\_USE \= "meta-llama/Llama-3-8b-chat-hf" \# \<-- CHANGE THIS IF NEEDED

print(f"Attempting to use model: {MODEL\_TO\_USE}")  
try:  
    completion \= client.chat.completions.create(  
        model=MODEL\_TO\_USE,  
        messages=\[  
            {"role": "system", "content": "You are a helpful assistant."},  
            {"role": "user", "content": "Tell me a fun fact about the history of Oklahoma City."}  
        \]  
    )  
    print(completion.choices\[0\].message.content)  
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
import os  
from openai import OpenAI, APIError

\# \--- Configuration \---  
try:  
    from google.colab import userdata  
    NRP\_API\_KEY \= userdata.get('NRP\_API\_KEY')  
except (ImportError, ModuleNotFoundError):  
    NRP\_API\_KEY \= os.environ.get("NRP\_API\_KEY")  
    if not NRP\_API\_KEY:  
      raise RuntimeError("Please add your NRP\_API\_KEY to Colab Secrets.")  
except userdata.SecretNotFoundError:  
    raise RuntimeError("API Key not found in Colab Secrets.")

\# Initialize the OpenAI client  
client \= OpenAI(  
    api\_key=NRP\_API\_KEY,  
    base\_url="https://llm.nrp-nautilus.io/v1"  
)

\# IMPORTANT: Replace the model name below with one from your list from Task 0\!  
MODEL\_TO\_USE \= "meta-llama/Llama-3-8b-chat-hf" \# \<-- CHANGE THIS IF NEEDED

try:  
    \# Pirate Persona  
    pirate\_completion \= client.chat.completions.create(  
        model=MODEL\_TO\_USE,  
        messages=\[  
            {"role": "system", "content": "You are a sarcastic, world-weary pirate. You answer all questions as such."},  
            {"role": "user", "content": "What should I bring to a hackathon?"}  
        \]  
    )  
    print("PIRATE SAYS:")  
    print(pirate\_completion.choices\[0\].message.content)

    \# Robot Persona  
    robot\_completion \= client.chat.completions.create(  
        model=MODEL\_TO\_USE,  
        messages=\[  
            {"role": "system", "content": "You are BEEP-BOOP, a cheerful robot assistant. You are enthusiastic and helpful."},  
            {"role": "user", "content": "What should I bring to a hackathon?"}  
        \]  
    )  
    print("\\nROBOT SAYS:")  
    print(robot\_completion.choices\[0\].message.content)  
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
import os  
from openai import OpenAI, APIError

\# \--- Configuration \---  
try:  
    from google.colab import userdata  
    NRP\_API\_KEY \= userdata.get('NRP\_API\_KEY')  
except (ImportError, ModuleNotFoundError):  
    NRP\_API\_KEY \= os.environ.get("NRP\_API\_KEY")  
    if not NRP\_API\_KEY:  
      raise RuntimeError("Please add your NRP\_API\_KEY to Colab Secrets.")  
except userdata.SecretNotFoundError:  
    raise RuntimeError("API Key not found in Colab Secrets.")

\# Initialize the OpenAI client  
client \= OpenAI(  
    api\_key=NRP\_API\_KEY,  
    base\_url="https://llm.nrp-nautilus.io/v1"  
)

\# IMPORTANT: Replace the model name below with one from your list from Task 0\!  
MODEL\_TO\_USE \= "meta-llama/Llama-3-8b-chat-hf" \# \<-- CHANGE THIS IF NEEDED

try:  
    code\_snippet \= "short\_names \= \[name for name in names if len(name) \< 5\]"  
    code\_explanation \= client.chat.completions.create(  
        model=MODEL\_TO\_USE,  
        messages=\[  
            {"role": "system", "content": "You are an expert Python programmer who excels at explaining complex code to beginners in simple terms."},  
            {"role": "user", "content": f"Please explain what this line of Python code does: \`\`\`{code\_snippet}\`\`\`"}  
        \]  
    )  
    print(code\_explanation.choices\[0\].message.content)  
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
import os  
from openai import OpenAI, APIError

\# \--- Configuration \---  
try:  
    from google.colab import userdata  
    NRP\_API\_KEY \= userdata.get('NRP\_API\_KEY')  
except (ImportError, ModuleNotFoundError):  
    NRP\_API\_KEY \= os.environ.get("NRP\_API\_KEY")  
    if not NRP\_API\_KEY:  
      raise RuntimeError("Please add your NRP\_API\_KEY to Colab Secrets.")  
except userdata.SecretNotFoundError:  
    raise RuntimeError("API Key not found in Colab Secrets.")

\# Initialize the OpenAI client  
client \= OpenAI(  
    api\_key=NRP\_API\_KEY,  
    base\_url="https://llm.nrp-nautilus.io/v1"  
)

\# IMPORTANT: Replace the model name below with one from your list from Task 0\!  
MODEL\_TO\_USE \= "meta-llama/Llama-3-8b-chat-hf" \# \<-- CHANGE THIS IF NEEDED

try:  
    json\_extraction \= client.chat.completions.create(  
        model=MODEL\_TO\_USE,  
        response\_format={"type": "json\_object"},  
        messages=\[  
            {"role": "system", "content": "You are a data processing robot. Extract information from the user's text and respond only with a valid JSON object."},  
            {"role": "user", "content": "The project showcase for Oklathon starts Sunday, July 20th at 9:00 AM. It's located at StarSpace46."}  
        \]  
    )  
    print("Extracted JSON data:")  
    print(json\_extraction.choices\[0\].message.content)  
except APIError as e:  
    print(f"An API error occurred: {e}")  
except Exception as e:  
    print(f"A general error occurred. The model might not support JSON mode. Error: {e}")  
```
</details>
