# The MCP Lifecycle: How the Architecture Works

## 1. Stage 1: Initialization (The Handshake)

This phase is the first interaction between the Client and Server, establishing the rules for the session.

### Step-by-Step Process (Three Steps)

| Step | Action | Description |
| :--- | :--- | :--- |
| **1. Request** | **Client sends `initialize` request** [00:05:53]. | The Client sends a JSON-RPC message to the Server containing its **Protocol Version** (e.g., `2025-08-29`), **Client Capabilities** (what it can do for the Server), and **Implementation Info** (name/version) [00:06:16]. |
| **2. Response** | **Server sends `initialize` response** [00:07:28]. | The Server responds with its own **Protocol Version**, **Server Capabilities** (what it offers, e.g., Tools, Resources), and its **Implementation Info** [00:07:40]. |
| **3. Notification** | **Client sends `initialized` notification** [00:08:23]. | If the negotiation is successful, the Client sends a final, ID-less JSON-RPC notification to confirm the connection is ready. |

### Negotiation and Rules
* **Version Negotiation:** If the Client and Server protocol versions mismatch, the Client checks its supported list. If the Server's version is incompatible, the connection fails.
* **Safety Rules:** Neither the Client nor the Server is allowed to send any requests (other than `ping` or logging messages) until the final **`initialized`** notification is exchanged.

### Capability Negotiation
During initialization, the Client and Server exchange and negotiate the features they support [00:16:59].

| Party | Major Capabilities Offered | Description |
| :--- | :--- | :--- |
| **Client** | **Roots** | Giving the Server read/write access to the Host's base directory (e.g., a project folder). |
| | **Sampling** | Allowing the Server to ask the Client's LLM for help (e.g., asking the Client to summarize a document for the Server). |
| | **Elicitation** | Allowing the Server to request missing information from the Client (e.g., asking for an API key that was omitted). |
| **Server** | **Tools** | Actions the Server can perform (e.g., code push, file creation). |
| | **Resources** | Static documents the Server can provide. |
| | **Prompts** | Templates to guide the Client's behavior. |
| | **Log** | Sending periodic updates/status messages for long-running tasks. |

***

## 2. Stage 2: Operation (The Work)

In this stage, the Client and Server exchange messages based on the capabilities negotiated during Initialization.

### Part 1: Capability Discovery
Immediately after Initialization, the Client automatically attempts to discover the exact capabilities of the Server.
* The Client sends batch JSON-RPC requests using standard methods: `tools/list`, `resources/list`, and `prompts/list`.
* The Server returns a detailed list, including the arguments and schemas for every available Tool and Resource. The Host stores this information to map user requests to the correct tool.

### Part 2: Tool Calling
The Host/LLM selects the best tool and instructs the Client to call it.
* The Client sends a JSON-RPC request using the `tools/call` method, specifying the tool name and required arguments.
* The Server executes the task (e.g., reading a file, listing a directory) and returns the result in the JSON-RPC response.

***

## 3. Stage 3: Shutdown (The Termination)

This is the termination phase where the continuous connection (session) between the Client and Server is ended, typically initiated by the Host closing.

* **No JSON-RPC Messages:** Unlike the other two stages, no JSON-RPC messages are exchanged during the Shutdown phase. Termination is handled entirely by the **Transport Layer**.

| Server Type | Transport Layer Used | Shutdown Mechanism |
| :--- | :--- | :--- |
| **Local Server** | **STDIO** | The Client closes the Server's input stream (STDIN). If the Server doesn't exit, the Client sends low-level OS signals (`SIGTERM`, then `SIGKILL`) to force termination. |
| **Remote Server** | **HTTP** | The Client simply closes the persistent HTTP connection [02:36:50]. The Client must be prepared to handle an unexpected Server-initiated closure (dropped connection). |

***

## 4. Special Case Scenarios

The regular lifecycle is supplemented by several advanced protocols for maintaining session health and providing feedback.

| Scenario | Protocol/Method | Purpose |
| :--- | :--- | :--- |
| **Session Health** | **Pings** (`ping` method) | A lightweight request/response sent periodically by either side to confirm the connection is still alive and prevent proxies/firewalls from silently dropping the line. |
| **Error Handling** | **JSON-RPC Error Object** | Standardized error messages (e.g., "Method Not Found" with code `-32601`) used when a request fails due to invalid parameters, server failure, or protocol violation. |
| **Time Out** | **Cancellation Notification** | If a Server fails to respond within a set threshold (e.g., 30 seconds), the Client triggers a timeout, sending a `notifications/cancelled` message to the Server to stop processing and free up resources. |
| **Progress Tracking** | **Progress Notification** | For long-running tasks (e.g., scanning a large codebase), the Server sends periodic `notifications/progress` updates using a **Progress Token**. This provides the user with real-time feedback on the task's completion status. |