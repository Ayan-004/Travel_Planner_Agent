# ✈️ AI Travel Planner (Multi-Agent System)

An intelligent, self-correcting travel planning agent built with **LangGraph** and **OpenAI**. This system orchestrates multiple AI agents to plan, generate, and refine detailed travel itineraries, ensuring logical consistency and high-quality results through an autonomous critique loop.

## 🌟 Key Features

* **🧠 Multi-Agent Architecture:** Utilizes a graph-based workflow with specialized nodes for **Planning**, **Itinerary Generation**, and **Critiquing**.
* **🔄 Self-Correction Loop:** Implements a "Reflexion" mechanism where a Critic agent evaluates the itinerary. If improvements are needed, it sends feedback to the Generator for automatic revision (up to a configurable limit).
* **💾 Persistent State:** Uses `sqlite3` check-pointing to save the state of the conversation, allowing for "time-travel" debugging and context retention.
* **🖥️ Interactive UI:** Features a user-friendly **Gradio** interface to submit tasks, view real-time logs, and inspect the intermediate outputs of each agent (Plan, Itinerary, Critique).

## 🛠️ Tech Stack

* **Framework:** [LangGraph](https://github.com/langchain-ai/langgraph) (Stateful Multi-Agent Orchestration)
* **LLM:** OpenAI GPT-4o-mini (via `langchain-openai`)
* **Interface:** [Gradio](https://www.gradio.app/)
* **Database:** SQLite (for state persistence/memory)
* **Environment:** Python 3.9+

## 📋 Prerequisites

1.  **Python 3.9 or higher** installed on your machine.
2.  An **OpenAI API Key**. You can get one [here](https://platform.openai.com/).

## 🚀 Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/yourusername/ai-travel-planner.git](https://github.com/yourusername/ai-travel-planner.git)
    cd ai-travel-planner
    ```

2.  **Create and activate a virtual environment:**
    ```bash
    python -m venv venv
    # On Windows:
    venv\Scripts\activate
    # On macOS/Linux:
    source venv/bin/activate
    ```

3.  **Install dependencies:**
    ```bash
    pip install langgraph langchain-openai gradio python-dotenv
    ```

4.  **Set up environment variables:**
    Create a `.env` file in the root directory and add your API key:
    ```env
    OPENAI_API_KEY=your_sk_key_here
    ```

##  Usage

1.  **Run the application:**
    ```bash
    python main.py
    ```

2.  **Open the UI:**
    The script will launch a local web server. Open the URL provided in the terminal (usually `http://127.0.0.1:7860`) in your browser.

3.  **Generate a Plan:**
    * Go to the **"Submit Task"** tab.
    * Enter a query (e.g., *"Give me a 3-day itinerary for Tokyo with a focus on food and anime culture, budget $500"*).
    * Click **Submit**.

4.  **Monitor Progress:**
    * Switch to the **"Intermediate Stage"** tab to see the raw outputs of the Planner and Critique agents as they work.
    * Check the **"Live logs"** tab to see the backend graph events.

## 🧠 How It Works (The Graph)

The system is modeled as a state machine (Graph):

1.  **Entry Point (`planner` node):** The LLM generates a high-level outline based on your request.
2.  **Generator (`itinerary_generator` node):** Creates a detailed day-by-day itinerary based on the plan.
3.  **Critique (`critique` node):** Reviews the itinerary for budget validation, logical flow, and missing details.
4.  **Decision Edge (`should_continue`):**
    * If the critique is minor or the max revision count (2) is reached, the flow **ends**.
    * If significant issues are found, the flow cycles back to the **Generator** with specific feedback for revision.

## 📂 Project Structure

```text
.
├── main.py           # Core logic: Graph definition, Agent classes, and Gradio UI
├── .env              # Environment variables (API Keys)
└── README.md         # Project documentation
```

## 🤝 Contributing
Contributions are welcome! Please fork the repository and submit a pull request for any enhancements (e.g., adding tools for hotel booking, flight search, etc.).

## 📄 License
This project is licensed under the MIT License.
