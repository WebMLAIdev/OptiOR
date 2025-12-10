# OptiOR - Operating Room Analytics & Prediction

OptiOR is a web-based application designed to help hospitals analyze operating room (OR) utilization and predict surgery durations. It provides an interactive dashboard for visualizing historical data and a machine learning tool to estimate future case times.

The application consists of a Flask backend that serves data and a Streamlit frontend that provides a user-friendly interface for interaction.

---

## Features

- **Interactive Dashboard**: View daily OR schedules, case distribution by surgical specialty, and average surgery durations.
- **Surgery Duration Prediction**: Uses a machine learning model (Random Forest Regressor) to predict the duration of a new surgery based on its characteristics.
- **On-Demand Model Retraining**: Trigger a retraining of the ML model on the latest data directly from the UI.
- **Automated Data Seeding**: Initializes the database from a sample CSV file.
- **RESTful API**: A clean backend API to manage data and predictions.

---

## Project Layout

```
OptiOR/
├── data/                  # CSV input files for seeding
│   └── 2022_Q1_OR_Utilization.csv
├── database/              # DB configuration and SQLAlchemy schema
│   ├── config.py
│   ├── schema.py
│   └── or_database.db     # SQLite database file
├── models/                # Stores the trained machine learning model
│   └── OptiOR.joblib
├── notebooks/             # Jupyter notebooks for analysis
├── client.py              # Streamlit frontend application
├── server.py              # Flask backend server
├── requirements.txt       # Project dependencies
└── README.md
```

---

## Running OptiOR

Follow these steps to get the application running locally.

### 1. Install Dependencies

First, install the required Python packages from `requirements.txt`:

```bash
pip install -r requirements.txt
```

### 2. Start the Backend Server

The backend is a Flask application. Run the following command in your terminal:

```bash
python server.py
```

The backend API will be available at `http://127.0.0.1:5000`.

### 3. Seed the Database (One-Time Step)

Before using the application for the first time, you must populate the database with the sample data. The server must be running.

Open a **new terminal** and run the following `curl` command to send a request to the seeding endpoint:

```bash
curl -X POST http://127.0.0.1:5000/api/seed
```

This will load the data from `data/2022_Q1_OR_Utilization.csv` into the database and train the initial machine learning model. You should see a success message in the response.

### 4. Start the Frontend Application

With the backend still running, open a **third terminal** and start the Streamlit frontend:

```bash
streamlit run client.py
```

Your web application will open in your browser, and you can access it at:

```
http://localhost:8501
```

---

## How to Use the Application

- **Dashboard**: Open the application to view the main dashboard. Use the date picker to explore the OR schedule for different days.
- **Predict Duration**: Navigate to the "Predict Duration" page from the sidebar to get an estimated surgery time.
- **Retrain Model**: Use the "Retrain Model" button in the sidebar to update the prediction model with the latest data from the database.

---

## API Endpoints

The Flask server provides the following API endpoints:

- `POST /api/seed`: Seeds the database from the CSV file.
- `DELETE /api/clear`: Clears all records from the database.
- `GET /api/schedule`: Retrieves scheduled cases for a specified date.
- `GET /api/analytics`: Provides aggregate data for dashboard charts.
- `POST /api/predict`: Predicts surgery duration for a new case.
- `POST /api/retrain`: Retrains the machine learning model.