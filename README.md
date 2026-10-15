# MediGo 🩺

**Telemedicine platform connecting patients in underserved rural areas with remote doctors through video consultations, IoT vital-sign sensors and AI-assisted diagnosis.**

![Django](https://img.shields.io/badge/Django-092E20?logo=django&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)

---

## 📌 Context

In many rural areas, access to a doctor means hours of travel. MediGo lets a patient consult a remote doctor, share live vital signs from connected sensors, and receive a digital prescription, while an AI module supports the doctor during the consultation.

Developed as an end-of-year project (PFA) at **Université Mohammed VI des Sciences de la Santé (UM6SS)**, Digital Health Engineering program.

---

## ✨ Features

| Module | What it does |
|---|---|
| 👤 **Accounts** | Role-based access: patient, doctor, admin. Admins create and manage doctor accounts |
| 🏥 **Patients** | Patient records and dedicated patient interface |
| 📹 **Consultations** | Remote consultations between patient and doctor, with live AI assistance |
| 🤖 **AI assistant** | Symptom-based disease suggestions from a trained ML model and a medical knowledge base |
| 📡 **IoT sensors** | Vital-sign acquisition from connected sensors, sent to the platform |
| 💊 **Prescriptions** | PDF prescriptions generated after the consultation, with a QR code for verification |
| 💬 **Messaging** | Patient–doctor chat with image sharing |
| 🌍 **i18n** | Multilingual patient interface |

---

## 🏗️ Architecture

```
                    ┌─────────────┐
   Browser  ──────▶ │    Nginx    │ ──▶ /static, /media
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐        ┌──────────────┐
                    │   Django    │ ◀────▶ │  PostgreSQL  │
                    │ (Uvicorn,   │        └──────────────┘
                    │   ASGI)     │        ┌──────────────┐
                    │             │ ◀────▶ │    Redis     │
                    └──────▲──────┘        └──────────────┘
                           │
                    ┌──────┴──────┐
                    │ IoT sensors │
                    └─────────────┘
```

All services run with **Docker Compose** on a shared bridge network.

---

## 🛠️ Tech stack

- **Backend:** Python, Django, Django REST Framework, Uvicorn (ASGI)
- **Database / cache:** PostgreSQL 15, Redis 7
- **AI:** Python ML model (joblib) + medical knowledge base
- **IoT:** Python sensor reader
- **Frontend:** Django templates, HTML/CSS/JavaScript
- **DevOps:** Docker, Docker Compose, Nginx

---

## 📁 Project structure

```
medigo_full/medigo/
├── backend/
│   ├── apps/
│   │   ├── accounts/        # Authentication & roles
│   │   ├── patients/        # Patient records
│   │   ├── consultations/   # Consultations + live AI
│   │   ├── ordonnances/     # PDF prescriptions + QR codes
│   │   ├── messaging/       # Patient–doctor chat
│   │   ├── sensors/         # Vital-sign data
│   │   └── i18n_patient/    # Multilingual patient UI
│   ├── medigo_ai/           # ML model, features, knowledge base
│   ├── config/              # Django settings, ASGI/WSGI, URLs
│   ├── templates/           # patient / medecin / admin / auth views
│   ├── static/
│   ├── Dockerfile
│   └── requirements.txt
├── sensors/reader.py        # IoT sensor reader
├── nginx/nginx.conf
├── docker-compose.yml
└── .env.example
```

---

## 🚀 Getting started

**Prerequisites:** Docker and Docker Compose

```bash
git clone https://github.com/Imane-Guihia/MediGo.git
cd MediGo/medigo_full/medigo

# 1. Configure environment variables
cp .env.example .env
# then edit .env with your own values

# 2. Build and start all services
docker compose up --build

# 3. Create an admin account
docker compose exec django python manage.py createsuperuser
```

The app is then available at **http://localhost**.

> ⚠️ The ML model file (`medigo_model_patient.joblib`, ~70 MB) is included in the repository.

---

## 🔒 Data & privacy

This repository contains **no patient data**. Uploaded files (`media/`), the database and secrets (`.env`) are excluded from version control.

---

## 👥 Authors

- **Imane Guihia** · [@Imane-Guihia](https://github.com/Imane-Guihia)
- **Aya Adrif** · [@AyaA-23](https://github.com/AyaA-23)
- **Zakaria Ouakil**

**Supervisor:** Pr. Fouad Moutaouakkil, UM6SS
