# Model Context Protocol (MCP) Architecture 

## 1. Core Architecture: Host and Server

The simplest version of the MCP architecture involves two main components [00:02:01]:

| Component | Role | Description |
| :--- | :--- | :--- |
| **Host** | The AI Chatbot | The user-facing application that interacts with an LLM (OpenAI, Anthropic, Gemini). This can be a pre-built app (e.g., Claude Desktop, Cursor IDE) or a custom-built one [00:02:36]. |
| **Server** | The Tool/Service | An external capability that executes a specific task (e.g., GitHub, Slack, Google Drive) [00:03:31]. |

**Simplified Communication Flow:**
1.  **User** sends a prompt to the **Host** (e.g., "Are there any new commits on the GitHub repo?") [00:04:19].
2.  The Host sends the prompt to the **LLM**.
3.  The LLM determines its training data is insufficient and realizes it needs the **GitHub Server** [00:04:40].
4.  The Host queries the GitHub Server.
5.  The Server executes the task and returns the data to the Host.
6.  The LLM formats the final answer, and the Host displays it to the User [00:05:28].

---

## 2. Refined Architecture: Introducing the Client

The Host never communicates directly with the Server. It uses a helper—the **MCP Client** [00:06:18].

* **MCP Client:** An entity that helps the Host communicate with the Server. The Client is the only component that "speaks" the standardized MCP language [00:07:01].
* **Relationship:** The Client-Server relationship is strictly **one-on-one** (one Client connects to only one Server) [00:08:50].
* **Structure:** If a Host needs to connect to multiple Servers (e.g., GitHub, Slack, Drive), it must spawn an equal number of Clients (one Client for GitHub, one for Slack, etc.) [00:09:48].
* **Benefit:** This architecture provides **decoupling** (separation of concerns, failure in one channel doesn't affect others) and **scalability** (allows parallel task execution and connecting to any number of servers) [01:12:26].

| Component | Analogy (Phone) [01:10:06] |
| :--- | :--- |
| **Host** | Phone (Your interface) |
| **Client** | SIM card (Handles communication protocol) |
| **Server** | Network (Airtel/Jio - The actual service provider) |

---

## 3. Primitives: Server Offerings

Primitives are the set of offerings or capabilities that an MCP Server provides to the Host [01:14:48]. There are three main types:

1.  **Tools:**
    * **Purpose:** Actions the AI can ask the Server to **perform** (dynamic data/actions).
    * **Examples:** `get_commit_count()`, `list_active_issues()`, `create_new_file()` [01:15:35].
    * **Standard Operations:** `tools/list` (discover available tools), `tools/call` (execute a tool) [01:22:52].

2.  **Resources:**
    * **Purpose:** Structured data sources or static knowledge the AI can **read**.
    * **Examples:** A GitHub repository's `README.md` file, a database schema, a static security policy document [01:16:54].
    * **Standard Operations:** `resources/list`, `resources/read`, `resources/subscribe` (for change notifications) [01:24:26].

3.  **Prompts:**
    * **Purpose:** Predefined prompt templates or instructions stored on the Server to help **shape the AI's behavior** [01:18:16].
    * **Scenario:** Used to enforce specific formatting (e.g., always including "Steps to Reproduce" and "Environment" when creating a bug report/issue on GitHub) [01:20:41].
    * **Standard Operations:** `prompts/list`, `prompts/get` [01:25:11].

---

## 4. Data Layer: JSON-RPC 2.0

The Data Layer defines the language and grammar used for communication between the Client and Server [01:27:25].

* **Foundation:** MCP uses **JSON-RPC 2.0** as its foundational communication protocol [01:28:40].
    * **JSON-RPC:** Combines the concept of a **Remote Procedure Call (RPC)**—executing a function on a different machine—with the simplicity of **JSON** for structuring requests and responses [01:32:09].
    * **Messages:** All MCP messages are structured JSON objects containing `jsonrpc` version (`2.0`), `method` name (e.g., `tools/list`), optional `params`, and a unique `id` [01:34:40].
    * **Notification:** JSON-RPC supports **notifications** (one-way messages without a required response), which are crucial for features like subscribing to resource updates [01:42:30].

**Why JSON-RPC over REST APIs?** [01:46:53]
1.  **Lightweight:** JSON-RPC requests are simpler, containing less metadata/headers than HTTP-based REST, making them faster to transmit and easier to debug [01:48:33].
2.  **Bidirectional Communication:** REST is mostly one-way (Client request -> Server response). JSON-RPC natively supports two-way communication (Server can also send requests/notifications) [01:49:53].
3.  **Transport Agnostic:** JSON-RPC is flexible and can work with various transport methods (HTTP, STDIO, WebSockets), a critical requirement for MCP [01:50:47].
4.  **Batching:** JSON-RPC allows sending multiple requests simultaneously in a single message payload [01:51:50].

---

## 5. Transport Layer

The Transport Layer is the mechanism that moves the JSON-RPC messages between the Client and Server [01:53:33]. The mode of transport depends on the Server type.

### Server Type 1: Local Servers
* **Definition:** Servers running on the **same computer** as the Host (e.g., a local file system server) [01:55:45].
* **Transport Mechanism:** **STDIO (Standard Input/Output)** [01:59:09].
    * The Host launches the Server as a **sub-process**, creating a Parent-Child relationship [02:01:45].
    * The Host/Client gains control of the Server's **Standard Input** (to send JSON-RPC requests) and **Standard Output** (to receive JSON-RPC responses) [02:02:07].
* **Benefits:** **Fast** (inter-process communication on the same machine), **Secure** (no network ports opened), and **Simple** to implement [02:06:23].

### Server Type 2: Remote Servers
* **Definition:** Servers running on a **different computer** over a network or the internet (e.g., the official GitHub MCP server) [01:56:04].
* **Transport Mechanism:** **HTTP + SSE** [02:07:39].
    * **HTTP:** Used as the base protocol to send requests (as POST requests) and supports standard authentication methods [02:08:10]. The JSON-RPC message is placed inside the HTTP payload [02:08:57].
    * **SSE (Server-Sent Events):** An HTTP extension used for **streaming** long-running or incremental updates back to the Client (similar to how chat responses are streamed) [02:09:43].

### Conclusion on Transport Agnosticism
MCP's brilliant architecture separates the Data Layer (JSON-RPC) from the Transport Layer (STDIO/HTTP) [02:13:54]. This separation is why JSON-RPC was chosen: it can seamlessly use **STDIO** for fast local connections and **HTTP** for remote connections, making the entire protocol future-proof and flexible for any environment [02:12:43].

