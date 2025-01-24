---
sidebar_position: 3
---
# Integrate Langchain Tools

LangChain has revolutionized the way developers interact with language models and build powerful AI applications. One of its most compelling features is the extensive ecosystem of tools and integrations that allow developers to quickly and easily extend their agents' capabilities.

## The Power of LangChain Tools
LangChain's true strength lies in its [vast array of community-supported tools and integrations](https://python.langchain.com/docs/integrations/tools/). These tools enable developers to:

- **Rapidly expand agent capabilities**: Integrate with various APIs, databases, and services without writing extensive custom code
- **Leverage specialized functionalities**: Access domain-specific tools for tasks like image generation, social media posting and consumption, internet search, data analysis, or blockchain interactions
- **Create multi-modal agents**: Combine different types of interactions (text, image, code) within a single agent
- **Stay up-to-date**: Benefit from a constantly growing ecosystem of tools maintained by the community

By utilizing these tools, developers can create sophisticated AI agents that can perform a wide range of tasks, from generating images to sending emails, all through natural language interfaces.

## Adding the Dall-E Image Generator to Your Agent
In this guide, we'll walk through the process of adding the Dall-E Image Generator tool to an existing LangChain agent. This will demonstrate how easily you can enhance your agent's capabilities using community toolkits.

### Prerequisites
- An existing AgentKit setup, like the one in our Replit template
- Python 3.10+
- OpenAI API key

### Step 1: Install Required Packages
First, ensure you have the necessary packages installed:

```bash
pip install langchain-community
```

### Step 2: Import Required Modules
Add the following imports to your existing imports:

```python
from langchain.agents import load_tools
from langchain_community.utilities.dalle_image_generator import DallEAPIWrapper
```

### Step 3: Set Up OpenAI API Key
If you haven't already, set up your OpenAI API key as an environment variable and ensure the account is funded:

```bash
export OPENAI_API_KEY="your_api_key"
```

### Step 4: Load the Dall-E Tool
Before initializing your agent, load the Dall-E tool:

```python
dalle_tool = load_tools(["dalle-image-generator"])
```

### Step 5: Combine Tools
Add the Dall-E tool to your existing tools:

```python
all_tools = cdp_toolkit.get_tools() + dalle_tool
```

### Step 6: Update Agent Initialization
Modify your create_react_agent call to include the new tools:

```python
def initialize_agent():
  """Initialize the agent with CDP AgentKit and Dall-E."""
  llm = ChatOpenAI(model="gpt-4o-mini")

  # ... (existing wallet data handling code) ...

  agentkit = CdpAgentkitWrapper(**values)

  # ... (existing wallet data saving code) ...

  cdp_toolkit = CdpToolkit.from_cdp_agentkit_wrapper(agentkit)
  cdp_tools = cdp_toolkit.get_tools()

  dalle_tool = load_tools(["dalle-image-generator"])
  all_tools = cdp_tools + dalle_tool

  memory = MemorySaver()
  config = {"configurable": {"thread_id": "CDP AgentKit Chatbot Example!"}}

  return create_react_agent(
    llm,
    tools=all_tools,
    checkpointer=memory,
    state_modifier="You are a helpful agent that can interact onchain on the Base Layer 2 using the Coinbase Developer Platform AgentKit. You are also capable of generating images using Dall-E. You are empowered to interact onchain using your tools and generate images when requested. If you ever need funds, you can request them from the faucet. You can also deploy your own ERC-20 tokens, NFTs, and interact with them. If someone asks you to do something you can't do, you can say so, and encourage them to implement it themselves using the CDP SDK + Agentkit, recommend they go to docs.cdp.coinbase.com for more information.",
  ), config
```

Now your agent is equipped with the ability to generate images using Dall-E alongside its existing CDP capabilities. You can test it by asking the agent to generate images through natural language requests.

For more information on available tools and integration options, visit the [LangChain documentation](https://python.langchain.com/docs/how_to/#tools).
