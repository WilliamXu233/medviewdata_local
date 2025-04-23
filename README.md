# medviewdata_local
# MedView Data Layer

This module contains the data pipeline components for the MedView project. This is a healthcare support platform that enables users to semantically search and interact with medical device documentation using language models.

---

##  Overview

The data system powers a Retrieval-Augmented Generation (RAG) pipeline by:
- Scraping manufacturer websites
- Embedding device manuals and FAQs
- Storing vectorized documents in AstraDB and MongoDB
- Enabling fast, contextual retrieval via LangChain

---

##  Repository Structure

```
 medviewdata_local/
├── mdvn.ipynb               # Vectorization and AstraDB uploader
├── scraper_medview.ipynb   # Website link crawler
├── mdvnosql.ipynb          # MongoDB FAQ uploader
├── Retrieval Model.ipynb   # RAG model pipeline
├── medviewrag_demo.txt     # Sample FreeStyle Lite manual
└── MedView Data Documentation.docx
```

---

## Key Features

###  Vector Database (AstraDB)
- Embed `.txt` documents using `OpenAIEmbeddings`
- Split into overlapping chunks
- Store and retrieve via `AstraDBVectorStore`

###  Web Scraper
- Recursive crawler that extracts internal links from manufacturer domains (e.g. ResMed, Philips)
- Normalizes and deduplicates links
- Supports retry and depth control

###  MongoDB NoSQL Upload
- Stores structured FAQs (device + list of Q&A pairs)
- JSON-style schema, easily queryable
- Atlas connectivity with connection verification

###  RAG Model (Retrieval + LLM)
- LangChain’s `RetrievalQA` integrates AstraDB retriever and OpenAI LLM
- Input query → vector search → LLM with context → Answer

---

##  Setup Instructions

1. Clone the repo:
```bash
git clone https://github.com/WilliamXu233/medviewdata_local.git
cd medviewdata_local
```

2. Install dependencies:
```bash
pip install langchain openai astrapy pymongo python-dotenv
```

3. Create a `.env` file:
```env
OPENAI_API_KEY=your_key
ASTRA_DB_API_ENDPOINT=your_endpoint
ASTRA_DB_APPLICATION_TOKEN=your_token
MONGODB_URI=your_mongodb_uri
```

---

## How to Run Each Component

###  Embed + Upload Text
```python
# In mdvn.ipynb
load_and_vectorize_text_file("medviewrag_demo.txt")
```

###  Scrape Links from Website
```python
# In scraper_medview.ipynb
scraper = AdvancedWebScraper(base_url="https://www.resmed.com")
links = scraper.get_all_links()
```

###  Insert FAQs into MongoDB
```python
# In mdvnosql.ipynb
insert_data()  # Inserts CPAP model FAQs
```

###  Query the RAG Pipeline
```python
# In Retrieval Model.ipynb
qa.run("How do I clean my ResMed device?")
```

---

##  Future Work

- Convert scripts into CLI or API services
- Support multi-language content and queries
- Expand device coverage across more brands
- Add admin dashboard for data ingestion

---

##  Contributors

- [@WilliamXu233](https://github.com/WilliamXu233)
- [@harrrisw](https://github.com/harrrisw)

---

