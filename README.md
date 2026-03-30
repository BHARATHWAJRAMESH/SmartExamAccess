# SmartExamAccess

An intelligent AI-based system designed to provide secure and automated exam access using facial recognition and device authentication. The system utilizes advanced image processing and cloud-based technologies to identify students through face scanning and verify their registered device using MAC address mapping.

# Introduction

The Smart Exam Access system addresses this challenge by integrating facial recognition and device authentication technologies. It enables automatic verification of students entering the exam hall and ensures that only authorized devices can access the examination network. By leveraging artificial intelligence and cloud-based data management, the system improves security, reduces manual effort, and enhances the overall efficiency of exam monitoring.

Ensuring secure and fair examinations is a major challenge in modern educational systems. The Smart Exam Access system uses facial recognition and device authentication to automatically verify students and allow only authorized devices to access the exam network. This improves security, reduces malpractice, and simplifies exam management.

# Overview

This project focuses on developing an intelligent system using Artificial Intelligence to automate student verification and secure exam access. The system captures facial data, matches it with stored records, and validates registered device MAC addresses through a cloud database.

The goal of the Smart Exam Access system is to enhance exam security, prevent impersonation, and ensure that only authorized students and devices can access the examination network.

# 🎯 Objectives

* To capture and verify student identity using facial recognition
* To store and manage registered device MAC addresses in the cloud
* To authenticate and allow only authorized devices to access exam WiFi
* To prevent impersonation and unauthorized exam access
* To automate and streamline exam hall monitoring and security


# 🧠 Technologies Used

* Python
* OpenCV (for facial recognition)
* Cloud Database (for storing student data and MAC IDs)
* WiFi Network Control / Router Configuration
* Machine Learning Models (for face matching)
* NumPy, Pandas

# ⚙️ System Workflow

* Student Registration (face data and MAC ID storage)
* Entry into Exam Hall
* Face Scanning and Recognition
* Identity Verification with Database
* MAC Address Retrieval
* Device Authentication via WiFi
* Secure Network Access for Exam
* Monitoring and Access Control


# Project Structure

```
SmartExamAccess/
│
├── dataset/           # Student face images and details  
├── models/            # Face recognition models  
├── src/               # Source code  
├── utils/             # Helper functions (face processing, MAC handling)  
├── cloud/             # Cloud database integration  
└── app.py             # Main application (authentication & access control)  

```
# 📄 Abstract

Ensuring secure and fair exams is critical in educational environments. This project proposes an AI-based *Smart Exam Access* system that uses facial recognition and device authentication to control exam network access.

The system captures student face data, verifies identities, and matches registered MAC addresses stored in the cloud to allow only authorized devices to connect. Experimental results show that the system enhances exam security, prevents impersonation, and streamlines exam monitoring, making it an effective solution for educational institutions.

# 🧩 Problem Statement

Manual verification of students during exams is time-consuming, error-prone, and prone to malpractice such as impersonation. Traditional methods of checking identity and device eligibility cannot ensure complete security, especially in large exam halls.

There is a need for an intelligent system that can **automatically verify student identity** and **restrict exam network access to authorized devices**. The challenge is to design a system that can handle real-time verification for many students while maintaining accuracy and security.

The *Smart Exam Access* system addresses this problem by combining **AI-based facial recognition** and **MAC address authentication** to ensure secure, automated, and reliable exam access.


# Architecture Diagram

Student → Face Scan → Identity Verification → Retrieve MAC ID → Device Authentication → WiFi Access → Exam Network → Monitoring & Logging


# Code Implementation

```
import streamlit as st
import time
from PIL import Image
import hashlib

# ---------- Page Config ----------
st.set_page_config(page_title="🔒 Smart Exam Access", page_icon="🧑‍🎓", layout="wide")

# ---------- Custom CSS ----------
st.markdown("""
<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap');

html, body, [class*="css"] {
    font-family: 'Poppins', sans-serif;
}

/* Background gradient */
.stApp {
    background: linear-gradient(135deg, #000000, #1a1a1a, #2e2e2e);
    color: #FFFFFF;
}

/* Neon Title */
.neon-title {
    font-size: 2.8rem;
    font-weight: 700;
    text-align: center;
    margin-bottom: 25px;
    text-shadow: 0 0 15px #00ffcc, 0 0 30px #00ffff;
    color: #00ffff;
    animation: glow 2s ease-in-out infinite alternate;
}
@keyframes glow {
    from { text-shadow: 0 0 10px #00ffcc; }
    to { text-shadow: 0 0 25px #00ffff; }
}

/* Card styling with hover effect */
.card {
    background: rgba(255,255,255,0.08);
    border-radius: 16px;
    padding: 18px;
    backdrop-filter: blur(12px);
    box-shadow: 0 6px 20px rgba(0,255,255,0.3);
    margin-bottom: 20px;
    font-size: 1rem;
    color: #FFFFFF;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 25px rgba(0,255,255,0.6);
}

/* Input fields */
textarea, input, select {
    font-size: 1rem !important;
    padding: 10px 12px !important;
    border-radius: 12px !important;
    border: 1px solid #00ffff !important;
    background-color: #111111 !important;
    color: #FFFFFF !important;
    transition: border 0.3s ease, box-shadow 0.3s ease;
}
textarea:focus, input:focus, select:focus {
    border: 1px solid #00ffcc !important;
    box-shadow: 0 0 10px #00ffff;
}

/* File uploader */
div[data-testid="stFileUploader"] {
    background-color: #111111;
    border: 1px dashed #00ffff;
    border-radius: 12px;
    padding: 12px;
    transition: border 0.3s ease;
}
div[data-testid="stFileUploader"]:hover {
    border: 1px dashed #00ffcc;
}
div[data-testid="stFileUploader"] > div {
    color: #FFFFFF !important;
    background-color: transparent !important;
}
div[data-testid="stFileUploader"] button {
    background: linear-gradient(90deg, #00ffff, #00ffcc);
    color: #000000 !important;
    border: none !important;
    border-radius: 8px;
    font-weight: 600;
    transition: transform 0.2s ease;
}
div[data-testid="stFileUploader"] button:hover {
    transform: scale(1.05);
}

/* Footer */
.footer {
    color:#00ffff;
    font-size:0.85rem;
    margin-top:30px;
    text-align:center;
    opacity: 0.8;
}
</style>
""", unsafe_allow_html=True)

# ---------- Header ----------
st.markdown('<div class="neon-title">🔒 Smart Exam Access</div>', unsafe_allow_html=True)
st.write("🧑‍🎓 Scan student face and verify device to allow secure exam access.")

# ---------- Mock Database ----------
# Simulating a cloud database with student face hashes and MAC addresses
mock_student_db = {
    "student1": {
        "face_hash": "a1b2c3d4e5f6",  # Mock hash for uploaded image
        "mac_address": "00:1A:2B:3C:4D:5E"
    },
    "student2": {
        "face_hash": "f6e5d4c3b2a1",
        "mac_address": "11:22:33:44:55:66"
    }
}

# ---------- Mock Functions ----------
def scan_face(image_file):
    """
    Simulate face scanning by generating a hash of the uploaded image.
    In real system, this would use facial recognition.
    """
    img = Image.open(image_file)
    img_bytes = img.tobytes()
    face_hash = hashlib.sha256(img_bytes).hexdigest()[:12]  # take first 12 chars
    # Check against mock database
    for student, data in mock_student_db.items():
        if face_hash == data["face_hash"]:
            return True, student
    return False, None

def verify_device(mac_address, student_name):
    """
    Verify that the device MAC address matches the registered student MAC
    """
    if student_name and mock_student_db[student_name]["mac_address"].lower() == mac_address.lower():
        return True
    return False

# ---------- Layout ----------
left_col, right_col = st.columns([1,1])

with left_col:
    uploaded_face = st.file_uploader("📷 Upload/Scan Student Face (Image)", type=["jpg","png"])
    device_mac = st.text_input("💻 Enter Device MAC Address (Laptop)")

# ---------- Verification ----------
if uploaded_face and device_mac:
    with left_col:
        st.markdown('<div class="card">📂 Processing Verification...</div>', unsafe_allow_html=True)
        progress_bar = st.progress(0)
        for percent in range(0, 101, 20):
            time.sleep(0.1)
            progress_bar.progress(percent)

        face_verified, student_name = scan_face(uploaded_face)
        device_verified = verify_device(device_mac, student_name)

    with right_col:
        if face_verified and device_verified:
            st.markdown('<div class="card" style="color:#00ffcc; font-size:1.2rem; font-weight:600;">✅ Access Granted: Student Verified & Device Authorized</div>', unsafe_allow_html=True)
        elif not face_verified:
            st.markdown('<div class="card" style="color:#ff4d4d; font-size:1.2rem; font-weight:600;">❌ Access Denied: Face Not Recognized</div>', unsafe_allow_html=True)
        elif not device_verified:
            st.markdown('<div class="card" style="color:#ff4d4d; font-size:1.2rem; font-weight:600;">❌ Access Denied: Device MAC Not Authorized</div>', unsafe_allow_html=True)

# ---------- Footer ----------
st.markdown('<div class="footer">🌐 Smart Exam Access System • AI-Based Face & Device Verification</div>', unsafe_allow_html=True)

```

# Output
<img width="1366" height="768" alt="Screenshot (53)" src="https://github.com/user-attachments/assets/578ace1c-87ff-4245-9d3c-12175de04ce9" />




<img width="1366" height="768" alt="Screenshot (54)" src="https://github.com/user-attachments/assets/03f46f0a-f68d-4c73-92a4-abf8ba7aa804" />

# 🔮 Future Work

* Integrate real-time webcam-based face recognition for instant verification
* Support automatic MAC address detection and device fingerprinting
* Enhance security with multi-factor authentication (face + device + credentials)
* Deploy on cloud platforms for scalable exam management across multiple halls
* Add analytics and reporting for exam attendance and access patterns
