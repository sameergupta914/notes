# Model Context Protocol (MCP) - The Why

## 2. The Problem: Fragmentation and Context Assembly

The accessibility of AI created a new problem known as **The Problem of Fragmentation** 

### The Fragmentation Issue
We now live in "multiple AI worlds" (e.g., Notion's AI, Slack's AI, VS Code's AI). These systems are isolated, with no awareness of what is happening in the others, forcing users to juggle between them [00:10:43].

### The Core Problem: Context
The vision was a **Unified AI Agent** that understands and solves problems across our entire workflow [00:11:31]. The biggest obstacle to this vision is the **Problem of Context** [00:12:13].
* **Definition:** Context is **"everything an AI can see when it generates a response"** (e.g., conversation history, external documents) [00:12:43].
* **Professional Context is Scattered:** In a real-world scenario (e.g., a developer implementing 2FA), the necessary context is spread across multiple, unconnected systems [00:16:42]:
    * **Task/Requirements:** JIRA ticket
    * **Code:** GitHub repository
    * **Data:** MySQL schema
    * **Guidelines:** Google Drive security document
    * **Discussion:** Slack/Teams chat

### The "Human API" Problem
To get help from the AI, the developer must manually copy and paste all this scattered information into the chatbot [00:17:10].
* Developers become **"Human APIs,"** whose job is to assemble the context for the AI [00:18:38].
* This is not scalable, especially with large projects (e.g., 500k lines of code) [00:19:15].

---

## 3. The Flawed Solution: Function Calling

In mid-2023, OpenAI introduced **Function Calling** as a solution to the Context Assembly problem [00:21:23].

### How Function Calling Worked
Function Calling allowed an LLM to request the execution of an **external function** or "tool" [00:21:43].
1.  The developer provides the LLM with a list of functions and their descriptions (e.g., `load_file` to read content).
2.  When the user prompts, the LLM determines which function to invoke and with what arguments (e.g., "Read the content of `abc.txt`") [00:22:23].
3.  The task is executed, and the result is returned to the LLM [00:22:53].

This was revolutionary because the AI could now fetch scattered context automatically (e.g., from a database or GitHub) [00:28:16].

### The New Problem: The N x M Integration Nightmare
Function Calling created a massive development overhead, leading to a new problem—the **Integration Problem** [00:29:00].
* **The Math:** If a company has **N** AI chatbots (Client) and wants to connect them to **M** services (Server), they have to write **N x M** unique code integrations/functions [00:31:02].
* **Developer Nightmare:** Every developer had to write custom function code for:
    * Authentication
    * API Patterns
    * Data Format Translation
    * Error Handling
    * Maintaining this custom code in-house [00:31:33].
* **Cost & Time:** A whole new team of developers was required just to create and maintain these non-standardized integrations, defeating the initial goal of increased developer productivity [00:33:58].

**The Core Issue:** **"Every AI tool is building its own way to call every API"** [00:35:31].

---

## 4. The True Solution: Model Context Protocol (MCP)

MCP was developed to solve the **N x M Integration Nightmare** by introducing a standard language for AI-to-tool communication [00:36:05].

### MCP Architecture and Difference
MCP follows a Client-Server model, but with a critical difference from Function Calling:

| Feature | Function/Tool Calling | Model Context Protocol (MCP) |
| :--- | :--- | :--- |
| **Code Location** | Client (AI Chatbot) and Server (API) both require custom code [00:41:20]. | **Server** does the heavy lifting [00:42:08]. |
| **Client-Side** | Requires custom code (a Python function, for example) for every tool [00:40:03]. | Requires **no custom code**; only a configuration file and an **MCP Client SDK** [00:40:52]. |
| **Integration** | Custom-built for every unique N x M pair [00:35:14]. | Standardized via **MCP Server SDK**; one server works with any MCP-compliant client [00:38:56].

### The Benefits of MCP
By shifting the heavy lifting to the server, MCP delivers significant benefits:

| Benefit | Description |
| :--- | :--- |
| **Reduced Integrations** | The problem changes from **N x M** integrations to a simpler **M + N** architecture. The service providers (GitHub, Google Drive) write the servers, offloading all custom integration code from the client (AI chatbot) [00:44:07]. |
| **No Maintenance Overhead** | The client has no code to maintain. If a server API changes, the service provider updates the server, and the client sees no disruption [00:44:53]. |
| **Reduced Cost and Time** | An AI chatbot can connect to dozens of services on **Day One** simply by maintaining a single configuration file, eliminating months of integration development time [00:45:15].
| **Better Security** | All keys and tokens are centrally managed in a single, simple configuration file (`JSON`) instead of being scattered across dozens of individual function files [00:46:00]. |

### The Network Effect
MCP is rapidly becoming an industry standard because of a continuous feedback loop [00:50:24]:
1.  **AI Clients** (e.g., Claude Desktop, Perplexity) announce MCP support.
2.  **Service Providers** (e.g., GitHub, Slack) feel pressure to build official MCP Servers to ensure their service is accessible via the most popular AI tools [00:48:19].
3.  **More Servers** make the MCP ecosystem more valuable.
4.  **New AI Clients** automatically adopt MCP to gain instant access to all existing servers **without writing custom code** [00:50:17].

Any company that does not adopt MCP risks being cut off from this massive, interconnected ecosystem [00:50:42].