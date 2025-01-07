# 🔢 LangChain Code Execution Agent: Real-Time Python Code Execution 🚀

This project demonstrates the use of **LangChain** with **Ollama's Llama 3.2 model** for creating an interactive, real-time Python code execution agent. Users can execute Python code, perform calculations, or solve programming tasks in real time using custom-defined tools integrated into LangChain.

---

## 🌟 Key Features
- **Real-Time Python Code Execution**: Execute Python code dynamically with custom tools.
- **Llama 3.2 Model Integration**: Use the power of Ollama's Llama model for reasoning and generating responses.
- **Interactive User Queries**: Allow users to define tasks interactively, such as calculating factorials or checking prime numbers.
- **Zero-Shot Description Agent**: LangChain's zero-shot agent type for flexible, descriptive queries.

---

## 🛠️ Installation

### Install Required Libraries
Run the following command to set up your environment:
```bash
pip install langchain langchain_ollama colab-xterm
Additional Setup
Load Colab's interactive terminal for advanced testing:
bash
Copy code
%load_ext colabxterm
Log in to Hugging Face:
python
Copy code
from huggingface_hub import login
login("your_huggingface_token")
🚀 Workflow
1️⃣ Define Python Code Execution Tool
Create a Python function to execute code and return the results:

python
Copy code
from langchain.agents import Tool

def execute_python_code(code: str) -> str:
    try:
        exec_globals = {}
        exec(code, exec_globals)
        return str(exec_globals.get("_", "Code executed successfully."))
    except Exception as e:
        return f"Error: {e}"

python_tool = Tool(
    name="PythonTool",
    func=execute_python_code,
    description="Executes Python code and returns the output."
)
2️⃣ Integrate with Llama 3.2 Model
Initialize Ollama's Llama model and integrate the tool:

python
Copy code
from langchain_community.llms import Ollama
from langchain.agents import initialize_agent, AgentType

# Initialize the LLM
llm = Ollama(model="llama3.2")

# Add the custom Python tool
tools = [python_tool]

# Initialize the LangChain agent
agent = initialize_agent(
    tools,
    llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)
3️⃣ Example Task: Factorial Calculation
Use the agent to solve a factorial calculation task:

python
Copy code
response = agent.run("Calculate the factorial of 5 using Python.")
print(response)
📚 Example Interaction
Factorial Calculation
Input:

text
Copy code
Calculate the factorial of 5 using Python.
Output:

python
Copy code
def factorial(n):
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n-1)

factorial(5)  # Output: 120
Prime Number Checking
Add a custom tool for checking prime numbers:

python
Copy code
def is_prime(n: int) -> str:
    if n <= 1:
        return f"{n} is not a prime number."
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return f"{n} is not a prime number."
    return f"{n} is a prime number."

prime_tool = Tool(
    name="PrimeChecker",
    func=is_prime,
    description="Checks if a given number is prime."
)

tools.append(prime_tool)
Test the prime checker:

python
Copy code
response = agent.run("Check if the number 7 is prime.")
print(response)
Output:

text
Copy code
The number 7 is a prime number.
🎯 Customization Options
Add More Tools: Extend the agent by adding tools for specific programming tasks.
Change LLM Model: Replace llama3.2 with other compatible models from Ollama.
Interactive User Queries: Modify the agent to accept real-time user inputs for diverse tasks.
Error Handling: Improve exception handling in the Python code execution function.
📦 Deployment and Testing
Launch Interactive Xterm in Colab
bash
Copy code
%xterm
Run the Agent
After integrating tools, test the agent by interacting with it for various programming tasks.

