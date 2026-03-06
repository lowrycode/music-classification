# Music Detection and Song Identification from YouTube Videos

A complete pipeline for **detecting music in YouTube videos** and **identifying individual songs by reading on‑screen lyrics**. It was built to automate tagging of church service recordings.

The project includes audio signal processing, machine learning, OCR/vision, database integration, and cloud services—all driven from Jupyter notebooks.

> ***Topics:*** *audio signal processing, machine learning, OCR, computer vision, fuzzy matching, database integration*

## 🔄 Pipeline Overview

YouTube Video  
→ Audio Extraction (ffmpeg)  
→ Feature Extraction (librosa)  
→ Music Classifier (Random Forest)  
→ Detected Music Blocks  
→ Frame Capture  
→ OCR (EasyOCR)  
→ Lyric Matching (RapidFuzz)  
→ Song Identification

## 🗂️ Project Structure

```
.
├── data/                # versioned raw, clips, and results
│   ├── v1/              # example earlier version
│   └── v2/              # current data version
├── models/              # trained classifier(s)
│   └── v2/              # model files
├── notebooks/           # Jupyter notebooks for each stage
├── requirements.txt     # Python dependencies
├── settings.py          # constants used by notebooks
├── README.md            # you are here!
└── ...
```


## 🎯 The Problems This Project Solves

### Automating Music Detection in Videos

Before this project, identifying music sections in YouTube recordings required manual listening and timestamping, which was time-consuming and error-prone. This application automates those workflows by training a machine learning model to classify audio clips as music or non-music.

The pipeline allows users to:
- Generate labeled training data from YouTube videos
- Train a classifier on audio features
- Apply the model to new videos to detect music blocks
- Visualize results with timeline plots and CSV summaries

### Song Identification via OCR

Previously, identifying songs in detected music blocks involved manual review of lyrics. This project expands those capabilities with OCR-based lyric recognition, enabling automatic matching against a lyrics database.

The pipeline allows users to:
- Extract lyrics from video frames using computer vision
- Match scraped text against song lyrics using fuzzy string matching
- Resolve song identities and timestamps
- Integrate with a database for batch processing and thumbnail generation

## 🔗 Related Repositories

- **Song Planner Dashboard (Frontend):**  
  https://github.com/lowrycode/song-planner-dashboard

- **Song Planner API (Backend):**  
  https://github.com/lowrycode/song-planner-api

## 🛠️ Tech Stack

- **Languages:** Python, Jupyter
- **Core libs:** `librosa`, `scikit-learn`, `pandas`, `NumPy`, `OpenCV`, `EasyOCR`, `RapidFuzz`, `SQLAlchemy`
- **Tools:** `ffmpeg`, `yt-dlp`
- **Database:** PostgreSQL (accessed via SQLAlchemy)
- **Cloud services:** YouTube Data API, Cloudinary (thumbnails)

## ✨ Key Features

- **Automated training data generation**
  - Download YouTube audio and split into labeled clips
  - Support for multiple videos and versioned datasets

- **Machine learning classifier**
  - Random Forest model trained on MFCCs and spectral centroids
  - Evaluation with accuracy metrics and cross-validation

- **Music block detection**
  - Sliding window prediction on new videos
  - Smoothing and gap-filling for robust block identification

- **OCR-based song identification**
  - Video frame capture and lyric extraction
  - Fuzzy matching against lyrics database with RapidFuzz

- **Batch automation**
  - YouTube API integration for video search
  - Database updates with song links and thumbnails

- **Versioned workflows**
  - Isolated data and model versions for experimentation
  - Notebook-driven process with clear staging

## 🧩 Architecture & Design Decisions

### Notebook-Driven Workflow

The project uses Jupyter notebooks for each stage to enable interactive development, debugging, and documentation. This approach allows non-technical users to follow along while maintaining reproducibility.

### Versioned Data and Models

All data (`data/vX`) and models (`models/vX`) are versioned to isolate experiments and allow safe iteration without overwriting previous work.

### Feature Extraction Pipeline

Audio features are extracted using FFmpeg for fast decoding and librosa for MFCCs and spectral analysis, ensuring efficient processing of large audio files.

### OCR and Matching Integration

EasyOCR is used for robust text recognition from video frames, combined with RapidFuzz for flexible lyric matching, handling variations in text quality and formatting.

## ⚙️ Environment & Configuration

The pipeline communicates with PostgreSQL for lyrics data and uses cloud services for thumbnail storage.

Environment variables should be configured in a `.env` file in the repository root:

```bash
# Database containing song data for matching lyrics
DB_URL=postgresql://user:pass@host/dbname

# For batch processing
YOUTUBE_API_KEY=...

# For storing thumbnails with Cloudinary
CLOUDINARY_CLOUD_NAME=...
CLOUDINARY_API_KEY=...
CLOUDINARY_API_SECRET=...
CLOUDINARY_FOLDER=...
```

Notebooks 3 and 4 are closely related and require the same `YOUTUBE_URL` so this is specified in `settings.py` for use in these notebooks.


## 🚀 Getting Started

### Prerequisites:
- Python 3.10+ (virtual environment recommended)
- `ffmpeg` and `yt-dlp` installed on PATH
- PostgreSQL instance with tables for songs, song usages, song lyrics and youtube links
- YouTube API key (optional, for batch workflow in notebook 05)
- Cloudinary account (optional, for storing thumbnails in notebook 06)

### Install dependencies:

To avoid package conflicts, install dependencies in a virtual environment.

```bash
python -m venv venv
source venv/bin/activate    # On Windows use: source .venv/Scripts/activate
pip install -r requirements.txt
```

Run the notebooks in order:
1. `01_generate_training_data.ipynb` – Generate labeled clips
2. `02_train_classifier.ipynb` – Train the model
3. `03_predict_music_blocks_using_classifier.ipynb` – Detect music blocks
4. `04_identify_songs_in_blocks.ipynb` – Identify songs via OCR
5. `05_combined_workflow.ipynb` – Batch process (includes adding links to DB)
6. `05b_generate_thumbnail.ipynb` – Generate thumbnails


## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.