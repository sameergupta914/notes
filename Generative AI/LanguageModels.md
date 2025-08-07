#Language Models

- Definition: Language Models are AI systems designed to process, generate, and understand natural language text.

- Body (including diagram annotations transcribed as best as can be inferred):
(There’s a diagram showing: input → Language Models → text, with branches to “LLMs” and “Chat Models”, with handwritten notes like “general purpose” and “convos”.)

- LLMs - General-purpose models that is used for raw text generation. They take a string (or plain text) as input and returns a string (plain text). These are traditionally older models and are not used much now.

- Chat Models - Language models that are specialized for conversational tasks. They take a sequence of messages as inputs and return chat messages as outputs (as opposed to using plain text). These are traditionally newer models and used more in comparison to the LLMs.

| Feature              | LLMs (Base Models)                                                             | Chat Models (Instruction-Tuned)                                              |
| -------------------- | ------------------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| **Purpose**          | Free-form text generation                                                      | Optimized for multi-turn conversations                                       |
| **Training Data**    | General text corpora (books, articles)                                         | Fine-tuned on chat datasets (dialogues, user-assistant conversations)        |
| **Memory & Context** | No built-in memory                                                             | Supports structured conversation history                                     |
| **Role Awareness**   | No understanding of "user" and "assistant" roles                               | Understands "system", "user", and "assistant" roles                          |
| **Example Models**   | GPT-3, Llama-2-7B, Mistral-7B, OPT-1.3B                                        | GPT-4, GPT-3.5-turbo, Llama-2-Chat, Mistral-Instruct, Claude                 |
| **Use Cases**        | Text generation, summarization, translation, creative writing, code generation | Conversational AI, chatbots, virtual assistants, customer support, AI tutors |


