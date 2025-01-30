# Azure AI Agent Service-enterprise-demo

This sample demonstrates how to build a streaming enterprise agent using **Azure AI Agent Service**. The agent can answer questions in real time using local HR and company policy documents, integrate external context via Bing, using gpt-4o-2024-05-13.

![gif demo](https://github.com/Azure-Samples/azure-ai-agent-service-enterprise-demo/blob/main/assets/demo-short-3-2.gif)

## Features

This demo teaches developers how to:

- **Create or Reuse Agents Programmatically**  
  Demonstrates how to connect to an Azure AI Foundry hub, either create a new agent with customized instructions (using GPT-4o or any supported model), or reuse an existing agent.

- **Incorporate Vector Stores for Enterprise Data**  
  Shows how to automatically create or reuse a vector store containing local policy files (HR, PTO, etc.), enabling retrieval-augmented generation (RAG).

- **Integrate Server-Side Tools**  
  Illustrates adding tools—like Bing search, file search, and custom Python functions—into a single `ToolSet`, and how to intercept and log each tool call.

- **Stream Real-Time Agent Responses**  
  Demonstrates a streaming approach for partial message updates from the agent, seamlessly handling partial tool invocation and chunked text output.

- **Build an Interactive Gradio UI**  
  Provides a Gradio-based chat interface that prompts the agent with user questions, displays partial tool calls and final results, and makes it easy to extend or adapt the UI.

Use this demo as a reference for creating, deploying, and managing enterprise-scale AI agents with strong integration, data security, and real-time conversation capabilities.

## Getting Started

### Prerequisites

- **Python 3.9+**  
- **Visual Studio Code** with the Python and Jupyter extensions  
- An **Azure AI Foundry** resource set up (see [Azure AI Agent Service docs](https://learn.microsoft.com/en-us/azure/ai-services/agents/))

### Installation & Setup

1. **Clone** this repository:

   ```bash
   git clone https://github.com/Azure-Samples/azure-ai-agent-service-enterprise-demo.git
   ```

2. **Create a virtual environment** (using venv as an example):

    ```bash
    python -m venv .venv
    ```

3. **Activate** your virtual environment:

    - Windows: `.venv\Scripts\activate`
    - macOS/Linux: `source .venv/bin/activate`

4. **Install** the required dependencies:

    ```bash
    pip install -r requirements.txt
    ```

5. **Create a `.env` file** at the root of this folder to store secret keys and settings (e.g., the connection string and optional model name). You can copy the provided `.env.example` file:

    - Windows (PowerShell):
      ```powershell
      Copy-Item -Path .env.example -Destination .env
      ```
    
    - macOS/Linux:
      ```bash
      cp .env.example .env
      ```

    Then, open the `.env` file and update it with your configuration details.

    - Add your [Azure AI Foundry](https://learn.microsoft.com/en-us/azure/ai-services/agents/quickstart?pivots=programming-language-python-azure#configure-and-run-an-agent) connection string:

        ```plaintext
        PROJECT_CONNECTION_STRING="<HostName>;<AzureSubscriptionId>;<ResourceGroup>;<ProjectName>"
        ```

    - Specify the [compatible model](https://learn.microsoft.com/en-us/azure/ai-services/agents/how-to/tools/bing-grounding?tabs=python&pivots=overview#setup) you want to use (e.g. GPT-4o):
        ```plaintext
        MODEL_NAME="YOUR_MODEL_NAME"
        ```

    - (Optional) **Grounding with Bing**
        
        You can add real-time web data to your agent via Grounding with Bing Search. For details on how to create a Bing search resource, link it with your Azure AI Agent, and meet display requirements, see Grounding with Bing Search.

        ```plaintext
        BING_CONNECTION_NAME="YOUR_CONNECTION_NAME"
        ```

        > In this sample, the code automatically tries to discover an .env variable named `BING_CONNECTION_NAME`. If available, you’ll see a console message like `bing > connected`. Otherwise, it gracefully proceeds without Bing.

    - (Optional) **OpenWeather** API keys to enable `fetch_weather` tool:
        ```plaintext
        OPENWEATHER_GEO_API_KEY="YOUR_OPENWEATHER_GEOCODING_API_KEY"
        OPENWEATHER_ONE_API_KEY="YOUR_OPENWEATHER_ONE_CALL_API_KEY"
        ```
        If you leave these blank, the weather function will simply return an error or remain disabled.

        > **Tip**: If you don’t plan to use Bing grounding or OpenWeather, you can skip setting up those resources. The demo will still work with your local documents and core agent features.

    > Make sure that .env is listed in your .gitignore. Never commit your credentials to source control!

6. **Open** the project folder in Visual Studio Code:

    - Select your Python interpreter:
      1. Press Ctrl+Shift+P (or Cmd+Shift+P on Mac)
      2. Choose **Python: Select Interpreter** and select the `venv` environment.

### Quickstart

1. **Run Jupyter Notebook:**
    - Open the `enterprise-streaming-agent.ipynb`, in VS Code.
    - Step through the cells to:
      1. Connect to Azure AI Foundry and create or reuse an agent.
      2. Optionally upload local HR/policy files to a vector store.
      3. Add Bing integration, local file search, and custom Python functions (weather, stock lookup, etc.) to the agent’s ToolSet.
      4. See real-time streaming queries and partial responses via your console logs and Gradio chat UI.

2. **Try the Demo Chat:**
    - When you run the notebook, a local Gradio instance should launch in your cell output. You can click the localhost link to open the chat UI.
    - Ask the agent questions like:
        - “What’s my company’s remote work policy?”
        - “Fetch the weather forecast for Seattle tomorrow.” (Requires valid OpenWeather keys)
        - “How is MSFT stock price trending today?”
        - “Send an email summary of the HR policy.”

## Resources
- [Azure AI Agent Service Documentation](https://learn.microsoft.com/en-us/azure/ai-services/agents/overview)
- [Grounding with Bing Search](https://learn.microsoft.com/en-us/azure/ai-services/agents/how-to/tools/bing-grounding)
- [OpenWeather API](https://openweathermap.org/api)

## Acknowledgments

- **[Gradio](https://github.com/gradio-app/gradio)**  
  This project uses Gradio under the [Apache License 2.0](https://github.com/gradio-app/gradio/blob/main/LICENSE). No modifications to Gradio’s source code are distributed in this repository.
