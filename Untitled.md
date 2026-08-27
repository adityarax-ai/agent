**MODULE:1**

**Simple AI Agent using Agno framework to generate nvidia report**


```python
%pip install -U agno groq
```


```python
import os
os.environ["GROQ_API_KEY"]="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"
```


```python
from agno.agent import Agent
from agno.models.groq import Groq
```


```python
agent=Agent(
    model=Groq(
        id="openai/gpt-oss-20b"
    )
)
```


```python
report=agent.run(

    """
    wrtie a simple report about NVIDIA

    Includes:
    1) Company Overview
    2) Major Product
    3) AI Technology
    4) Future Growth

    use simple and easy to understand Language
    """, markdown=True
)
print(report.content)
```

    **Simple Report: NVIDIA**
    
    ---
    
    ### 1. Company Overview  
    NVIDIA is a U.S. technology company that designs computer chips (graphics processing units – GPUs).  
    - **Founded:** 1993, by Jensen Huang, Chris Malachowsky, and Curtis Priem.  
    - **Headquarters:** Santa Clara, California.  
    - **What it does:** Makes GPUs for gaming, professional design, data centers, and artificial‑intelligence (AI) work.
    
    ### 2. Major Product  
    **GeForce GPUs**  
    - These are the most popular graphics cards for gamers.  
    - They deliver smooth, realistic graphics and high frame rates.  
    - They are also used by artists, engineers, and researchers for complex visual work.
    
    ### 3. AI Technology  
    - NVIDIA’s GPUs accelerate AI training and inference.  
    - **CUDA** programming platform lets developers write software that runs quickly on NVIDIA chips.  
    - **TensorRT** and **DLSS** are tools that speed up deep‑learning models and enhance image quality.  
    - AI uses NVIDIA GPUs in fields like self‑driving cars, medical imaging, and natural‑language processing.
    
    ### 4. Future Growth  
    - **Data Centers:** More companies need AI servers; NVIDIA’s GPUs are the “brain” of many data‑center systems.  
    - **Automotive:** Self‑driving cars rely on NVIDIA’s Drive platform.  
    - **AI Services:** NVIDIA is expanding cloud‑based AI services and software tools.  
    - **New Chiplines:** Upcoming GPUs (e.g., the RTX 40 series) will bring faster performance and lower power use.  
    - **Partnerships:** Collaborations with major cloud providers and AI startups will boost demand.
    
    Overall, NVIDIA continues to grow by supplying the powerful chips that make high‑performance gaming, professional work, and advanced AI possible.
    

**PLANNING  DESIGN PATEERN**


```python
plan=agent.run(

    """
    Create a simple 4-step plan for writing a report about NVIDIA.

    The report should cover:
    1. Company Overview
    2. Major Products
    3. AI Technology
    4. Future Growth

    Return only the plan.
    """
)

print(plan.content)
```

    **4‑Step Plan for Writing the NVIDIA Report**
    
    1. **Company Overview**  
       • Research NVIDIA’s history, mission, and core values.  
       • Summarize key financial metrics (revenue, profit, market cap).  
       • Identify the leadership team and corporate structure.
    
    2. **Major Products**  
       • List flagship hardware lines: GeForce GPUs, Quadro/RTX professional GPUs, Tesla/Datacenter GPUs, Tegra mobile processors.  
       • Highlight flagship software platforms (CUDA, RTX, DLSS).  
       • Note any recent product launches or updates.
    
    3. **AI Technology**  
       • Explain NVIDIA’s role in AI hardware (GPU architecture, tensor cores).  
       • Outline software ecosystems supporting AI (CUDA, cuDNN, TensorRT).  
       • Discuss research collaborations and AI initiatives (NVIDIA AI Enterprise, research labs).
    
    4. **Future Growth**  
       • Identify emerging markets: automotive, data center, edge computing, autonomous systems.  
       • Evaluate strategic partnerships and acquisitions (e.g., Mellanox, Arm prospects).  
       • Analyze potential risks and opportunities shaping NVIDIA’s long‑term trajectory.
    


```python

```


```python

```

**ReAct Design Pattern**


```python
# STEP 2: Generate Output Based on Plan

output = agent.run(
    f"""
    Write a simple report about NVIDIA.

    Follow this plan:

    {plan.content}

    Use simple and easy-to-understand language.
    Make sure the final report follows all the steps in the plan.
    """
)

print("\nFINAL OUTPUT:")
print(output.content)
```


```python

```


```python

```

**MODULE:2**

**HOW TO CONVERT THE CHAIN TO WEB API USING LANGSERVE FRAMEWORK**


```python
pip install sse_starlette
```


```python
%pip install -U langchain langchain-groq langserve uvicorn requests fastapi
```


```python

```


```python

```


```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_groq import ChatGroq
```


```python
prompt=ChatPromptTemplate.from_template(
    "Give me a simple explanation about Artificial Intelligence"
)
```


```python
model=ChatGroq(
    model="openai/gpt-oss-20b",
    api_key=os.environ["GROQ_API_KEY"]
)
```


```python
chain=prompt | model
```


```python
from fastapi import FastAPI
from langserve import add_routes

app = FastAPI(
    title="My AI Chain API",
    version="1.0"
)

add_routes(
    app,
    chain,
    path="/chain"
)
```


```python
import uvicorn

config = uvicorn.Config(
    app,
    host="127.0.0.1",
    port=5102
)

server = uvicorn.Server(config)

await server.serve()
```

    INFO:     Started server process [68980]
    INFO:     Waiting for application startup.
    INFO:     Application startup complete.
    INFO:     Uvicorn running on http://127.0.0.1:5102 (Press CTRL+C to quit)
    

    
         __          ___      .__   __.   _______      _______. _______ .______     ____    ____  _______
        |  |        /   \     |  \ |  |  /  _____|    /       ||   ____||   _  \    \   \  /   / |   ____|
        |  |       /  ^  \    |   \|  | |  |  __     |   (----`|  |__   |  |_)  |    \   \/   /  |  |__
        |  |      /  /_\  \   |  . `  | |  | |_ |     \   \    |   __|  |      /      \      /   |   __|
        |  `----./  _____  \  |  |\   | |  |__| | .----)   |   |  |____ |  |\  \----.  \    /    |  |____
        |_______/__/     \__\ |__| \__|  \______| |_______/    |_______|| _| `._____|   \__/     |_______|
        
    [1;32;40mLANGSERVE:[0m Playground for chain "/chain/" is live at:
    [1;32;40mLANGSERVE:[0m  │
    [1;32;40mLANGSERVE:[0m  └──> /chain/playground/
    [1;32;40mLANGSERVE:[0m
    [1;32;40mLANGSERVE:[0m See all available routes at /docs/
    

    INFO:     Shutting down
    INFO:     Waiting for application shutdown.
    INFO:     Application shutdown complete.
    INFO:     Finished server process [68980]
    


```python

```


```python

```


```python
import requests

response = requests.post(
    "http://127.0.0.1:5102/chain/invoke",
    json={
        "input": {
            "topic": "Artificial Intelligence",
        }
    }
)

print("Status:", response.status_code)
print(response.json())
```


```python

```


```python

```


```python

```


```python

```


```python

```

**Build a Self-correcting Coding Assistant with LangChain**


```python

```


```python

```


```python
from langchain_core.prompts import ChatPromptTemplate
from langchain_groq import ChatGroq
```


```python
model=ChatGroq(
    model="openai/gpt-oss-20b",
    api_key=os.environ["groq_api_key"]
)
```


```python
prompt=ChatPromptTemplate.from_template(
    """
    write a python code for problem {problem}

    return only python code.
    """
)
```


```python
chain=prompt | model
```


```python
prob="""
create a python program that add two numbers.
"""

resp=chain.invoke(
    {
        "problem":prob
    }
)

code=resp.content
print(code)
```

    ```python
    # Read two numbers from standard input, separated by whitespace
    a, b = map(float, input().split())
    
    # Compute the sum
    result = a + b
    
    # Print the result
    print(result)
    ```
    


```python
corr_prompt=ChatPromptTemplate.from_template(
    """
    you are self correcting coding assistant.

    The use asked for: {problem}

    The generated code was: {code}

    The code produced this error: {error}

    Fix the code.

    Return only corrected python code
    
   """
)
```


```python
corr_chain=corr_prompt | model
```


```python
def self_correct_code(prob,code):
    try:
        exec(code)
        print("Code run")
        return code

    except Exception as e:
        print("Error found")
        print(e)

        resp=corr_chain.invoke(
            {
                "problem":prob,
                "code":code,
                "error":str(e)
            }
        )
        corr_code=resp.content
        print(corr_code)
        return corr_code
```


```python
problem = """
Create a Python program that adds two numbers.
"""

bad_code = """
a = 10
b = "20"
print(a + b)
"""
```


```python
corrected_code = self_correct_code(
    problem,
    bad_code
)
```

    Error found
    unsupported operand type(s) for +: 'int' and 'str'
    a = 10
    b = 20
    print(a + b)
    


```python

```


```python

```

**Financial Bot**


```python
%pip install -U langgraph langchain-groq
```


```python
from langgraph.graph import StateGraph, START, END
from langchain_groq import ChatGroq
```


```python
model = ChatGroq(
    model="openai/gpt-oss-20b"
)
```


```python
from typing import TypedDict
class FinanceState(TypedDict):
    company: str
    answer: str
```


```python
def finance_bot(state: FinanceState):

    company = state["company"]

    response = model.invoke(
        f"""
        Give me a simple financial overview of {company}.

        Include:
        1. Company overview
        2. Main products
        3. Financial situation
        4. Future growth

        Use simple language.
        """
    )

    return {
        "answer": response.content
    }
```


```python
graph = StateGraph(FinanceState)

graph.add_node("finance_bot", finance_bot)

graph.add_edge(START, "finance_bot")
graph.add_edge("finance_bot", END)

app = graph.compile()
```


```python
result = app.invoke({
    "company": "NVIDIA",
    "answer": ""
})

print(result["answer"])
```

    **NVIDIA – Simple Financial Overview**
    
    | Item | What it means | Quick facts |
    |------|---------------|-------------|
    | **Company overview** | NVIDIA is a U.S. tech company that designs chips (semiconductors) and software for computers, gaming, data centers, and AI. | Founded 1993, HQ in Santa Clara, California. |
    | **Main products** | 1. **GPU chips** – used in gaming PCs, AI servers, and data‑center supercomputers.<br>2. **Software & AI platforms** – like CUDA, NVIDIA AI, and Omniverse.<br>3. **Automotive & edge solutions** – for self‑driving cars and industrial IoT. | GPUs are the biggest money‑maker. |
    | **Financial situation (FY 2024)** | • **Revenue:** ~$28 billion (up ~30 % YoY).<br>• **Net income:** ~$11 billion (profit margin ~40 %).<br>• **Cash & investments:** ~$20 billion (good liquidity).<br>• **Debt:** ~$10 billion (manageable).<br>• **Cash flow:** ~$15 billion operating cash flow. | Strong growth, high profit margins, and a healthy balance sheet. |
    | **Future growth** | • **AI & data‑center demand** – GPUs are the core hardware for AI training & inference, which is growing fast.<br>• **Gaming & esports** – still a big revenue stream; new console/PC releases keep sales high.<br>• **Automotive & edge** – expanding self‑driving tech and smart‑city solutions.<br>• **New product launches** – next‑gen GPUs (Ada Lovelace, Hopper) and new AI software tools. | NVIDIA is well‑positioned to keep growing as AI and cloud computing expand. |
    
    **Bottom line:** NVIDIA is a high‑growth chip company with strong profits and a solid cash position. Its GPUs power gaming, AI, and data‑center workloads, which are all expected to keep expanding in the coming years.
    


```python

```


```python

```

**MODULE :3**

**Create an AI-Powered Sales Report Analyzer with LlamaIndex**


```python
%pip install -U llama-index llama-index-llms-groq
```


```python
from llama_index.core import Document
from llama_index.llms.groq import Groq
```

    C:\Users\Aditya Raj.000\agenticai310\lib\site-packages\tqdm\auto.py:21: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
      from .autonotebook import tqdm as notebook_tqdm
    None of PyTorch, TensorFlow >= 2.0, or Flax have been found. Models won't be available and only tokenizers, configuration and file/data utilities can be used.
    


```python
llm = Groq(
    model="openai/gpt-oss-20b"
)
```


```python
sales_report = """
Sales Report

Product: Laptop
Units Sold: 120
Revenue: 120000

Product: Smartphone
Units Sold: 200
Revenue: 100000

Product: Tablet
Units Sold: 80
Revenue: 40000

Product: Monitor
Units Sold: 150
Revenue: 45000
"""
```


```python
document = Document(
    text=sales_report
)
```


```python
def analyze_sales():

    prompt = f"""
    You are a sales report analyzer.

    Analyze the following sales report:

    {document.text}

    Give a simple report containing:

    1. Best selling product
    2. Highest revenue product
    3. Total units sold
    4. Total revenue
    5. Simple business summary

    Use simple and easy-to-understand language.
    """

    response = llm.complete(prompt)

    return response.text
```


```python
result = analyze_sales()

print("SALES REPORT ANALYSIS")
print("=====================")
print(result)
```

    SALES REPORT ANALYSIS
    =====================
    **Sales Summary**
    
    1. **Best‑selling product** – *Smartphone* (200 units sold)  
    2. **Highest revenue product** – *Laptop* (₹120,000 revenue)  
    3. **Total units sold** – 550 units  
    4. **Total revenue** – ₹305,000  
    5. **Business snapshot**  
       - The smartphone is the most popular item, but the laptop brings in the most money.  
       - Overall sales are healthy, with a good mix of high‑volume and high‑margin products.  
       - Consider promoting the laptop more aggressively or adding accessories to boost revenue further.
    


```python

```


```python

```

**Design a Data Analysis Agent with Phidata**


```python

```


```python

```


```python

```


```python
%pip install -U phidata
```


```python
import os
os.environ["GROQ_API_KEY"]="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"
```


```python
import os
import pandas as pd

from phi.agent import Agent
from phi.model.groq import Groq
```


```python
data = {
    "Product": ["Laptop", "Phone", "Tablet", "Monitor"],
    "Sales": [120, 200, 80, 150],
    "Revenue": [120000, 100000, 40000, 45000]
}

df = pd.DataFrame(data)

df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Product</th>
      <th>Sales</th>
      <th>Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Laptop</td>
      <td>120</td>
      <td>120000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Phone</td>
      <td>200</td>
      <td>100000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Tablet</td>
      <td>80</td>
      <td>40000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Monitor</td>
      <td>150</td>
      <td>45000</td>
    </tr>
  </tbody>
</table>
</div>




```python
agent = Agent(
    model=Groq(
        id="openai/gpt-oss-20b"
    ),
    instructions=[
        "Analyze the given data.",
        "Find the best-selling product.",
        "Find the product with the highest revenue.",
        "Calculate total sales and total revenue.",
        "Give a simple summary."
    ],
    markdown=True
)

print("Data Analysis Agent created!")
```

    Data Analysis Agent created!
    


```python
question = f"""
Analyze this sales data:

{df.to_string(index=False)}

Give me:
1. Best-selling product
2. Highest revenue product
3. Total sales
4. Total revenue
5. Simple business summary
"""

resp=agent.run(question)
print(resp.content)
```

    **Sales Data Analysis**
    
    | Product | Sales | Revenue |
    |---------|-------|---------|
    | Laptop  | 120   | 120 000 |
    | Phone   | 200   | 100 000 |
    | Tablet  | 80    | 40 000  |
    | Monitor | 150   | 45 000  |
    
    | Metric | Value |
    |--------|-------|
    | **Best‑selling product** | **Phone** (200 units) |
    | **Highest revenue product** | **Laptop** (120 000) |
    | **Total sales** | **550 units** |
    | **Total revenue** | **305 000** |
    
    ### Simple Business Summary
    - **Phone** is the most popular item, driving the highest volume of sales, but its revenue per unit is lower than the laptop.
    - **Laptop** generates the greatest revenue, indicating a higher unit margin or a premium price point.
    - **Monitor** and **Tablet** contribute modestly; the monitor has a higher sales volume than the tablet but lower revenue, suggesting it may be priced lower or sold in larger quantities.
    - **Recommendation:** Focus on optimizing the laptop’s profit margin while promoting the phone’s high demand—perhaps bundle deals or targeted marketing—to balance volume and revenue. Consider re‑evaluating the pricing or promotion strategy for the tablet to boost its performance.
    


```python

```


```python

```


```python

```


```python

```


```python

```

**MODULE-4**


```python
!pip install -q langgraph langchain langchain-groq
```


```python
import os
from typing import TypedDict

from langgraph.graph import StateGraph, START, END
from langchain_groq import ChatGroq
```


```python
llm = ChatGroq(
    model="openai/gpt-oss-20b"
)
```


```python
class CustomerState(TypedDict):
    question: str
    agent: str
    answer: str
```


```python
def router(state: CustomerState):

    question = state["question"].lower()

    if any(word in question for word in ["payment", "bill", "billing", "refund", "price"]):
        return {"agent": "billing"}

    return {"agent": "support"}
```


```python
def support_agent(state: CustomerState):

    response = llm.invoke(
        f"""
        You are a customer support agent.

        Customer question:
        {state["question"]}

        Give a simple and helpful answer.
        """
    )

    return {
        "answer": response.content
    }
```


```python
def billing_agent(state: CustomerState):

    response = llm.invoke(
        f"""
        You are a billing support agent.

        Customer question:
        {state["question"]}

        Give a simple and helpful answer about billing.
        """
    )

    return {
        "answer": response.content
    }
```


```python
graph = StateGraph(CustomerState)

graph.add_node("router", router)
graph.add_node("support", support_agent)
graph.add_node("billing", billing_agent)

graph.add_edge(START, "router")

graph.add_conditional_edges(
    "router",
    lambda state: state["agent"],
    {
        "support": "support",
        "billing": "billing"
    }
)

graph.add_edge("support", END)
graph.add_edge("billing", END)

customer_bot = graph.compile()

print("Customer Support Bot created successfully!")
```

    Customer Support Bot created successfully!
    


```python
result = customer_bot.invoke({
    "question": "I want a refund for my payment.",
    "agent": "",
    "answer": ""
})

print("Agent:", result["agent"])
print("Answer:", result["answer"])
```

    Agent: billing
    Answer: Sure, I can help with that. To process a refund, I’ll need a few details:
    
    1. The order or transaction number  
    2. The date of the payment  
    3. The reason for the refund (e.g., product not as described, duplicate charge, etc.)
    
    Once I have that information, I’ll submit a refund request on your behalf and let you know the expected timeline. If you’d like, you can also start the process through our support portal by clicking **“Request a Refund”** in your account’s order history.
    


```python
result = customer_bot.invoke({
    "question": "How can I reset my password?",
    "agent": "",
    "answer": ""
})

print("Agent:", result["agent"])
print("Answer:", result["answer"])
```

    Agent: support
    Answer: Sure! Here’s what to do:
    
    1. **Go to the login page** of the website or app.  
    2. Click the **“Forgot password?”** link.  
    3. Enter the email address (or username) you use to log in.  
    4. Check your inbox for a password‑reset email.  
    5. Click the link in that email and follow the prompts to set a new password.  
    6. If you don’t see the email, check your spam/junk folder or try again in a few minutes.  
    
    If you still can’t reset it, let us know and we’ll help you out!
    


```python

```


```python

```


```python

```


```python

```

**Design a Stock Analysis Agent with CrewAI**


```python

```


```python

```


```python

```


```python

```

**Develop an AI Research Agent with Autogen**


```python
# %pip install -U "autogen-agentchat" "autogen-ext[openai]"
```


```python
import os

from autogen_agentchat.agents import AssistantAgent
from autogen_ext.models.openai import OpenAIChatCompletionClient
```


```python
import os
os.environ["GROQ_API_KEY"]="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"
```


```python
model_client = OpenAIChatCompletionClient(
    model="openai/gpt-oss-20b",
    api_key=os.environ["GROQ_API_KEY"],
    base_url="https://api.groq.com/openai/v1",

    model_info={
        "vision": False,
        "function_calling": True,
        "json_output": True,
        "family": "unknown",
        "structured_output": True
    }
)

print("AI model created successfully!")
```

    AI model created successfully!
    


```python
research_agent = AssistantAgent(
    name="ResearchAgent",
    model_client=model_client,
    system_message="""
    You are an AI research assistant.

    Research the given topic and provide:

    1. Introduction
    2. Important facts
    3. Advantages
    4. Disadvantages
    5. Future scope

    Use simple and easy-to-understand language.
    """
)

print("Research Agent created successfully!")
```

    Research Agent created successfully!
    


```python
result = await research_agent.run(
    task="Research Artificial Intelligence"
)

print(result.messages[-1].content)
```

    ## 1. Introduction  
    **Artificial Intelligence (AI)** is the field of computer science that builds machines and software that can do things that normally need human intelligence.  
    Typical “intelligent” tasks include:  
    
    * Understanding spoken or written language  
    * Seeing and recognizing objects in pictures or video  
    * Playing games better than a human  
    * Recommending movies, products, or routes  
    
    AI is not a single technology; it is a set of methods and tools that let computers learn from data, reason, plan, and adapt.
    
    ---
    
    ## 2. Important Facts  
    
    | Fact | What it means |
    |------|----------------|
    | **AI is everywhere** | From the voice assistant on your phone to fraud‑detection systems in banks. |
    | **Deep learning = boom** | A type of machine learning that uses “neural networks” with many layers. It powers image recognition, translation, and voice synthesis. |
    | **Data is the fuel** | AI systems learn patterns from large amounts of data. Without good data they can’t perform well. |
    | **Speed of progress** | The number of AI research papers published each year has been growing faster than any other field in the last decade. |
    | **Economic impact** | Global AI market expected to reach **$1 trillion** by 2030, creating millions of jobs and new business models. |
    | **Ethics & policy** | Governments worldwide are drafting rules to ensure AI is safe, fair, and respects privacy. |
    | **AI & jobs** | AI can automate routine tasks, freeing humans for creative and complex work, but it also changes the skill requirements for many occupations. |
    | **OpenAI and other labs** | Organizations such as OpenAI, Google DeepMind, and Microsoft Research publish cutting‑edge models (e.g., GPT‑4, AlphaFold) that set new performance standards. |
    
    ---
    
    ## 3. Advantages  
    
    | Advantage | Why it matters |
    |-----------|----------------|
    | **Increased efficiency** | AI can process millions of data points in seconds, speeding up decision‑making in finance, logistics, and healthcare. |
    | **24/7 operation** | Unlike humans, AI systems can work continuously without fatigue, ideal for customer support chatbots or monitoring equipment. |
    | **Personalization** | Recommender systems (e.g., Netflix, Amazon) tailor suggestions to individual tastes, improving user experience and sales. |
    | **Safety** | Autonomous systems (self‑driving cars, surgical robots) can reduce human error and improve safety in dangerous environments. |
    | **Insight from data** | AI can uncover hidden patterns in complex data, helping scientists discover new drugs or engineers design more efficient structures. |
    | **Cost savings** | Automating repetitive tasks lowers labor costs and reduces human error, leading to financial savings for businesses. |
    
    ---
    
    ## 4. Disadvantages  
    
    | Disadvantage | What it could cause |
    |--------------|--------------------|
    | **Job displacement** | Automation may replace some routine jobs, requiring workers to reskill. |
    | **Bias & fairness** | If training data is biased, AI can reinforce stereotypes or make unfair decisions (e.g., hiring, lending). |
    | **Lack of transparency** | Complex models (“black boxes”) are hard to interpret, making it difficult to audit decisions or explain failures. |
    | **Security risks** | AI can be misused for deepfakes, automated cyber‑attacks, or weaponization. |
    | **High resource cost** | Training large models consumes huge amounts of electricity and computing power, raising environmental and economic concerns. |
    | **Ethical dilemmas** | Decisions made by AI (e.g., in healthcare triage) raise questions about responsibility and accountability. |
    | **Overreliance** | Blindly trusting AI can lead to mistakes if the system fails or behaves unexpectedly. |
    
    ---
    
    ## 5. Future Scope  
    
    | Area | What might happen |
    |------|-------------------|
    | **General AI** | Current AI excels at narrow tasks. Researchers are working toward more flexible, “general” systems that can learn across many domains, like humans. |
    | **AI in Medicine** | Predictive diagnostics, personalized treatment plans, and robotic surgery could become routine, improving outcomes and lowering costs. |
    | **Edge AI** | Running AI directly on smartphones, drones, or sensors reduces latency and saves bandwidth, enabling real‑time decision making. |
    | **Human‑AI collaboration** | Tools that augment human creativity—design assistants, code generators, or brainstorming partners—will become common. |
    | **Ethical & governance frameworks** | International standards and regulations will evolve to ensure AI is safe, transparent, and fair. |
    | **Energy‑efficient models** | Techniques like model pruning, quantization, and neuromorphic chips aim to reduce the carbon footprint of AI. |
    | **AI for climate** | Modeling weather, optimizing energy grids, and monitoring ecosystems can help combat climate change. |
    | **AI in education** | Adaptive learning platforms will personalize instruction, giving every student tailored feedback. |
    
    ---
    
    ### Quick Takeaway  
    Artificial Intelligence is a rapidly growing field that lets computers mimic human intelligence in many ways. It brings huge benefits—speed, efficiency, and new insights—while also posing challenges like bias, job shifts, and ethical questions. The next decade will see AI expand into new industries, become more responsible and accessible, and potentially transform the way we live and work.
    


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```

**12. LangSmith dashboard**


```python
%pip install -q langchain langchain-groq langsmith
```

    Note: you may need to restart the kernel to use updated packages.
    


```python
import os

from langchain_groq import ChatGroq
```


```python
os.environ["GROQ_API_KEY"] = "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"

os.environ["LANGSMITH_API_KEY"] = "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"

os.environ["LANGSMITH_TRACING"] = "true"

os.environ["LANGSMITH_PROJECT"] = "AI Observability Demo"

os.environ["LANGSMITH_ENDPOINT"] = "https://api.smith.langchain.com"

print("API keys configured!")
```

    API keys configured!
    


```python
from langchain_groq import ChatGroq

llm = ChatGroq(
    model="openai/gpt-oss-20b"
)

response = llm.invoke(
    "Explain Artificial Intelligence in simple language."
)

print(response.content)
```

    **Artificial Intelligence (AI) – in plain words**
    
    Think of a computer that can *learn* from experience, just like you do.
    
    * **Learning** – Instead of being told exactly what to do, the computer looks at lots of data (pictures, words, numbers) and figures out patterns on its own.  
      *Example:* A program looks at thousands of photos of cats and learns what a cat looks like.
    
    * **Thinking** – Once it has learned, it can use that knowledge to solve problems or answer questions.  
      *Example:* When you ask a smart speaker “What’s the weather?” it uses what it learned to give you a correct answer.
    
    * **Doing tasks** – AI can perform many jobs that usually need a human mind: recognizing speech, translating languages, recommending movies, driving cars, diagnosing diseases, etc.
    
    **Key points**
    
    1. **It’s software, not a person** – AI doesn’t have feelings or consciousness; it just follows patterns it discovered.
    2. **It improves over time** – The more data it sees, the better it gets at its job.
    3. **It’s everywhere** – From the “Play next” button on a video site to voice assistants on your phone.
    
    So, AI is basically a smart computer program that learns from examples and then uses that learning to help us do things faster or better.
    


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```


```python

```
