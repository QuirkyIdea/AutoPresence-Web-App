# AutoPresence-Web-App
AttendAI is an AI-powered smart attendance platform that uses facial recognition, live location verification, and automated classroom scanning to mark attendance instantly and securely through mobile and web applications.
# 🚀 AttendAI — AI-Powered Smart Attendance System

## 📌 Overview

AttendAI is a modern AI-based attendance management platform that automates classroom attendance using:

* Facial recognition
* GPS/geofencing validation
* Teacher-side automatic scanning
* Mobile applications for students
* Web dashboard for teachers and administrators
* Real-time attendance processing
* Push notifications and alerts

The platform is designed for schools, colleges, universities, coaching centers, and enterprise training programs.

---

# Vision

The goal of AttendAI is to eliminate manual attendance systems and prevent attendance fraud by combining:

1. Artificial Intelligence
2. Computer Vision
3. Geolocation Validation
4. Real-Time Processing
5. Secure Authentication

---

# ✨ Core Features

## Student Features

### Authentication

* Student registration
* Secure login
* Email/phone verification
* Password reset
* JWT authentication

### Face Enrollment

* Register face using mobile camera
* Capture multiple angles
* Generate facial embeddings
* Store encrypted biometric vectors

### GPS Verification

* Detect current student location
* Verify student is inside classroom radius
* Prevent remote attendance fraud

### Attendance Dashboard

* View attendance history
* View subject-wise attendance
* Attendance percentage analytics
* Download attendance reports

### Notifications

* Attendance marked successfully
* Absent alerts
* Low attendance warning
* Class reminders

---

## Teacher Features

### Teacher Authentication

* Secure teacher login
* Role-based access

### Attendance Session

* Start attendance session
* End attendance session
* Select classroom and subject

### Webcam Attendance

* Use laptop webcam
* Detect multiple students simultaneously
* Automatically recognize faces
* Mark attendance instantly

### Manual Controls

* Manual attendance override
* Edit attendance
* Mark late entry
* Add remarks

### Reports

* Daily attendance reports
* Monthly reports
* Export CSV/PDF
* Subject-wise analytics

---

## Admin Features

### User Management

* Add students
* Add teachers
* Add departments
* Assign subjects

### Classroom Management

* Create classrooms
* Configure geofencing radius
* Assign classrooms to subjects

### Analytics

* Attendance trends
* Student performance insights
* Fraud detection logs

### System Configuration

* Notification settings
* AI confidence threshold
* Security settings

---

# 🛠 Technology Stack

## Mobile Application

Recommended:

### Flutter

Reason:

* Single codebase for Android and iOS
* Fast development
* Excellent camera support
* Good performance

Alternative:

* React Native

---

## Frontend Web Dashboard

### React

Recommended Libraries:

* React Router
* Redux Toolkit
* Axios
* Material UI or Tailwind CSS
* Recharts

---

## Backend API

### Node.js + Express

Responsibilities:

* Authentication
* API management
* Attendance processing
* Notification handling
* Database communication

Alternative:

* NestJS

---

## AI Recognition Service

### Python + FastAPI

Responsibilities:

* Face detection
* Face recognition
* Anti-spoofing
* Face embedding generation

Libraries:

* OpenCV
* InsightFace
* RetinaFace
* NumPy
* TensorFlow/PyTorch

---

## Database

### PostgreSQL

Stores:

* Users
* Attendance records
* Class data
* Session logs

Alternative:

* MongoDB

---

## Cloud Infrastructure

Recommended:

* AWS
* Google Cloud Platform
* Azure

Services:

* EC2 / Compute Engine
* S3 / Cloud Storage
* RDS / Cloud SQL
* Cloud Functions

---

# 🏗 System Architecture

## High-Level Flow

```text
Student Mobile App
        ↓
REST API Gateway
        ↓
Backend Server
        ↓
AI Recognition Service
        ↓
Database

Teacher Dashboard
        ↓
Webcam Stream
        ↓
Face Recognition Engine
        ↓
Attendance Processing
```

---

# Folder Structure

## Backend Structure

```text
backend/
├── src/
│   ├── controllers/
│   ├── routes/
│   ├── middleware/
│   ├── services/
│   ├── models/
│   ├── utils/
│   ├── config/
│   └── app.js
├── package.json
└── .env
```

---

## AI Service Structure

```text
ai-service/
├── app/
│   ├── recognition/
│   ├── embeddings/
│   ├── anti_spoof/
│   ├── detection/
│   └── api/
├── models/
├── requirements.txt
└── main.py
```

---

## Flutter App Structure

```text
mobile/
├── lib/
│   ├── screens/
│   ├── services/
│   ├── widgets/
│   ├── models/
│   ├── providers/
│   └── main.dart
├── assets/
└── pubspec.yaml
```

---

## React Dashboard Structure

```text
dashboard/
├── src/
│   ├── pages/
│   ├── components/
│   ├── services/
│   ├── redux/
│   ├── hooks/
│   └── App.js
├── public/
└── package.json
```

---

# Database Design

## Users Table

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY,
    full_name VARCHAR(255),
    email VARCHAR(255) UNIQUE,
    phone VARCHAR(20),
    password_hash TEXT,
    role VARCHAR(20),
    created_at TIMESTAMP
);
```

---

## Students Table

```sql
CREATE TABLE students (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    roll_number VARCHAR(50),
    department VARCHAR(100),
    semester INTEGER,
    face_embedding TEXT
);
```

---

## Teachers Table

```sql
CREATE TABLE teachers (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    employee_id VARCHAR(50)
);
```

---

## Classrooms Table

```sql
CREATE TABLE classrooms (
    id UUID PRIMARY KEY,
    room_name VARCHAR(100),
    latitude DOUBLE PRECISION,
    longitude DOUBLE PRECISION,
    radius INTEGER
);
```

---

## Attendance Table

```sql
CREATE TABLE attendance (
    id UUID PRIMARY KEY,
    student_id UUID REFERENCES students(id),
    class_id UUID,
    subject_id UUID,
    timestamp TIMESTAMP,
    confidence_score FLOAT,
    gps_verified BOOLEAN,
    face_verified BOOLEAN
);
```

---

# 🤖 Facial Recognition System

## Step 1 — Face Enrollment

### Flow

1. Student opens app
2. Student captures selfie
3. App sends image to AI service
4. AI extracts facial embedding
5. Embedding stored in database

### Implementation

#### Face Detection

Use:

* RetinaFace
* MediaPipe

#### Face Embedding

Use:

* InsightFace
* FaceNet

### Why Embeddings?

Embeddings are numerical vectors representing facial features.

Example:

```text
[0.134, -0.442, 0.938, ...]
```

The actual image should not be stored as the main verification method.

---

# Attendance Recognition Flow

## Teacher Attendance Session

### Process

1. Teacher starts attendance
2. Webcam stream activates
3. Faces detected continuously
4. Embeddings generated
5. Compared with registered embeddings
6. Match found
7. Attendance marked
8. Student receives notification

---

## Face Matching Logic

### Similarity Check

Use cosine similarity.

### Confidence Threshold

Example:

```text
0.75 = minimum confidence
```

If confidence > threshold:

```text
Attendance = Verified
```

---

# 📍 GPS Verification System

## Objective

Prevent fake attendance from outside campus.

---

## Geofencing Logic

### Flow

1. App gets device GPS
2. Sends coordinates to backend
3. Backend calculates distance
4. If inside allowed radius:

   * GPS verified
5. Else:

   * Attendance rejected

---

## Distance Formula

Use Haversine Formula.

---

## Example

```text
Classroom Radius = 50 meters
```

If student is within 50 meters:

```text
Allow attendance
```

Else:

```text
Reject attendance
```

---

# Anti-Spoofing Protection

## Problem

Students may try:

* Photos
* Videos
* Fake cameras
* Printed images

---

## Solutions

### Liveness Detection

Check:

* Eye blink
* Head movement
* Facial depth
* Real-time motion

---

## Recommended Tools

### SilentFace

For anti-spoofing.

### MediaPipe Face Mesh

For eye and movement detection.

---

# 📱 Mobile App Development

# Step-by-Step Flutter Development

## Step 1 — Setup Flutter

Install:

* Flutter SDK
* Android Studio
* VS Code
* Emulator

---

## Step 2 — Create Project

```bash
flutter create attendai_mobile
```

---

## Step 3 — Install Packages

Recommended Packages:

```yaml
camera:
geolocator:
http:
provider:
flutter_secure_storage:
firebase_messaging:
```

---

## Step 4 — Authentication Screens

Build:

* Login screen
* Signup screen
* OTP verification

---

## Step 5 — Camera Integration

Features:

* Face capture
* Real-time preview
* Camera permissions

---

## Step 6 — GPS Integration

Features:

* Get current location
* Request permissions
* Send coordinates

---

## Step 7 — Notifications

Use Firebase Cloud Messaging.

---

# 💻 Teacher Dashboard Development

## Step 1 — Setup React

```bash
npx create-react-app dashboard
```

---

## Step 2 — Install Dependencies

```bash
npm install axios react-router-dom
```

---

## Step 3 — Create Pages

Pages:

* Login
* Dashboard
* Attendance Session
* Reports
* Students
* Analytics

---

## Step 4 — Webcam Integration

Use:

```text
react-webcam
```

---

## Step 5 — Real-Time Attendance

Use:

* WebSockets
* Socket.IO

---

# ⚙️ Backend Development

# Step-by-Step Backend Setup

## Step 1 — Initialize Project

```bash
npm init -y
```

---

## Step 2 — Install Dependencies

```bash
npm install express cors dotenv bcrypt jsonwebtoken
```

---

## Step 3 — Setup Express Server

Create:

* API routes
* Middleware
* Authentication

---

## Step 4 — Create APIs

Required APIs:

### Authentication APIs

* Register
* Login
* Refresh token

### Student APIs

* Upload face
* Get profile
* Attendance history

### Attendance APIs

* Start session
* Mark attendance
* End session

### Teacher APIs

* Reports
* Student management

---

# 🧠 AI Service Development

# Step-by-Step AI Setup

## Step 1 — Create Python Environment

```bash
python -m venv venv
```

---

## Step 2 — Install Libraries

```bash
pip install fastapi uvicorn opencv-python insightface numpy
```

---

## Step 3 — Build Face Detection API

Endpoints:

```text
POST /detect-face
POST /generate-embedding
POST /verify-face
```

---

## Step 4 — Face Recognition Pipeline

### Pipeline

```text
Image → Detect Face → Crop Face → Generate Embedding → Compare
```

---

# Notification System

## Push Notifications

Use:

* Firebase Cloud Messaging

Notifications:

* Attendance success
* Absence alerts
* Class reminders

---

## SMS Notifications

Use:

* Twilio
* MSG91

---

# Real-Time Communication

## WebSockets

Purpose:

* Live attendance updates
* Instant dashboard sync
* Session updates

Use:

* Socket.IO

---

# 🔐 Security Implementation

## Authentication Security

### JWT Tokens

Store securely.

### Password Hashing

Use bcrypt.

---

## API Security

### Add:

* Rate limiting
* CORS protection
* Helmet middleware
* Input validation

---

## Biometric Security

### Important Rules

* Never expose embeddings publicly
* Encrypt biometric data
* Use HTTPS only

---

# Performance Optimization

## Face Recognition Optimization

### Use:

* GPU acceleration
* Batch processing
* Frame skipping
* Embedding caching

---

## Database Optimization

### Add indexes on:

* student_id
* timestamp
* classroom_id

---

# Scalability Plan

## MVP Stage

Single server deployment.

---

## Growth Stage

Use:

* Kubernetes
* Docker
* Load balancers
* CDN

---

# Docker Setup

## Backend Dockerfile

```dockerfile
FROM node:20
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```

---

## AI Service Dockerfile

```dockerfile
FROM python:3.11
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["uvicorn", "main:app"]
```

---

# ☁️ Deployment Guide

## Backend Deployment

Recommended:

* AWS EC2
* Railway
* Render

---

## Database Hosting

Recommended:

* AWS RDS
* Supabase
* Neon

---

## Mobile App Deployment

### Android

Publish to:

* Google Play Store

### iOS

Publish to:

* Apple App Store

---

# 🗺 Development Roadmap

# Phase 1 — MVP

## Features

* Authentication
* Face registration
* GPS verification
* Teacher webcam attendance
* Attendance reports

Timeline:
6–8 weeks

---

# Phase 2 — AI Enhancement

## Features

* Anti-spoofing
* Multi-face recognition
* Analytics dashboard
* Notification automation

Timeline:
4–6 weeks

---

# Phase 3 — Enterprise Features

## Features

* Multi-campus support
* AI insights
* Parent portal
* ERP integration
* QR fallback attendance

Timeline:
6–10 weeks

---

# Recommended APIs

## Maps

* Google Maps API

## Notifications

* Firebase Cloud Messaging

## SMS

* Twilio
* MSG91

---

# Challenges You Will Face

## Face Recognition Accuracy

Solutions:

* Good lighting
* Multiple face samples
* High-quality cameras

---

## GPS Spoofing

Solutions:

* Detect mock locations
* Device verification
* WiFi validation

---

## Large Classroom Performance

Solutions:

* Frame optimization
* GPU inference
* Queue processing

---

# Recommended AI Models

| Task               | Recommended Model |
| ------------------ | ----------------- |
| Face Detection     | RetinaFace        |
| Face Recognition   | InsightFace       |
| Anti-Spoofing      | SilentFace        |
| Landmark Detection | MediaPipe         |

---

# Estimated Costs

## Initial MVP

| Service    | Monthly Cost |
| ---------- | ------------ |
| VPS Server | $20–50       |
| Database   | $10–20       |
| Storage    | $5–15        |
| SMS        | Variable     |
| Domain     | $10/year     |

---

# Testing Strategy

## Unit Testing

Test:

* APIs
* Services
* Database operations

---

## Integration Testing

Test:

* Face verification
* Attendance flow
* Notifications

---

## Security Testing

Test:

* Authentication
* GPS spoofing
* Fake face attempts

---

# 🔮 Future Improvements

## Advanced Features

* AI analytics
* Heatmaps
* Attendance prediction
* Smart scheduling
* Voice assistant
* NFC support
* Smart ID integration

---

# Final Recommended Development Order

## Step 1

Backend authentication

## Step 2

Database setup

## Step 3

Flutter mobile app

## Step 4

Face enrollment system

## Step 5

AI recognition service

## Step 6

Teacher dashboard webcam

## Step 7

GPS verification

## Step 8

Notifications

## Step 9

Anti-spoofing

## Step 10

Deployment and scaling

---

# 🎯 Final Notes

AttendAI is a strong real-world SaaS product idea.
