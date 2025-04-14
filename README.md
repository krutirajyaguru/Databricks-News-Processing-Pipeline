# Databricks News Processing Pipeline

## Overview
This project automates the extraction, processing, and storage of news articles using Databricks. It retrieves entertainment news from the NewsAPI, processes it by extracting full content, performing sentiment analysis, and stores the results in Delta Lake.

## Project Workflow

1. **Setup & Configuration** - Install required dependencies.
2. **Data Extraction** - Fetch news articles using NewsAPI.
3. **Data Processing** - Extract full content, clean text, and perform sentiment analysis.
4. **Data Storage** - Store processed data in Delta Lake.
5. **Job Scheduling & Monitoring** - Monitor stored data and ensure smooth execution.

## Installation & Setup

```sh
pip install azure-storage-blob==12.19.1
pip install newsapi-python==0.2.7
pip install nltk==3.8.1
pip install pandas==2.2.1
pip install psycopg2-binary==2.9.9
pip install pyarrow==15.0.2
pip install SQLAlchemy==2.0.20
pip install newspaper3k
pip install lxml_html_clean==0.1.1
```

## Data Extraction

- Fetch top entertainment news headlines using `NewsAPI`.
- Extract details such as source, title, author, and publication date.
- Save extracted data to `/dbfs/tmp/extracted_news.csv`.

## Data Processing

- Use `newspaper3k` to fetch full content.
- Tokenize words and remove stopwords.
- Perform sentiment analysis using `VADER`.
- Save processed data to `/dbfs/tmp/processed_news.csv`.

## Data Storage

- Convert processed data into a structured format.
- Store the data in Delta Lake (`the_news.news_table`).

## Job Scheduling & Monitoring

- Read stored data to validate its integrity.
- Track the count of stored records for monitoring.

## Challenges & Solutions

### 1. `TypeError: get_top_headlines() got an unexpected keyword argument 'from_param'`
- Solution: Use `from_` instead of `from_param`.

### 2. `OSError: Cannot save file into a non-existent directory: '/dbfs/tmp'`
- Solution: Use `dbutils.fs.put` instead of direct file operations.

### 3. `LookupError: Resource 'punkt_tab' or 'vader_lexicon' not found.`
- Solution: Download missing NLTK resources with:
  ```python
  import nltk
  nltk.download('punkt')
  nltk.download('stopwords')
  nltk.download('vader_lexicon')
  ```

### 4. `AnalysisException: [SCHEMA_NOT_FOUND] The schema 'the_news' cannot be found.`
- Solution: Ensure schema exists or create it before inserting data.

## Future Enhancements

- **Improve Data Cleaning** - Implement advanced NLP techniques.
- **Expand Categories** - Fetch news from different categories.
- **Real-time Processing** - Integrate Kafka for real-time updates.
- **Dashboard Integration** - Visualize sentiment trends using Power BI or Tableau.



