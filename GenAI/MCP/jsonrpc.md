## JSON-RPC 2.0: The Standardized Language

**JSON-RPC (JavaScript Object Notation - Remote Procedure Call)** is a lightweight, standardized protocol that dictates the format and rules for calling functions on a remote server. In MCP, it serves as the **Data Layer**—the "language and grammar" that allows the Client (AI chatbot) and Server (tool) to understand each other.

### Core Concept: RPC
The fundamental idea is to allow a program to **execute a function on another computer as if it were a local function**. This is achieved by sending a highly structured message—the JSON-RPC request—over the network.

### Key Features and Structure
All JSON-RPC messages are simple JSON objects structured around the remote procedure call concept:

| Field | Purpose |
| :--- | :--- |
| **`jsonrpc`** | Specifies the protocol version (`"2.0"`). |
| **`method`** | **(Request)** The name of the function to be executed on the Server (e.g., `tools/call`). |
| **`params`** | **(Request)** The arguments needed for the function. |
| **`result` / `error`** | **(Response)** The result of the call or a standardized error object. |
| **`id`** | A unique ID to match requests and responses. If omitted, the message is a **Notification**. |

### Advantages in MCP
JSON-RPC was chosen over alternatives like REST APIs because it offers:

* **Transport Agnosticism:** It doesn't rely on HTTP, allowing MCP to use **STDIO** for local communication and **HTTP** for remote communication.
* **Bidirectional Communication & Notifications:** It allows the **Server to initiate messages** to the Client (e.g., resource updates), which is not easily possible with REST.
* **Batching:** Multiple function calls can be bundled into a single request, improving efficiency.

---

## SSE (Server-Sent Events): The Streaming Mechanism

**SSE (Server-Sent Events)** is an extension of the **HTTP** protocol used as part of the **Transport Layer** for **Remote Servers** in MCP.

### Core Concept: Unidirectional Streaming
SSE establishes a **single, long-lived HTTP connection** from the client to the server. The key difference is that the server keeps the connection open and can **continuously push new data (events)** to the client.

### Role in AI and MCP
SSE is essential for providing a modern, responsive user experience in AI applications:

* **Streaming Responses:** Instead of sending one massive JSON-RPC response at the end of a long process, the server streams the data in small **chunks** as soon as they are ready. This creates the familiar word-by-word typing effect in chatbots.
* **Incremental Updates:** For **long-running agentic tasks** (where the AI is executing multiple steps), SSE ensures the user receives real-time status updates and progress notifications, preventing timeouts and improving transparency.

### Transport