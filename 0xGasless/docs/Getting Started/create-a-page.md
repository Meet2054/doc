---
sidebar_position: 1
---
# Quickstart

## Video is there 

## Using the Template

The fastest way to get started is by using our **Python or NodeJS** Replit templates or cloning the **Python or NodeJS** GitHub repositories.

  ### Step 1: Set Up Your Development Environment
  1. Fork the template from **Python or NodeJS** Replit templates.
  2. Once forked, you'll have your own version of the project to modify.

### Step 2: Obtain Required API Keys

####  0xGasless API Key
- Go to **0xGasless Portal**.
- Generate an API key and save both the key name and private key.

#### OpenAI API Key
- Go to [OpenAI Platform](https://platform.openai.com). (AgentKit is model-agnostic, but we'll use OpenAI for this guide.)
- If you're using the NodeJS template, we use [xAI](https://docs.x.ai/docs/overview) by default since they offer free credits.
- Sign up or log in to your account.
- Navigate to the [API keys](https://platform.openai.com/api-keys) section.
- Create a new API key.
- Fund your account with at least $1-2 for testing.

> **Caution**: Never commit API keys to version control or share them publicly. Use environment variables or secure secret management.

### Step 3: Configure Your Environment

#### Replit
If using Replit, in your Replit project:

1. Click on "Tools" in the left sidebar.
2. Select "Secrets."
3. Add the following secrets:
   ```
   0xGasless_API_KEY_NAME=your_0xGasless_key_name
   0xGasless_API_KEY_PRIVATE_KEY=your_0xGasless_private_key
   OPENAI_API_KEY=your_openai_key # Or XAI_API_KEY if using the NodeJS template
   NETWORK_ID="base-sepolia" # Optional, defaults to base-sepolia
   MNEMONIC_PHRASE=your_mnemonic_phrase # Optional, if it is not provided the agent will create a new wallet
   ```

#### Local Development with Cloned Repository
If using the GitHub repository, you can set these environment variables in your terminal using the following command:

```bash
export ENV_NAME="your_environment_variable_name"
```

### Step 4: Run the Agent

#### Replit
You can start this chatbot by clicking the "Run" button.

> **Security of Wallets on Replit Template**: Every agent comes with an associated wallet. Wallet data is read from `wallet_data.txt`, and if that file does not exist, this Replit will create a new wallet and persist it in a new file. Please note that this contains your wallet's private key and should not be used in production environments. Refer to the **0xGasless docs** on how to secure your wallets.
---
#### Local Development with Cloned Repository
Ensure that you have Python 3.10+ installed. You can check your Python version by running:

```bash
python --version
pip --version
```

Start the chatbot by executing:

```bash
python3 examples/chatbot/chatbot.py
```

See the README.md here for more details:

```bash
cd 0xGasless-langchain/examples/chatbot
pip install 0xGasless-langchain
python chatbot.py
```

## Starting from Scratch with LangChain

For developers who want more control over their agent implementation, you can start from scratch using LangChain integration.

### Prerequisites
- [Python 3.10+](https://www.python.org/downloads/)
- **0xGasless API Key**
- API keys to the LLM of your choice (we recommend [OpenAI](https://platform.openai.com/docs/quickstart#create-and-export-an-api-key)).
- A fresh directory.

### Installation
To use the 0xGasless AgentKit Toolkit with LangChain, first install the package:

```bash
pip install 0xGasless-langchain
```

### Environment Setup
Set the required environment variables:

```bash
export 0xGasless_API_KEY_NAME="your-0xGasless-key-name"
export 0xGasless_API_KEY_PRIVATE_KEY="your_0xGasless_private_key"
export OPENAI_API_KEY="your-openai-key" # You can use any LLM provider, but we'll use OpenAI for this guide
export NETWORK_ID="base-sepolia"  # Optional, defaults to base-sepolia
```

### Creating Your First Agent
Create a new file `my_agent.py`, which will contain your agent logic:

```python
from langchain_openai import ChatOpenAI
from 0xGasless_langchain.agent_toolkits import 0xGaslessToolkit
from 0xGasless_langchain.utils import 0xGaslessAgentkitWrapper
from langgraph.prebuilt import create_react_agent
from langchain_core.messages import HumanMessage

# Initialize the LLM
# Replace this line to support Claude, for example
llm = ChatOpenAI(model="gpt-4o-mini")  

# Initialize 0xGasless AgentKit wrapper
0xGasless = 0xGaslessAgentkitWrapper()

# Create toolkit from wrapper
0xGasless_toolkit = 0xGaslessToolkit.from_0xGasless_agentkit_wrapper(0xGasless)

# Get all available tools
tools = 0xGasless_toolkit.get_tools()
```
# Adding Agent Functionality
Extend your agent with chat capabilities. To add more functionality, see the **Add Agent Capabilities** guide.

```
# Create the agent
agent_executor = create_react_agent(
    llm,
    tools=tools,
    state_modifier="You are a helpful agent that can interact with the Base blockchain using 0xGasless AgentKit. You can create wallets, deploy tokens, and perform transactions."
)

# Function to interact with the agent
def ask_agent(question: str):
    for chunk in agent_executor.stream(
        {"messages": [HumanMessage(content=question)]},
        {"configurable": {"thread_id": "my_first_agent"}}
    ):
        if "agent" in chunk:
            print(chunk["agent"]["messages"][0].content)
        elif "tools" in chunk:
            print(chunk["tools"]["messages"][0].content)
        print("-------------------")

# Test the agent
if __name__ == "__main__":
    print("Agent is ready! Type 'exit' to quit.")
    while True:
        user_input = input("\nYou: ")
        if user_input.lower() == 'exit':
            break
        ask_agent(user_input)
```

### Testing Your Agent
Try these example interactions:

```
You: What is your wallet address?
You: Deploy an NFT collection called 'Cool Cats' with symbol 'COOL'
You: Register a basename for yourself that represents your identity
