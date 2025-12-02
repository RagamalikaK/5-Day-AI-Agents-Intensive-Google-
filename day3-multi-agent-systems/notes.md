Context Engineering: Sessions & Memory

Memory Management - Part 1 - Sessions¶
Welcome to Day 3 of the Kaggle 5-day Agents course!

In this notebook, you'll learn:

✅ What sessions are and how to use them in your agent
✅ How to build stateful agents with sessions and events
✅ How to persist sessions in a database
✅ Context management practices such as context compaction
✅ Best practices for sharing session State


Section 2: Session Management
2.1 The Problem
At their core, Large Language Models are inherently stateless. Their awareness is confined to the information you provide in a single API call. This means an agent without proper context management will react to the current prompt without considering any previous history.

❓ Why does this matter? Imagine trying to have a meaningful conversation with someone who forgets everything you've said after each sentence. That's the challenge we face with raw LLMs!

In ADK, we use Sessions for short term memory management and Memory for long term memory. In the next notebook, you'll focus on Memory.

2.2 What is a Session?
📦 Session
A session is a container for conversations. It encapsulates the conversation history in a chronological manner and also records all tool interactions and responses for a single, continuous conversation. A session is tied to a user and agent; it is not shared with other users. Similarly, a session history for an Agent is not shared with other Agents.

In ADK, a Session is comprised of two key components Events and State:

📝 Session.Events:

While a session is a container for conversations, Events are the building blocks of a conversation.

Example of Events:

User Input: A message from the user (text, audio, image, etc.)
Agent Response: The agent's reply to the user
Tool Call: The agent's decision to use an external tool or API
Tool Output: The data returned from a tool call, which the agent uses to continue its reasoning
{} Session.State:

session.state is the Agent's scratchpad, where it stores and updates dynamic details needed during the conversation. Think of it as a global {key, value} pair storage which is available to all subagents and tools.

Session state and events

2.3 How to manage sessions?
An agentic application can have multiple users and each user may have multiple sessions with the application. To manage these sessions and events, ADK offers a Session Manager and Runner.

SessionService: The storage layer

Manages creation, storage, and retrieval of session data
Different implementations for different needs (memory, database, cloud)
Runner: The orchestration layer

Manages the flow of information between user and agent
Automatically maintains conversation history
Handles the Context Engineering behind the scenes
Think of it like this:

Session = A notebook 📓
Events = Individual entries in a single page 📝
SessionService = The filing cabinet storing notebooks 🗄️
Runner = The assistant managing the conversation 🤖
2.4 Implementing Our First Stateful Agent
Let's build our first stateful agent, that can remember and have constructive conversations.

ADK offers different types of sessions suitable for different needs. As a start, we'll start with a simple Session Management option (InMemorySessionService):



Section 3: Persistent Sessions with DatabaseSessionService¶
While InMemorySessionService is great for prototyping, real-world applications need conversations to survive restarts, crashes, and deployments. Let's level up to persistent storage!

3.1 Choosing the Right SessionService
ADK provides different SessionService implementations for different needs:

Service	Use Case	Persistence	Best For
InMemorySessionService	Development & Testing	❌ Lost on restart	Quick prototypes
DatabaseSessionService	Self-managed apps	✅ Survives restarts	Small to medium apps
Agent Engine Sessions	Production on GCP	✅ Fully managed	Enterprise scale
3.2 Implementing Persistent Sessions
Let's upgrade to DatabaseSessionService using SQLite. This gives us persistence without needing a separate database server for this demo.

Let's create a chatbot_agent capable of having a conversation with the user.






Next Steps - Long Term Memory Systems (Part 2)
Why do we need memory?
In this notebook, we manually identified a couple characteristic (username and country) and built tools to manage it. But real conversations involve hundreds of such characteristics:

User preferences and habits
Past interactions and their outcomes
Domain knowledge and expertise levels
Communication styles and patterns
Contextual relationships between topics
The Memory System in ADK automates this entire process, making it a valuable asset for building truly Context-Aware Agents that can accommodate any user's current and future needs.

In the next notebook (Part 2: Memory Management), you'll learn how to:

Enable automatic memory extraction from conversations
Build agents that learn and adapt over time
Create truly personalized experiences at scale
Manage long-term knowledge across sessions
