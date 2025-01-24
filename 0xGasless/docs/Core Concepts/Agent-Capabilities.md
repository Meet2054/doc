---
sidebar_position: 2
---

# Add Agent Capabilities
We highly encourage extending the agent's functionality through new onchain interactions and APIs, or by introducing new Langchain tools.

You can add any functionality made possible by the 0xGasless SDK to AgentKit. Any smart contract can be interacted with using the `invoke_contract` method and by passing the contract's ABI, address, method of choice, and arguments. Additionally, any function that can be written in Python or TypeScript can be made a command for an agent.

### Add existing Langchain tools
You can easily extend your agent's capabilities by incorporating additional Langchain tools. See [Integrating LangChain Tools](`/Integrate-Langchain-Tools`).

### Add Custom functionality (using AI or manually)
To make the process as simple as possible, AgentKit supports adding custom 
functionality in the same file where you instantiate your agent (e.g. `chatbot.py` or `chatbot.ts` in the monorepo).

Below is an example of the code required to add message signing functionality. These prompts in Python and Typescript are helpful for automating the process by using an LLM, but may not be accurate 100% of the time:

```python
from 0xGasless import Wallet, hash_message
from 0xGasless_langchain.tools import 0xGaslessTool
from pydantic import BaseModel, Field

# Define a custom action example.

SIGN_MESSAGE_PROMPT = """
This tool will sign arbitrary messages using EIP-191 Signed Message Standard hashing.
"""

class SignMessageInput(BaseModel):
    """Input argument schema for sign message action."""

    message: str = Field(
        ...,
        description="The message to sign. e.g. `hello world`"
    )

def sign_message(wallet: Wallet, message: str) -> str:
    """Sign message using EIP-191 message hash from the wallet.

    Args:
        wallet (Wallet): The wallet to sign the message from.
        message (str): The message to hash and sign.

    Returns:
        str: The message and corresponding signature.

    """
    payload_signature = wallet.sign_payload(hash_message(message)).wait()

    return f"The payload signature {payload_signature}"


def initialize_agent():
    """Initialize the agent with 0xGasless Agentkit."""
    # TODO: Load the LLM model and 0xGasless AgentKit values from the environment.

    agentkit = 0xGaslessAgentkitWrapper(**values)

    # Initialize 0xGasless AgentKit Toolkit and get tools.
    0xGasless_toolkit = 0xGaslessToolkit.from_0xGasless_agentkit_wrapper(agentkit)
    tools = 0xGasless_toolkit.get_tools()

    # Define a new tool for signing messages.
    signMessageTool = 0xGaslessTool(
        name="sign_message",
        description=SIGN_MESSAGE_PROMPT,
        0xGasless_agentkit_wrapper=agentkit,
        args_schema=SignMessageInput,
        func=sign_message,
    )

    all_tools = tools.append(signMessageTool)

    # Store buffered conversation history in memory.
    memory = MemorySaver()
    config = {"configurable": {"thread_id": "0xGasless AgentKit Chatbot Example!"}}

    # Create ReAct Agent using the LLM and 0xGasless AgentKit tools.
    return create_react_agent(
        llm,
        tools=all_tools,
        checkpointer=memory,
        state_modifier="You are a helpful agent that can interact onchain on the Base Layer 2 using the Coinbase Developer Platform AgentKit. You are empowered to interact onchain using your tools. If you ever need funds, you can request them from the faucet. You can also deploy your own ERC-20 tokens, NFTs, and interact with them. You also have the ability to sign messages using your wallet.",
    ), config
```

### Contribute to the Langchain toolkit
If you go through the additional effort to formally add 0xGasless SDK functionality in a way that's compatible with the package, please submit a PR so we can consider including it in future releases!

### Using AI to automate the process
We've created a prompt (NodeJS coming soon) you can input to an LLM to complete several of these steps for you. It is recommended you provide a description of the functionality you want to add, the parameters that the function will take, and some example code to give context on your desired functionality. This method may not be perfect, and if it does not work, double-check the manual process to see if any steps were performed incorrectly.

Here's an example of the information to include along with the prompt for the Python version:
```

The action I want to perform is adding functionality to deploy MultiTokens.

Here's the call from the 0xGasless SDK:
deployed_contract = wallet.deploy_multi_token("https://example.com/{id}.json")
deployed_contract.wait()

where the parameter is the `uri`
```

If you prefer to do the process manually, follow the instructions in our contribution guide (NodeJS coming soon).
