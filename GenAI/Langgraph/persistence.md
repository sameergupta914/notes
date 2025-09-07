# Persistence

- Persistence refers to the ability to save and retain data or state information across different sessions or executions of a program. In the context of the provided transcript, persistence is implemented using a checkpoint mechanism that allows the program to store intermediate and final values in memory (specifically in RAM). This is particularly useful for debugging complex workflows, as it enables the user to replay the execution from a specific checkpoint if something goes wrong.

- The transcript also notes that while this checkpoint mechanism is useful for demonstrations, it is not typically used in production setups. In production, different types of checkpoint mechanisms are employed to ensure that data can be reliably saved and retrieved, allowing for more robust handling of workflows without losing important information when a program is restarted or interrupted.

# Checkpointers

- Checkpointers are mechanisms used in programming to save the state of a system at specific points during execution. They allow for the persistence of intermediate and final values in memory, which can be useful for debugging, demonstration purposes, or recovery in case of failure.

- In the context provided, a specific type of checkpointer called an "in-memory saver" is mentioned, which stores all state values in RAM. This is typically used for demos to help understand the workflow, but it is not recommended for production setups because the data would be lost if the program is terminated. In production environments, other types of checkpointers, such as those using databases like PostgreSQL or Redis, are utilized to ensure data persistence beyond the runtime of the program.

- Checkpointers have unique IDs that correspond to each saved state, allowing users to retrieve specific states later in the workflow. This functionality is particularly useful for managing different configurations or versions of data, enabling users to access and manipulate historical states as needed.

# Thread ID

- The thread ID is a unique identifier used during the execution of a workflow that involves persistence. When executing a workflow, you assign a thread ID to store the intermediate and final state values in a database. This allows you to retrieve specific execution states later by querying the database with the corresponding thread ID.

- For example, if you execute a workflow with a thread ID of one, all values generated during that execution will be stored against that thread ID in the database. If you later execute the workflow again with a different initial value and assign it a thread ID of two, the new values will be stored against thread ID two. This system enables you to easily access and manage different execution states by referencing their respective thread IDs, ensuring that all data is organized and retrievable based on the specific execution context.

# Code implementation

from langgraph.graph import StateGraph, START, END
from typing import TypedDict
from langchain_openai import ChatOpenAI
from dotenv import load_dotenv
from langgraph.checkpoint.memory import InMemorySaver

load_dotenv()

llm = ChatOpenAI()
class JokeState(TypedDict):

    topic: str
    joke: str
    explanation: str

def generate_joke(state: JokeState):

    prompt = f'generate a joke on the topic {state["topic"]}'
    response = llm.invoke(prompt).content

    return {'joke': response}
def generate_explanation(state: JokeState):

    prompt = f'write an explanation for the joke - {state["joke"]}'
    response = llm.invoke(prompt).content

    return {'explanation': response}

graph = StateGraph(JokeState)

graph.add_node('generate_joke', generate_joke)
graph.add_node('generate_explanation', generate_explanation)

graph.add_edge(START, 'generate_joke')
graph.add_edge('generate_joke', 'generate_explanation')
graph.add_edge('generate_explanation', END)

checkpointer = InMemorySaver()

workflow = graph.compile(checkpointer=checkpointer)


config1 = {"configurable": {"thread_id": "1"}}
workflow.invoke({'topic':'pizza'}, config=config1)
o/p:
{'topic': 'pizza',
 'joke': 'Why did the pizza go to the doctor? Because it was feeling a little saucy!',
 'explanation': 'This joke plays on the double meaning of the word "saucy." In one sense, "saucy" can mean bold, impertinent, or sassy. But in the context of a pizza, "saucy" refers to the tomato sauce typically used as a base on pizzas. So when the pizza went to the doctor because it was feeling "saucy," it implies that the pizza was not feeling well due to too much sauce, rather than being bold or sassy. The humor comes from the unexpected twist on the word\'s meaning.'}

workflow.get_state(config1)
o/p:
StateSnapshot(values={'topic': 'pizza', 'joke': 'Why did the pizza go to the doctor? Because it was feeling a little saucy!', 'explanation': 'This joke plays on the double meaning of the word "saucy." In one sense, "saucy" can mean bold, impertinent, or sassy. But in the context of a pizza, "saucy" refers to the tomato sauce typically used as a base on pizzas. So when the pizza went to the doctor because it was feeling "saucy," it implies that the pizza was not feeling well due to too much sauce, rather than being bold or sassy. The humor comes from the unexpected twist on the word\'s meaning.'}, next=(), config={'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '1f06cc6e-93a2-6a08-8002-395e36be0f5e'}}, metadata={'source': 'loop', 'step': 2, 'parents': {}, 'thread_id': '1'}, created_at='2025-07-29T21:56:42.071296+00:00', parent_config={'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '1f06cc6e-7a2f-60ea-8001-4ac26c539f8d'}}, tasks=(), interrupts=())

list(workflow.get_state_history(config1))
o/p:
[StateSnapshot(values={'topic': 'pizza', 'joke': 'Why did the pizza go to the doctor? Because it was feeling a little saucy!', 'explanation': 'This joke plays on the double meaning of the word "saucy." In one sense, "saucy" can mean bold, impertinent, or sassy. But in the context of a pizza, "saucy" refers to the tomato sauce typically used as a base on pizzas. So when the pizza went to the doctor because it was feeling "saucy," it implies that the pizza was not feeling well due to too much sauce, rather than being bold or sassy. The humor comes from the unexpected twist on the word\'s meaning.'}, next=(), config={'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '1f06cc6e-93a2-6a08-8002-395e36be0f5e'}},
StateSnapshot(values={'topic': 'pizza', 'joke': 'Why did the pizza go to the doctor? Because it was feeling a little saucy!'}, next=('generate_explanation',), config={'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '1f06cc6e-7a2f-60ea-8001-4ac26c539f8d'}}, metadata={'source': 'loop', 'step': 1, 'parents': {}, 'thread_id': '1'}, created_at='2025-07-29T21:56:39.402519+00:00', parent_config={'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '1f06cc6e-7232-6cb1-8000-f71609e6cec5'}},
 StateSnapshot(values={'topic': 'pizza'}, next=('generate_joke',), config={'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '1f06cc6e-7232-6cb1-8000-f71609e6cec5'}}, metadata={'source': 'loop', 'step': 0, 'parents': {}, 'thread_id': '1'}, created_at='2025-07-29T21:56:38.565188+00:00',
 StateSnapshot(values={}, next=('__start__',), config={'configurable': {'thread_id': '1', 'checkpoint_ns': '', 'checkpoint_id': '1f06cc6e-7230-65a8-bfff-0a96c2fc4e11'}} ]

config2 = {"configurable": {"thread_id": "2"}}
workflow.invoke({'topic':'pasta'}, config=config2)

workflow.get_state(config1)
list(workflow.get_state_history(config1))

# Benefits of Persistence

- Short-Term Memory: Persistence allows for the retention of intermediate and final state values during the execution of a workflow. This is particularly useful for debugging and understanding the flow of data, as it enables the user to refer back to previous states without losing information.

- Debugging: Persistence allows you to save the state of your workflow at various checkpoints. If something goes wrong during execution, you can replay the workflow from a specific checkpoint to identify and fix the issue.

- State Management: By saving intermediate and final values in memory, you can maintain the state of your application, which is particularly useful for complex workflows.

- Time Travel: This benefit refers to the ability to replay the execution of a workflow from a specific checkpoint. It allows users to revisit previous states and re-execute subsequent steps. This is particularly valuable for debugging, as it enables users to analyze the workflow's behavior at different points in time, helping to identify and resolve issues effectively. These benefits collectively enhance the functionality and robustness of workflows, making them more efficient and user-friendly.
- code:
    - workflow.get_state({"configurable": {"thread_id": "1", "checkpoint_id": "1f06cc6e-7232-6cb1-8000-f71609e6cec5"}})
    - workflow.invoke(None, {"configurable": {"thread_id": "1", "checkpoint_id": "1f06cc6e-7232-6cb1-8000-f71609e6cec5"}})
    - list(workflow.get_state_history(config1))

- Updating the state:
    - workflow.update_state({"configurable": {"thread_id": "1", "checkpoint_id": "1f06cc6e-7232-6cb1-8000-f71609e6cec5", "checkpoint_ns": ""}}, {'topic':'samosa'})
    - workflow.invoke(None, {"configurable": {"thread_id": "1", "checkpoint_id": "1f06cc72-ca16-6359-8001-7eea05e07dd2"}})

- Fault Tolerance: By implementing persistence, a system can recover from failures or interruptions. If a process is interrupted, the system can resume from the last saved state rather than starting over from scratch. This enhances the reliability of the workflow and ensures that progress is not lost.

- Understanding and Learning: Using checkpoints for demonstration purposes can help in understanding the workflow better, as you can visualize how different parts of the process interact. Overall, persistence enhances the robustness and reliability of workflows, especially in complex applications.'

- Human in the Loop: Persistence can facilitate the integration of human feedback into automated workflows. By saving states, the system can allow for human intervention at various points in the workflow, enabling adjustments or corrections based on human input. This can improve the overall quality and accuracy of the output.