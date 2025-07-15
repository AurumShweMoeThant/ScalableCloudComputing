# Scalable Cloud-Based Sentiment Analysis for Amazon Electronics Reviews

## Overview
This project implements a scalable, cloud-native pipeline for sentiment analysis of Amazon Electronics reviews using Amazon Web Services (AWS). The system leverages distributed processing to handle large-scale datasets, performing both batch and real-time analysis. The pipeline processes millions of reviews stored in JSON format, extracts meaningful insights through keyword extraction and sentiment classification, and visualizes results efficiently.

The project was developed as part of a continuous assessment by **Oluseyi Iyetomide Simon Coker** and **Shwe Moe Thant** at the National College of Ireland.

## Features
- **Cloud-Native Architecture**: Utilizes AWS services (S3, EMR, EC2, Kinesis) for scalable data storage, processing, and real-time analytics.
- **Data Ingestion**: Processes raw JSON data into a cleaned Parquet format, reducing dataset size from 11 GB to 3.3 GB for efficient processing.
- **Batch Processing**: Implements MapReduce jobs using PySpark on Amazon EMR to analyze dataset fractions (20%, 40%, 60%, 80%, 100%) and visualize top keywords and sentiment distributions.
- **Real-Time Analytics**: Streams data through Amazon Kinesis for near real-time sentiment scoring and keyword tracking.
- **Hybrid Parallelism**: Combines Spark-based data parallelism with task parallelism using Python's `ThreadPoolExecutor` for optimized performance.
- **Performance Monitoring**: Tracks metrics like CPU utilization, memory usage, and processing latency to ensure efficient resource use.
- **Scalability**: Designed to support auto-scaling with Amazon CloudWatch for dynamic resource allocation in production environments.

## Repository Structure
- **`data_ingestion.py`**: PySpark script for ingesting and cleaning raw JSON data from S3, converting it to Parquet format.
- **`20_percent.py`, `40_percent.py`, `60_percent.py`, `80_percent.py`, `100_percent.py`**: PySpark scripts for MapReduce-based batch processing of dataset fractions.
- **`stream_processing.py`**: Script for real-time sentiment analysis and keyword tracking using Kinesis and EC2.
- **`convert_mapreduce_to_json.py`**: Converts processed Parquet data to newline-delimited JSON for streaming.
- **`read_json_to_kinesis.py`**: Feeds JSON data into Kinesis for real-time processing.
- **`hybrid_parallelism.py`**: Implements hybrid parallelism for concurrent task execution.
- **Bootstrap Script**: Installs required Python packages (`boto3`, `pandas`, `textblob`, `nltk`, `plotly`, `kaleido`) and sets up Google Chrome for Plotly visualizations on EMR.
- **Visualizations**: Includes bar charts for top 10 keywords and pie charts for sentiment distribution, stored in S3.

## AWS Architecture
The pipeline uses the following AWS services:
- **Amazon S3**: Stores raw and processed data (electronics.json and Parquet files).
- **Amazon EMR**: Runs PySpark jobs for distributed batch processing with Apache Spark and Hadoop YARN.
- **Amazon EC2**: Hosts lightweight T3 instances for real-time stream processing.
- **Amazon Kinesis**: Enables real-time data streaming and analytics.
- **Amazon CloudWatch** (future scope): Monitors metrics for auto-scaling.

The architecture diagram (refer to Fig. 1 in the report) illustrates data flow from S3 ingestion to EMR processing and Kinesis streaming.

## Key Results
- **Data Reduction**: Preprocessing reduced dataset size by ~70%, from 11 GB to 3.3 GB.
- **Sentiment Analysis**: Over 60% of reviews were positive, 25-30% neutral, and 10-15% negative, reflecting a positive bias in Amazon reviews.
- **Keyword Trends**: Common positive keywords include "great", "good", "excellent"; negative keywords include "broken", "waste", "return".
- **Performance**: Parallel execution improved CPU utilization (70-80% vs. 15% in sequential mode) and maintained stable throughput (4000-5600 records/sec).

## Setup and Installation
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/AurumShweMoeThant/ScalableCloudComputing.git
   cd ScalableCloudComputing
   ```
2. **AWS Configuration**:
   - Set up an AWS account and configure `boto3` with your credentials.
   - Create an S3 bucket (`electronics-reviews`) and upload the raw dataset (`electronics.json`).
   - Provision an EMR cluster with Apache Spark and Hadoop YARN.
   - Launch an EC2 T3 instance for stream processing.
   - Configure a Kinesis stream for real-time analytics.
3. **Install Dependencies**:
   - Use the provided bootstrap script to install Python packages on EMR (`boto3`, `pandas`, `textblob`, `nltk`, `plotly`, `kaleido`).
   - Ensure Google Chrome is installed for Plotly visualization rendering.
4. **Run the Pipeline**:
   - Execute `data_ingestion.py` to preprocess the dataset.
   - Run batch processing scripts (`20_percent.py`, etc.) on EMR.
   - Use `convert_mapreduce_to_json.py` and `read_json_to_kinesis.py` to set up streaming.
   - Launch `stream_processing.py` on EC2 for real-time analytics.

## Usage
1. **Batch Processing**:
   - Run `data_ingestion.py` to clean and store data in S3.
   - Execute the percentage-based scripts to analyze different dataset sizes and generate visualizations.
2. **Real-Time Processing**:
   - Stream data through Kinesis using `read_json_to_kinesis.py`.
   - Monitor live sentiment scores and trending keywords with `stream_processing.py`.
3. **Performance Analysis**:
   - Use `hybrid_parallelism.py` to compare sequential vs. parallel execution.
   - Monitor resource usage (CPU, memory) with the integrated `psutil` library.

## Demo
Watch the [Demo Video Presentation](https://studentncirl-my.sharepoint.com/:v:/g/personal/x24170399_student_ncirl_ie/EQWRqPC7jR9AsY4mKpSbfbMBIiOCrvPWsVteb-xgO2Tf4Q?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D&e=vpaito) for a walkthrough of the pipeline and results.

## Authors
- **Oluseyi Iyetomide Simon Coker** (x23370751@student.ncirl.ie)
- **Shwe Moe Thant**
