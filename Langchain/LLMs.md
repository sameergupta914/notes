#Models

- in langchain, models are the core interfaces through which you interact with ai models.
- eg:
    from langchain_openai import ChatOpenAI
    from dotenv improt load_dotenv
    load_dotenv()
    model=ChatOpenAI(model='gpt-4', temperature=0)
    result=model.invoke("now divide the result by 1.5")
    print(result.content)

- Takes a formatted prompt (or conversation history) and returns model output (completion or chat response).
- Wraps different backends (OpenAI, Anthropic, local models, etc.) behind a consistent API 

- Embedding model: An embedding model is a neural model that maps pieces of data into high-dimensional numerical vectors such that semantic similarity in the original space is reflected as geometric closeness in vector space.
-eg: Semantic encoding: “Apple” and “banana” get vectors that are closer than “apple” and “car” because their meanings are related.

- language model/chat model: Takes a prompt or conversation (text) and produces new text—completions, answers, summaries, decisions, etc. 
- eg: GPT, Claude, Llama, etc. It’s used for reasoning, answering questions, crafting responses, deciding next actions in agents, etc. 

#prompts

- Zero-shot prompting: Give the model a task in natural language without examples.

- Few-shot prompting: Supply a few input-output examples to demonstrate the pattern before asking the model to do the new instance.

- One-shot / N-shot: Same as few-shot but with exactly one example (one-shot) or N examples.

- Role prompting: Tell the model to adopt a persona or role to shape style/knowledge.
    Example:
    You are an experienced backend engineer. Explain REST vs GraphQL to a junior developer.

- Style / Tone control: Specify adjectives or constraints for output style.
    Example:
    Write a concise, friendly email decline of a meeting request in less than 100 words.

#Chains

- Chains in LangChain are composable workflows—ordered or conditional sequences of operations wrapped behind a simple interface so you can build higher-level applications without wiring everything manually. They implement the Runnable/Chain interface, so you can uniformly invoke, trace, retry, bind inputs, and compose them.

- LLMChain (the basic building block): Takes a prompt template plus an LLM and returns the model’s output for a given input. It’s “one prompt → one response” and is the foundation for more complex chains.

- SequentialChain: Connects multiple chains or runnables in sequence where the output of one feeds into the next. SimpleSequentialChain is a lightweight variant, and SequentialChain is its more flexible successor. Useful for multi-step pipelines like “summarize → classify → format.

#Indexes

- Document loader: Takes raw data from some source (PDFs, web pages, CSVs, plain text, Google Docs, etc.) and turns it into LangChain Document objects.

- Text splitter: Most real-world documents are too big to embed or feed into an LLM in one go. The text splitter chops each loaded document into smaller, overlapping pieces (chunks) according to rules you configure—like maximum chunk size, overlap, and preferred separators—so you preserve context while keeping each piece manageable.

- Vector store(database where vector are stored): After splitting, you convert each chunk into a vector embedding (via an embedding model) that captures its semantic meaning. The vector store is the database that holds those vectors (plus their associated chunk text/metadata) and lets you do similarity search efficiently

- Retrievers: it picks the query asked and generates its embedding wiht the help of a embedding model, and then whatever vector we get, we take that vector and do a semantic search in that vector database, relevant result which came from it are then put together with user query are then send to llm, which then replies to you.

#Memory

- LLM api calls are stateless.

- 1.ConversationBufferMemory: Stores the full conversation history verbatim (all messages) and feeds it back to the model each turn. Simple and intuitive; good for short chats or debugging.

- 2.ConversationSummaryBufferMemory: Keeps a running summary of earlier parts of the conversation plus a buffer of recent exchanges. It compresses older context to control token growth while preserving continuity.

- 3.ConversationBufferWindowMemory: Like buffer memory but only retains the last N messages (a sliding window). Keeps context recent and bounded by number of turns instead of full history. 

- 4.ConversationSummaryMemory: Periodically summarizes the whole conversation into a compact form (without necessarily keeping recent raw turns). Good when you want a distilled “what’s happened so far” and don’t need verbatim recent messages.

#Agents

- main diff bw ai agents and chat bot that ai agents thave reasoning capabilities and access to tools.

- Core pieces of an AI agent (the mental model):
    -Perception/Input: It takes in observations—text prompts, sensor data, search results, API replies, etc.

    -Internal state / memory: Keeps context or knowledge from past interactions (short-term buffer, summarized history, extracted facts, etc.).

    -Reasoning / Planning: Decides what to do next. In classical agents this might be rule logic or planning algorithms; in modern LLM agents this is the language model generating the next step (sometimes augmented with strategies like ReAct, Tool-Use, tree-of-thoughts, etc.).

    -Action selection / Tools: Executes an action—answers a question, calls a search API, writes to a database, issues a command to another system, moves a robot limb, etc.

    -Goal / Reward: There is an objective the agent is trying to satisfy (explicit instruction, maximizing some utility, fulfilling a user request).

    -Feedback / Observation update: After acting, it sees the result and updates its state, possibly refining future decisions.

