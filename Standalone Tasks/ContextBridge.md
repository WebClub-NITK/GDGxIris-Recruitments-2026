# Task ID: Context Bridge

#### `Manifest V3 Extensions`, `Real-Time DOM`, `Graph Algorithms`, `Full-Stack DevOps`

Mentors: Devansh Sharma

Difficulty: `Medium-Hard / Hard`

---

## Project Overview: What is Context Bridge?

**Context Bridge** is a comprehensive, full-stack browser extension project designed to evaluate engineering candidates for the Web Enthusiasts' Club. It bridges the gap between client-side DOM manipulation, complex algorithmic state management, and modern containerized backend deployments.

Modern LLMs (like ChatGPT or Claude) allow users to edit prompts and regenerate responses, inherently transforming the conversation from a flat timeline into a Directed Acyclic Graph (DAG) of alternate conversational realities. Context Bridge is a tool that silently observes these branching chats, visualizes the DAG in real-time, auto-prunes the tree to fit strict token limits, and synchronizes the state to a containerized cloud database.

---

## What Should the Final Output Look and Act Like?

The architecture is split into three distinct interactive environments:

1. **The Live Observer (Content Script):**
   - Runs invisibly on the target chat interface.
   - Constantly watches the DOM for new messages or branch forks.
   - Emits real-time state updates to the extension's background worker without lagging the browser.

2. **The Visual Control Panel (Extension Popup):**
   - Opens an interactive, pan-and-zoom node graph (similar to a mind map) rendering the conversation branches.
   - Features a strict Token Budget Slider (e.g., 2048 tokens).
   - Dynamically highlights which nodes are safely included in the export and which are pruned by the algorithm.
   - Includes a **History/Archive Tab** allowing users to fetch and view previously saved cloud contexts from the backend database.
   - Features two primary action buttons: `[ 📋 Export Pruned Path to Clipboard ]` and `[ ☁️ Save Context to Cloud ]`.

3. **The Cloud Backend (Node/Express + PostgreSQL):**
   - A remote server that receives the serialized DAG payload.
   - Saves the complex hierarchical tree into a relational database.
   - Provides a REST or GraphQL endpoint for the extension to fetch and list previously saved context graphs.

---

## Core Features & Engineering Challenges (Mandatory)

### 1. Frontend Architecture: Real-Time State Sync (MutationObserver)
- Instead of parsing the DOM only when the user clicks the extension icon, the extension must build the tree in real-time.
- **The Constraint:** Candidates must utilize a `MutationObserver` inside the Content Script to watch the webpage for dynamic message injections or DOM reflows.
- **Performance Requirement:** MutationObserver events must be properly debounced (e.g., 150-250ms). The script must only extract diffs or efficiently reconstruct the graph without causing frame drops, passing the payload to the Service Worker via `chrome.runtime.sendMessage`.

### 2. The Algorithmic Challenge: Token-Aware Auto-Pruning
- Exporting a massive chat history often breaks the context window limits of target LLMs. The extension must auto-optimize the payload.
- **Weight Calculation:** Candidates must implement a fast word-count or pseudo-token estimator to assign a "weight" to every node in the graph.
- **Optimization Logic:** Similar to a greedy algorithm or the Knapsack problem, the system must traverse the DAG and intelligently prune the graph to stay under the user's defined "Max Token Limit".
- **Lineage Preservation:** The algorithm must prioritize keeping the root prompts, system instructions, and the most recent branch leaves, pruning non-essential intermediate assistant turns first.

### 3. UI/UX: Interactive Canvas Graph Rendering
- A standard HTML list cannot effectively display multidimensional branches.
- **Visual Engineering:** The React (or Vanilla JS) popup must integrate a visualization library like D3.js, React Flow, or vis.js.
- **Interactivity:** The user must be able to pan, zoom, and manually click individual nodes on the canvas to override the pruning algorithm (force-including or force-excluding specific nodes).

### 4. Full-Stack Sync: API, Prisma, and Docker
- To bridge extension development with standard MERN/Node architectures, the localized context must be persistable.
- **CORS & API Communication:** The extension must securely `POST` the serialized graph payload to a custom Node.js/Express backend.
- **Relational Schema:** The backend must utilize Prisma to define the database schema. Candidates must figure out how to efficiently store hierarchical data (e.g., using Adjacency Lists where each node stores a `parentId`).
- **DevOps Deployment:** The submission must include a `Dockerfile` and `docker-compose.yml`. Running `docker-compose up` must seamlessly provision the Node server and the PostgreSQL database simultaneously.

---

## Technical Specifications & Edge Cases

*   **Manifest V3 Ephemeral State:** Background Service Workers in MV3 terminate when idle. The DAG state cannot be stored in a global variable in `service_worker.js`. It must be hydrated and dehydrated using `chrome.storage.local`.
*   **Cyclic Prevention:** When serializing the DAG to send via JSON (either to the popup or the backend), candidates must ensure there are no circular object references (e.g., a child pointing to a parent that points back to the child), which will instantly crash the JSON stringifier.
*   **Graceful Degradation:** If the target website's DOM structure changes unexpectedly, the parser should catch the error and display a clean "Sync Failed" UI in the popup rather than crashing the extension silently.

---

## Technical Evaluation Rubric

| Focus Area | Grading Criteria |
| :--- | :--- |
| **Algorithmic Complexity** | Accuracy of the tree traversal; efficiency of the token-aware greedy pruning algorithm; edge-case handling for massive chat trees. |
| **DOM & Extension APIs** | Clean implementation of debounced MutationObservers; correct Manifest V3 message passing; effective use of chrome.storage. |
| **Full-Stack & Database** | Efficient Prisma schema design for hierarchical data; secure API routes; clean relational queries avoiding the N+1 problem. |
| **DevOps & Containerization** | A flawless, reproducible `docker-compose.yml` setup; proper environment variable mapping; optimized container image sizes. |
| **UI Rendering** | Smooth canvas/SVG rendering in the popup; intuitive pan/zoom handling; clear visual feedback for pruned vs. active nodes. |

---

## Useful Technical Resources

- **Algorithms & Optimization**
  - [Greedy Algorithms & The Knapsack Problem](https://www.geeksforgeeks.org/0-1-knapsack-problem-dp-10/)
  - [Topological Sorting in Directed Acyclic Graphs](https://www.geeksforgeeks.org/topological-sorting/)
- **Frontend & Extensions**
  - [MDN Web Docs: MutationObserver Best Practices](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver)
  - [React Flow Documentation](https://reactflow.dev/)
- **Backend & DevOps**
  - [Prisma Docs: Working with Hierarchical / Self-Relation Data](https://www.prisma.io/docs/orm/prisma-schema/data-model/relations/self-relations)
  - [Docker: Compose File Reference](https://docs.docker.com/compose/compose-file/)
