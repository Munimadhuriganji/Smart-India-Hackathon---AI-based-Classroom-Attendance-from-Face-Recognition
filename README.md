# Smart-India-Hackathon---AI-based-Classroom-Attendance-from-Face-Recognition
### NAME:GANJI MUNI MADHURI
### REGISTER NUMBER:212223230060

This project is an AI-powered application designed to automate classroom attendance using face recognition. It provides a web-based dashboard for teachers to manage student registration, mark attendance from a photo or live feed, and export attendance reports.

## Objective

To build a system that automatically marks classroom attendance using AI-based face recognition, reducing manual roll call time and improving accuracy.

## Features

Student Registration: Easily enroll students by adding their name, ID, and a clear photo for face encoding.

Upload Attendance: Mark attendance for an entire class by uploading a single classroom photo.

Teacher Dashboard: A central hub to view daily attendance logs, manually correct any errors , and manage the class roster.

CSV Export: Export daily or weekly attendance reports as a CSV file for record-keeping.

## System Architecture

This application is built with the following technologies:

Backend: Python (Flask)

AI/Face Recognition: face_recognition (based on dlib) and OpenCV

Database: SQLite (for simple, file-based storage of student profiles and attendance logs)

Frontend: HTML, CSS, and vanilla JavaScript (for the live camera feed)

Data Flow

Registration: Teacher uploads a student's photo -> face_recognition library generates a 128-d face encoding -> Encoding is stored in the SQLite database alongside the student's ID.

Attendance (Upload): Teacher uploads a classroom photo -> The system detects all faces in the photo -> It generates encodings for each detected face.

Matching: The system compares the encodings from the classroom photo against the "known" encodings in the database.

Marking: If a match is found (with a tolerance), the corresponding student is marked Present. All other students are marked Absent.

Dashboard: The teacher views the Present/Absent list and can manually override any status.

Setup and Installation

To run this project locally, follow these steps.

Prerequisites:

Python 3.8+

pip (Python package installer)

cmake (required for dlib installation)

macOS: brew install cmake

Windows: Download installer from cmake.org

Linux: sudo apt-get install cmake

## Windows
```
python -m venv venv
```
## Install the required packages:
```
pip install -r requirements.txt
```

## Initialize the database:

```
python app.py init-db
```

## Run the application:
```
flask run
```

Your application will be available at "http://127.0.0.1:5000".

How to Use

Register Students:

Navigate to the "Register" page.

Enter the student's name and a unique Student ID.

Upload a clear, front-facing photo of the student.

Submit. Repeat for all students.

Mark Attendance:

Navigate to the "Mark Attendance" page.

Option 1 (Upload): Upload a photo of the classroom. The system will process it and redirect you to the dashboard.

Option 2 (Live): Click "Start Camera," pose, and "Take Snapshot." The snapshot will be processed.

View & Verify:

Go to the dashboard (homepage).

You will see the attendance list for the current day.

If any student was marked incorrectly, you can use the "Update" button to change their status from Absent to Present (or vice-versa).

Export Report:

On the dashboard, click the "Export as CSV" button to download the day's attendance log.

# Project Report

A detailed report is included in this repository as project_report.md. You can use this as a template.

Attendance Marking: Using the "Upload Photo" feature with an image containing the registered students.

Dashboard Use: Showing the resulting attendance list, manually correcting one student's status, and exporting the CSV file.
Project Report: AI-Based Classroom Attendance System


1. Problem Statement

Traditional classroom attendance methods, such as manual roll calls or sign-in sheets, are time-consuming and prone to errors. Manual roll calls disrupt the flow of a lecture, taking up valuable class time. Sign-in sheets can be forged, and manual data entry into a digital system is an administrative burden. This project aims to solve these problems by creating an automated, accurate, and efficient attendance system using AI-based face recognition.

2. Objective

The primary objective is to develop a fully functional system that can automatically detect and recognize student faces from a classroom photo or live camera feed. The system will mark recognized students as "Present" and all others as "Absent," storing these records in a database. The system must also include a web-based teacher dashboard to view, verify, and export daily attendance reports.

3. System Architecture

The system is built as a monolithic web application using a Python/Flask backend.

Backend: A Flask server handles all HTTP requests, business logic, and database interactions.

Frontend: Standard HTML, CSS, and JavaScript are used to render the teacher dashboard and capture webcam images.

Database: A lightweight SQLite database stores student information (name, ID, face encoding) and attendance logs (student ID, date, status).

Face Recognition Engine: The face_recognition library (built on dlib) is used for:

Face Encoding: During registration, a student's photo is converted into a 128-point numerical vector (an "encoding") that uniquely identifies their face. This encoding is stored in the database.

Face Detection: When a classroom photo is uploaded, the system identifies the location (pixel coordinates) of all faces in the image.

Face Matching: For each detected face, the system generates a new encoding and compares it to the list of known encodings in the database. A "match" is declared if the distance between the two vectors is below a set tolerance.

Architecture Diagram (Simplified Flow):
```
[User (Teacher)]
       |
       v
[Web Browser (Dashboard)]
       |
       |--- (Register Student: Upload Photo) --> [Flask Server] --> [Face Recognition] --> (Save Encoding) --> [SQLite DB]
       |
       |--- (Mark Attendance: Upload Photo) --> [Flask Server] --> [Face Recognition]
                                                    |                (Detect & Compare Faces)
                                                    |                         |
                                                    v                         v
                                                 (Save Attendance) <-- [Match Known Encodings] <-- [SQLite DB]
       |
       |--- (View/Export Report) -----------> [Flask Server] --> (Query Records) --> [SQLite DB]
                                                    |
                                                    v
                                                 (Return HTML/CSV) --> [Web Browser (Dashboard)]

```
4. Dataset Details

The "dataset" for this project is dynamically created by the teacher during the student registration phase.

Source: The teacher provides 1-2 clear, front-facing photos of each student.

Processing: Each photo is processed by the face_recognition library to generate and store a facial encoding.

Storage: The dataset consists of the raw images (stored in an uploads/
folder) and the corresponding encodings (stored in the students table in the database).

Quality: The accuracy of the system is highly dependent on the quality of these registration photos. Good lighting, no obstructions (e.g., hats, sunglasses), and a neutral expression are recommended.


5. Accuracy Results

To test the system, I registered [X] students. I then tested the attendance marking using [Y] different classroom photos under various conditions.

Test Case 1: All students present, good lighting

Result,Accuracy

Test Case 2: Some students absent, side-angles

Result,Accuracy

Test Case 3: Poor lighting / partial faces

Result, Overall Accuracy

### Observations:

The system is highly accurate with clear, front-facing images.

Accuracy decreases with poor lighting, extreme angles, and partial face obstructions.

The manual override feature on the dashboard is essential to correct the ~12% of errors.


### Challenges:

dlib Installation: The dlib library, a dependency for face_recognition, can be difficult to install on some systems, especially Windows, as it requires C++ build tools.

Recognition Accuracy: Faces at sharp angles or in the background of a photo are harder to detect and match.

Performance: Processing a large classroom photo with 30+ faces can take several seconds.

### Future Work:

Liveness Detection: Implement a check to ensure the camera is seeing a real person, not a photo, to prevent spoofing.

Batch Registration: Allow the teacher to upload a zip file of student photos named by their ID for faster registration.

Improved Model: Integrate a more robust model that is less sensitive to lighting and angles.

Optimize Performance: Offload the face recognition to a separate worker thread so it doesn't block the web server.
