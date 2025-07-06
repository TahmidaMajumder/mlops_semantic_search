# YouTube Video ETL Pipeline with Semantic Search

A comprehensive ETL pipeline that extracts YouTube video metadata and transcripts, processes them through automated workflows, and builds a semantic search API using FastAPI and machine learning embeddings.

## 🚀 Project Overview

This project creates an automated data pipeline that:
- Extracts video IDs, titles, and metadata from a YouTube channel
- Downloads video transcripts using YouTube's API
- Processes and cleans the data
- Generates semantic embeddings for search capabilities
- Provides a FastAPI backend for querying the data
- Runs automated updates via GitHub Actions
- Deploys using Docker and Google Cloud Platform

## 📁 Project Structure

```
.
├── .github/workflows/
│   └── data_pipeline.yml          # GitHub Actions workflow
├── app/
│   ├── main.py                    # FastAPI application
│   ├── functions.py               # API helper functions
│   └── data/
│       ├── video-ids.parquet      # Extracted video metadata
│       ├── video-transcripts.parquet  # Video transcripts
│       └── video-index.parquet    # Processed data with embeddings
├── data_pipeline/
│   ├── data_pipeline.py           # Main ETL pipeline script
│   ├── functions.py               # ETL helper functions
│   └── requirements.txt           # Pipeline dependencies
├── Dockerfile                     # Docker configuration
├── requirements.txt               # API dependencies
├── .gitignore                     # Git ignore rules
└── LICENSE                        # Project license
```

## 🛠️ Technologies & Tools

- **Data Processing**: Polars, YouTube Transcript API
- **Machine Learning**: Sentence Transformers (all-MiniLM-L6-v2)
- **Similarity Search**: scikit-learn (Manhattan Distance)
- **API Framework**: FastAPI, Uvicorn
- **Data Science**: NumPy, scikit-learn
- **Automation**: GitHub Actions
- **Containerization**: Docker
- **Cloud Platform**: Google Cloud Platform (GCP)
- **Data Storage**: Parquet files
- **Language**: Python 3.10

## 🔧 Installation

### Prerequisites
- Python 3.10+
- YouTube Data API v3 key
- Docker (optional)
- GCP account (for deployment)

### Local Setup

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd youtube-etl-pipeline
   ```

2. **Create virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   pip install -r data_pipeline/requirements.txt
   ```

4. **Set up environment variables**
   ```bash
   export YT_API_KEY="your_youtube_api_key_here"
   ```

5. **Run the ETL pipeline** (first time setup)
   ```bash
   python data_pipeline/data_pipeline.py
   ```

6. **Start the API server**
   ```bash
   uvicorn app.main:app --reload
   ```

## 🎯 Usage

### Running the ETL Pipeline

The pipeline consists of 4 main steps:

1. **Extract Video IDs**: Downloads video metadata from YouTube channel
2. **Extract Transcripts**: Retrieves video transcripts using YouTube Transcript API
3. **Transform Data**: Cleans and processes the raw data
4. **Generate Embeddings**: Creates semantic embeddings for search functionality

```bash
# Run the complete pipeline
python data_pipeline/data_pipeline.py
```

### Pipeline Functions

The ETL pipeline includes these key functions:

- `getVideoIDs()`: Extracts video metadata from YouTube channel
- `getVideoTranscripts()`: Downloads transcripts for all videos
- `transformData()`: Cleans and processes data (handles special characters, sets data types)
- `createTextEmbeddings()`: Generates semantic embeddings using SentenceTransformer

### Automated Execution

The pipeline runs automatically via GitHub Actions:
- **Daily**: Every day at 1:00 AM UTC
- **Manual**: Can be triggered manually via GitHub interface
- **On Push**: Optionally runs on code changes 

### API Usage

The FastAPI application provides a semantic search interface for YouTube videos:

**Start the API server:**
```bash
uvicorn app.main:app --reload
```

**API Endpoints:**

1. **Health Check**
   ```bash
   GET /
   ```
   Returns: `{'health_check': 'OK'}`

2. **API Information**
   ```bash
   GET /info
   ```
   Returns: API name and description

3. **Search Videos**
   ```bash
   GET /search?query=your_search_term
   ```
   Returns: Top 5 most relevant videos with titles and video IDs


**Check Locally:**
```bash
import requests
import json

url = "url/search"
query ="pca"

params ={'query' : query}
%time response = requests.get(url, params=params)

json.loads(response.text)

# Example response:
{'title': ['Principal Component Analysis (PCA) clearly explained (2015)',
  'StatQuest: PCA - Practical Tips',
  'StatQuest: PCA in R',
  'StatQuest: Principal Component Analysis (PCA), Step-by-Step',
  'PCA Eigenvalues'],
 'video_id': ['_UVHneBUBW0',
  'oRvgq966yZg',
  '0Jp4gsfOLMs',
  'FgakZw6K1QQ',
  'ccjrsxXmfnw']}
```

### Docker Deployment

The application is containerized for easy deployment:

**Build the Docker image:**
```bash
docker build -t nameoftheimage .
```

**Run the container:**
```bash
docker run -p 8080:8080 nameofthecontainer
```

**Access the API Locally:**
```bash
import requests
import json

url = "url/search"
query ="pca"

params ={'query' : query}
%time response = requests.get(url, params=params)

json.loads(response.text)

# Example response:
{'title': ['Principal Component Analysis (PCA) clearly explained (2015)',
  'StatQuest: PCA - Practical Tips',
  'StatQuest: PCA in R',
  'StatQuest: Principal Component Analysis (PCA), Step-by-Step',
  'PCA Eigenvalues'],
 'video_id': ['_UVHneBUBW0',
  'oRvgq966yZg',
  '0Jp4gsfOLMs',
  'FgakZw6K1QQ',
  'ccjrsxXmfnw']}
```
```

## 📊 Data Flow

1. YouTube API → Raw video metadata
2. YouTube Transcript API → Video transcripts
3. Data Processing→ Cleaned and formatted data
4. Sentence Transformers → Semantic embeddings (384 dimensions each for title & transcript)
5. FastAPI → Search and query interface
6. Semantic Search → Manhattan distance similarity matching
7. Docker → Containerized deployment
8. GCP → Cloud hosting

## 🔍 Semantic Search Features

The API implements advanced semantic search capabilities:

- Embedding Model: Uses all-MiniLM-L6-v2 for high-quality text embeddings
- Distance Metric: Manhattan distance for similarity calculation
- Dual Search: Searches both video titles and transcript content
- Smart Filtering: Threshold-based filtering (distance < 40) for relevant results
- Top-K Results: Returns top 5 most relevant videos
- Real-time: Fast search using pre-computed embeddings


### Search Algorithm

- Query Embedding: Converts search query to vector representation
- Distance Calculation: Computes Manhattan distance between query and all videos
- Threshold Filtering: Filters results below similarity threshold
- Ranking: Sorts by relevance score
- Top-K Selection: Returns best 5 matches with titles and video IDs

## 🔄 Automation Features

- Scheduled Updates: Daily pipeline execution
- Change Detection: Only commits when new data is available
- Error Handling: Graceful handling of missing transcripts
- Logging: Detailed timing and progress information

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📋 Configuration

### Docker Configuration
- **Base Image**: `python:3.10-slim` for optimized size
- **Working Directory**: `/code`
- **Port**: 8080 (production ready)
- **Host**: `0.0.0.0` (accepts external connections)

### Environment Variables
- `YT_API_KEY`: YouTube Data API v3 key
- `PERSONAL_ACCESS_TOKEN`: GitHub personal access token for automated commits

### File Structure in Container
```
/code/
├── app/
│   ├── main.py              # FastAPI application
│   ├── functions.py         # Search helper functions
│   └── data/
│       ├── video-ids.parquet
│       ├── video-transcripts.parquet
│       └── video-index.parquet
└── requirements.txt
```

## 🔒 Security Notes

- API keys are stored as GitHub Secrets
- Sensitive data is excluded via `.gitignore`
- Personal access tokens are used for automated commits

## 📄 License

This project is licensed under the under theApache-2.0 license. See the LICENSE file for details.

## 🚀 Deployment Options

### Local Development
```bash
# Start API server locally
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### Docker Deployment
```bash
# Build and run with Docker
docker build -t nameoftheimage
docker run -p 8080:8080 nameofthecontainer
```

### Google Cloud Platform (GCP)
The application is designed for GCP deployment:

Follow the below intruction for continious feployment from Github Repo:
![image alt](https://github.com/TahmidaMajumder/mlops_semantic_search/blob/1200b157e79defd869e8302b2ac64fe6176fab27/GCP_1.png)
![image alt](https://github.com/TahmidaMajumder/mlops_semantic_search/blob/1200b157e79defd869e8302b2ac64fe6176fab27/GCP_2.png)
![image alt](https://github.com/TahmidaMajumder/mlops_semantic_search/blob/1200b157e79defd869e8302b2ac64fe6176fab27/GCP_3.png)
![image alt](https://github.com/TahmidaMajumder/mlops_semantic_search/blob/1200b157e79defd869e8302b2ac64fe6176fab27/GCP_4.png)
![image alt](https://github.com/TahmidaMajumder/mlops_semantic_search/blob/1200b157e79defd869e8302b2ac64fe6176fab27/GCP_5.png)

**GCP deployment Check:**
Use the url to check
![image alt](https://github.com/TahmidaMajumder/mlops_semantic_search/blob/1200b157e79defd869e8302b2ac64fe6176fab27/GCP_6.png)

ADD search query "pca" with url

![image alt](https://github.com/TahmidaMajumder/mlops_semantic_search/blob/1200b157e79defd869e8302b2ac64fe6176fab27/GCP_7.png)


## 🚧 Status

**Current Status**: ✅ Complete implementation
- ✅ ETL Pipeline with GitHub Actions automation
- ✅ Semantic Search API with FastAPI
- ✅ Docker containerization
- ✅ GCP deployment ready

**Features Implemented**:
- Automated daily data extraction from YouTube
- Transcript processing and text cleaning
- 384-dimensional semantic embeddings
- Manhattan distance similarity search
- RESTful API with health checks
- Dockerized deployment
- Production-ready configuration

---

*Project is complete and ready for deployment! 🎉*
