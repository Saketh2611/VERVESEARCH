# VerveSearch: AI-Powered Sports Recovery Search Engine

VerveSearch is a smart search engine built with Streamlit that helps athletes, coaches, and fitness enthusiasts find relevant discussions on Reddit about sports recovery, injuries, fatigue, and performance. It uses semantic search to understand the user's query and finds the most relevant posts, then leverages a generative AI model to provide a concise, expert-like summary.

## ✨ Key Features

* **🧠 Semantic Search**: Understands the *meaning* behind your query (e.g., "my ankle hurts after a run") to find the most relevant content, not just keywords.
* **📚 Reddit Data Source**: Scrapes data from popular sports and fitness subreddits like `r/running`, `r/Fitness`, `r/triathlon`, and more.
* **🤖 AI-Powered Suggestions**: Uses Google's Gemini model to generate a helpful summary and response based on the top search result.
* **⚡ Vector Database**: Powered by **Qdrant Cloud** for efficient and scalable similarity search.
* **👍 User Feedback Loop**: Captures user feedback on search results (Relevant/Not Relevant) and stores it in a PostgreSQL database for future model improvement.
* **🔄 On-Demand Data Refresh**: A simple button in the UI allows you to fetch the latest posts from Reddit.
* **📂 Keyword Filtering**: Refine your search results by selecting specific keywords like "injury," "recovery," or "motivation."

---

## 🛠️ Tech Stack

* **Frontend**: [Streamlit](https://streamlit.io/)
* **Machine Learning**:
    * **Embeddings**: `sentence-transformers`
    * **Generative AI**: `google-generativeai` (Gemini 1.5 Flash)
* **Database**:
    * **Vector DB**: [Qdrant Cloud](https://qdrant.tech/)
    * **Relational DB**: [PostgreSQL](https://www.postgresql.org/) (for feedback)
* **Web Scraping**: `asyncpraw` (Asynchronous Python Reddit API Wrapper)
* **Core Language**: Python

---

## ⚙️ Setup and Installation

Follow these steps to get the project running on your local machine.

### 1. Prerequisites

* Python 3.8+
* Git
* A running PostgreSQL instance.

### 2. Clone the Repository

```bash
git clone [https://github.com/your-username/VerveSearch.git](https://github.com/your-username/VerveSearch.git)
cd VerveSearch
```
### 3. Set up a Virtual Environment

It's highly recommended to use a virtual environment to manage project dependencies.

```bash
# For Unix/macOS
python3 -m venv venv
source venv/bin/activate

# For Windows
python -m venv venv
.\venv\Scripts\activate
```

### 4. Install Dependencies

Create a `requirements.txt` file with the following content:

**`requirements.txt`**
```
streamlit
pandas
psycopg2-binary
sentence-transformers
qdrant-client
asyncpraw
google-generativeai
```

Then, install the packages:

```bash
pip install -r requirements.txt
```

### 5. Configure Credentials

This project requires several API keys and database credentials to function. You will need to add them directly into the Python script.

1.  Open the main Python application file (e.g., `your_app_file.py`).
2.  Locate the sections where the clients (Qdrant, Gemini, PostgreSQL, Reddit) are initialized.
3.  Replace the placeholder values with your actual credentials.

**Example:**

```python
# In your Python script, find and replace these values:

# Qdrant Connection
client = QdrantClient(
    url="YOUR_QDRANT_URL_HERE", 
    api_key="YOUR_QDRANT_API_KEY_HERE"
)

# Gemini API Key
genai.configure(api_key="YOUR_GEMINI_API_KEY_HERE")

# PostgreSQL Connection
conn = psycopg2.connect(
    host="YOUR_DB_HOST",
    dbname="YOUR_DB_NAME",
    user="YOUR_DB_USER",
    password="YOUR_DB_PASSWORD",
    port="YOUR_DB_PORT"
)

# Reddit Scraper
reddit = asyncpraw.Reddit(
    client_id="YOUR_REDDIT_CLIENT_ID",
    client_secret="YOUR_REDDIT_CLIENT_SECRET",
    password="YOUR_REDDIT_PASSWORD",
    user_agent="VerveSearch by u/your-username",
    username="YOUR_REDDIT_USERNAME",
)
```
**Note on Security:** Hardcoding credentials directly in the source code is not recommended for production environments. For deployment, you should use environment variables or a secrets management tool to keep your keys safe.

---

### 6. Initialize Databases

* **PostgreSQL**: The application will automatically create the `feedback` table on the first run.
* **Qdrant**: You must create the collection manually in your Qdrant dashboard or via a script. The collection must be named `vervesearch_reddit` and configured with a vector size of **768** (the output dimension of the `paraphrase-mpnet-base-v2` model).

---

## 🚀 How to Use

### 1. Run the Streamlit App

Open your terminal, activate your virtual environment, and run:

```bash
streamlit run your_app_file.py
```

### 2. Populate the Database

The first time you run the app, the Qdrant database will be empty.
* Open the web app in your browser (usually `http://localhost:8501`).
* Click the **"🔄 Refresh Data from Reddit"** button in the sidebar.
* This will trigger the scraper to fetch posts, generate embeddings, and save them to your Qdrant collection.

### 3. Search for Information

* In the main search box, type your query (e.g., "shin splints from running too much").
* Optionally, use the multi-select box to filter results by keywords.
* The top 5 most semantically similar posts from Reddit will be displayed.
* For any result, click the **"✨ Get AI Suggestion"** button to get a summarized, actionable response.

---
