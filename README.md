# 🎵 Microservices MP4 -> MP3 Converter

This application converts video files to MP3 format using a scalable and modular microservices architecture.

---

## 🧭 Overview

This is a hands-on learning project that demonstrates how to build a video-to-MP3 converter using microservices. It simulates a scalable, asynchronous system using modern DevOps tools and architectural patterns such as message queues, JWT authentication, and containerization with Docker.

---

## ⚙️ System Components

| Component                | Description                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| **API Gateway**          | Handles user requests, authentication, video upload, and MP3 download       |
| **Auth Service**         | Authenticates users and issues JWT tokens                                   |
| **Converter Service**    | Listens for jobs, converts video to MP3, and stores result in MongoDB       |
| **Notification Service** | Sends emails when the MP3 file is ready for download                        |
| **MongoDB**              | Stores uploaded videos, MP3s, and metadata                                   |
| **RabbitMQ**             | Manages asynchronous communication between services                         |

---

## 🔐 Authentication Flow

1. **JWT Request**:  
   Users first authenticate by sending login credentials to the **API Gateway**, which forwards the request to the **Auth Service**. A JWT (JSON Web Token) is issued on successful login.

2. **JWT Usage**:  
   This token must be included in all subsequent requests, including uploading videos and downloading MP3 files.

---

## 📽️ Video Upload Workflow

1. **Upload**:  
   Users send video files to the API Gateway (with JWT). The video is stored in MongoDB.

2. **Queue Job**:  
   The API Gateway publishes a message to RabbitMQ to signal that a new video is ready for conversion.

---

## 🛠️ Video-to-MP3 Conversion Workflow

1. **Job Handling**:  
   The **Converter Service** listens for RabbitMQ messages. Once a job is received, it fetches the video from MongoDB.

2. **Processing**:  
   The video is converted into MP3 format.

3. **Storage**:  
   The MP3 file is saved back to MongoDB.

4. **Notification Trigger**:  
   A new message is sent to RabbitMQ indicating that the MP3 file is ready.

---

## ✉️ Notification Workflow

1. **Listen for Updates**:  
   The **Notification Service** monitors RabbitMQ for conversion-complete messages.

2. **Send Email**:  
   It sends an email notification to the user informing them that their MP3 is ready for download.

---

## 🎧 MP3 Download Flow

1. **Secure Request**:  
   Users use their **JWT** and the **file ID** (from the email) to request the MP3 from the **API Gateway**.

2. **Authorization**:  
   The gateway verifies the token and retrieves the file from MongoDB.

3. **Download**:  
   The MP3 file is served to the user.

---

## 🛠️ Tech Stack

- **Python** (Flask/FastAPI)
- **RabbitMQ** – message broker
- **MongoDB** – database for videos and MP3s
- **MySQL** – optional metadata storage
- **Docker & Docker Compose**
- **JWT** – authentication
- **SMTP** – email service

---

## 📌 Future Improvements

- Add UI frontend
- Implement user registration
- Rate limiting and upload size checks
- Deploy to Kubernetes (Helm chart support)
