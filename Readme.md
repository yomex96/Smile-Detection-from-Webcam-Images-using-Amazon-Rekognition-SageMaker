#Lab Details

This hands-on lab helps you build a real-time smile and emotion detection system using Amazon Rekognition, integrated within a Jupyter Notebook hosted on Amazon SageMaker. 
You’ll capture a live webcam image using Python, upload it to Amazon S3, and analyze it using Rekognition's face detection API. The results, smile status and emotions, which are displayed with annotated images and confidence scores. This beginner-friendly lab showcases how to build a serverless AI workflow using AWS services without training any custom models.

Duration: 45 Minutes
AWS Region: US East (N. Virginia) us-east-1

##Introduction

Amazon Rekognition
Amazon Rekognition is a deep learning–based image and video analysis service by AWS. It allows you to quickly detect faces, emotions, objects, and more ~ without needing to build or train machine learning models.
In this lab, Rekognition is used to detect human faces in a webcam image, identify whether a person is smiling, and determine dominant facial emotions such as Happy, Sad, Angry, Calm, etc.
Amazon S3 (Simple Storage Service)
Amazon S3 is a secure, scalable object storage service used to store and retrieve files (called objects). It acts as a cloud-based storage layer between your local application and AWS services.
In this project, S3 is used to store the webcam image captured from the user’s system using Python and OpenCV.
The image is uploaded to a dedicated S3 bucket, from where Amazon Rekognition directly accesses it for face and emotion analysis.
Amazon SageMaker
Amazon SageMaker is a fully managed cloud service that provides tools to build, train, and deploy machine learning models at scale.
Although this lab doesn’t involve model training, SageMaker is used to host a Jupyter Notebook instance where we:
Write and run the image processing and analysis code
Download images from S3
Use Rekognition to analyze them
Display annotated results using libraries like Pillow and Matplotlib
SageMaker serves as a centralized, cloud-native environment to experiment with AI workflows without local setup.
Architecture Diagram
