
<!--by bantingrui 2205308010349-->
                    FastAPI Model Module(models file）(by bantingrui 2205308010349)

  This module defines the core data models used in the FastAPI project, covering data structures related to projects and users.

✨ Features

Clear Model Definitions: Utilize the Pydantic library to precisely define the data structures for projects and users.
Support for Creation and Update Operations: Provide models for creating and updating projects and users.

ORM Mode Compatibility: The data models support ORM mode, enabling efficient interaction with the database.

📦 Project Structure

fastapi-initial-project/
└── app/
└── models/
├── **init**.py # Initialization file
├── projects.py # Project-related data models
└── users.py # User-related data models

📮 Core Model Analysis (by Bantingrui)​
I. Project Models (projects.py)​

1. Basic Project Information Model (ProjectBase)​
   Define the basic fields of a project, serving as the base class for other models:​
   ​
   from pydantic import BaseModel​
   class ProjectBase(BaseModel):​
   title: str​
   ​
2. Project Creation Model (ProjectCreate)​
   Inherit from ProjectBase and add the user_id field (required) to specify the ID of the user who created the project:​
   ​
   class ProjectCreate(ProjectBase):​
   user_id: int​
   ​
3. Project Update Model (ProjectUpdate)​
   Inherit from ProjectBase, with fields set as optional to support partial updates:​
   ​
   from typing import Optional​
   class ProjectUpdate(ProjectBase):​
   title: Optional[str] = None​
   ​
4. Complete Project Information Model (Project)​
   Inherit from ProjectBase, add database fields (ID, user_id), and enable ORM mode:​
   ​
   class Project(ProjectBase):​
   id: int​
   user_id: int​
   class Config:​
   orm_mode = True # Support conversion from SQLAlchemy models​
   ​
   II. User Models (users.py)​
   **1. Basic User Information Model (UserBase)​**
   Define the basic fields of a user (name, email, active status):​
   ​
   class UserBase(BaseModel):​
   name: str​
   email: str​
   is_active: bool​
   ​
   **2. User Creation Model (UserCreate)​**
   Inherit from UserBase and add the password field (required for user registration):​
   ​
   class UserCreate(UserBase):​
   password: str​
   ​
   **3. User Update Model (UserUpdate)​**
   Inherit from UserBase, with all fields set as optional to support partial updates:​
   ​
   class UserUpdate(UserBase):​
   name: Optional[str] = None​
   email: Optional[str] = None​
   password: Optional[str] = None​
   is_active: Optional[bool] = None​
   ​
   **4. Complete User Information Model (User)​**
   Inherit from UserBase, add the user ID, and enable ORM mode:​
   ​
   class User(UserBase):​
   id: int​
   class Config:​
   orm_mode = True​
   \*\*​
5. User Login Model (UserLogin)​**
   Used for user authentication, containing email and password fields (verified during login):​
   ​
   class UserLogin(BaseModel):​
   email: str​
   password: str​
   class Config:​
   orm_mode = True​
   ​
   **6. Token Model (Token)​**
   Represents the token structure returned after a user logs in (access token, token type):​
   ​
   class Token(BaseModel):​
   access_token: str​
   token_type: str​
   class Config:​
   orm_mode = True​
   ​
   **7. Token Data Model (TokenData)​\*\*
   Used to store data within the token, currently only including the user's email (optional):​
   ​
   from typing import Optional​
   class TokenData(BaseModel):​
   email: Optional[str] = None​
<!--by bantingrui 2205308010349-->
=======
<!--张振锟-->
# FastAPI Starter Project

## Introduction

This is a starter project template based on the [FastAPI](https://fastapi.tiangolo.com/) framework, designed to help developers quickly bootstrap Python-based web applications. The project adopts a modular architecture, supporting rapid development, extension, and deployment.

---

## Key Features

- 🚀 **Quick Startup**: Minimal FastAPI project structure included
- 🛠️ **Extensible**: Modular code structure for easy feature expansion
- 📦 **Dependency Management**: Managed via `requirements.txt` or `poetry`
- 🔐 **Security**: Built-in environment variables support (`.env` file)
- 🗃️ **Database Support**: Compatible with multiple databases (SQLite, PostgreSQL, etc.)
- 🌐 **API Documentation**: Auto-generated interactive API docs (Swagger UI & ReDoc)

---

## Environment Requirements

Before proceeding, ensure your development environment meets the following requirements:

- Python version: `>=3.8`
- Recommended OS: Windows, MacOS, Linux
- Package manager: `pip` or `poetry`

---

## Installation Steps

### 1. Clone Repository

```bash
git clone https://github.com/zzk-zuishuai/fastapi-initial-project.git
cd fastapi-initial-project
```

### 2. Create Virtual Environment

Create and activate virtual environment using `venv`:

```bash
python -m venv venv
# Activate (Linux/MacOS)
source venv/bin/activate
# Activate (Windows)
venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## Usage Guide

### Run Development Server

Start server in development mode:

```bash
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

After server starts, access these endpoints:

- Swagger UI: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)
- ReDoc: [http://127.0.0.1:8000/redoc](http://127.0.0.1:8000/redoc)

### Database Migrations

If using databases, run migration scripts:

```bash
alembic upgrade head
```

---

## Deployment Guide

### 1. Gunicorn Deployment

```bash
gunicorn -k uvicorn.workers.UvicornWorker app.main:app --bind 0.0.0.0:8000
```

### 2. Nginx Reverse Proxy

Sample Nginx configuration:

```nginx
server {
    listen 80;
    server_name your_domain.com;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### 3. Docker Deployment (Optional)

Create `Dockerfile`:

```dockerfile
FROM python:3.10

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Build and run Docker image:

```bash
docker build -t fastapi-app .
docker run -d -p 8000:8000 fastapi-app
```

---

## Directory Structure

```plaintext
fastapi-initial-project/
├── app/
│   ├── main.py          # FastAPI entry point
│   ├── routers/         # API route modules
│   ├── models/          # Database models
│   ├── schemas/         # Data validation schemas
│   └── utils/           # Utility functions
├── tests/               # Test cases
├── requirements.txt     # Dependency list
├── Dockerfile           # Docker configuration
└── README.md            # Project documentation
```
<!--张振锟-->

<!-- by 2205308010338蒙思勇 -->
# FATSAPI Project

This is a task management tool based on "FastAPI" and "Vue.js", designed to help users manage tasks efficiently. The project is suitable for learning and practicing full-stack development, especially the combination of FastAPI and Vue.js.

✨ Project Features
📝 Task Management: Supports adding, editing, and deleting tasks to help users plan and manage their daily tasks clearly.
✅ Task Status Management: Allows users to check off tasks as completed, making it easy to track progress.
💾 Data Storage:
Frontend: Supports browser LocalStorage for task data storage, facilitating a quick experience.
Backend: Uses a database (such as MySQL) to store task data, ensuring data persistence.
🎨 Responsive Design: The interface is adapted for both mobile phones and PCs, providing a good user experience.
🔗 API Support: The backend provides RESTful APIs, making it easy to integrate with other systems.
🚀 Quick Start
Clone the Project
bash
复制
git clone https://github.com/cxkmsy/fastapi-initial-project.git  
cd fastapi-initial-project
Backend Part
Install Dependencies:
bash
复制
python -m venv venv
source venv/bin/activate  # Linux/MacOS
venv\Scripts\activate     # Windows
pip install -r requirements.txt
Configure Database:
Ensure MySQL or another database is installed and running.
Modify the database configuration in the app/config.py file.
Initialize Database:
bash
复制
python -c "from app.schemas.schema import index; index()"
Run Backend Service:
bash
复制
uvicorn app.main:app --reload
The backend service will run at http://127.0.0.1:8000.
Frontend Part
Install Dependencies:
bash
复制
cd frontend
npm install
Start Frontend Service:
bash
复制
npm run dev
The frontend service will run at http://localhost:5173.
📮 API Documentation
The backend provides automatically generated API documentation accessible at the following addresses:
Swagger UI: http://127.0.0.1:8000/docs
ReDoc: http://127.0.0.1:8000/redoc
📦 Project Structure
复制
fastapi-initial-project/
├── app/                  # Backend code directory
│   ├── controllers/      # Controller modules for handling business logic
│   │   ├── __init__.py   # Package initialization file
│   │   ├── projects.py   # Business logic related to projects
│   │   ├── users.py      # Business logic related to users
│   ├── helpers/          # Helper tool modules
│   │   ├── __init__.py   # Package initialization file
│   │   ├── auth.py       # Authentication-related utility functions
│   │   ├── hashing.py    # Hash processing tools (e.g., password encryption)
│   ├── middlewares/      # Middleware modules
│   │   ├── __init__.py   # Package initialization file
│   │   ├── auth.py       # Authentication middleware
│   ├── models/           # Database model definitions
│   │   ├── __init__.py   # Package initialization file
│   │   ├── projects.py   # Project models
│   │   ├── users.py      # User models
│   ├── routers/          # Routing modules
│   │   ├── __init__.py   # Package initialization file
│   │   ├── index.py      # Routing entry point
│   │   ├── projects.py   # Routing related to projects
│   │   ├── users.py      # Routing related to users
│   ├── schemas/          # Data validation and serialization modules
│   │   ├── __init__.py   # Package initialization file
│   │   ├── projects.py   # Pydantic models related to projects
│   │   ├── schema.py     # Database initialization script
│   │   ├── users.py      # Pydantic models related to users
│   ├── __init__.py       # Package initialization file
│   ├── config.py         # Configuration file (e.g., database connection settings)
│   ├── database.py       # Database connection and initialization
│
├── .gitignore            # Git ignore file configuration
├── LICENSE               # Project license
├── main.py               # Main entry point of the FastAPI application
├── README.md             # English version of the project documentation
├── README.zh.md          # Chinese version of the project documentation
├── requirements.txt      # List of Python dependencies
├── terms.md              # Project terms or description
├── ai_usage_screenshots  # Directory for AI usage screenshots
📸 Project Features and Screenshots
1. Add Task
Users can add new tasks by entering the task name and description.
![Add Task Interface]
2. View Task List
The task list displays all tasks, including incomplete and completed tasks.
![Task List Interface]
3. Edit Task
Supports modifying existing tasks, including task name and description.
![Edit Task Interface]
4. Delete Task
Users can delete tasks that are no longer needed.
![Delete Task Interface]
5. Check Off Completed Tasks
By checking off tasks, mark tasks as completed.
![Check Off Task Interface]
<!-- by 2205308010338蒙思勇 -->

