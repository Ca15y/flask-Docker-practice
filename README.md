Flask Docker CI/CD Pipeline
A production-style deployment pipeline built with Flask, Docker, Nginx, AWS EC2, and GitHub Actions. Every push to main automatically tests and deploys the app to a live server.



What This Project Does
This project demonstrates a complete end-to-end DevOps workflow:

A simple Flask API serves two endpoints
The app is containerized with Docker for consistent, portable deployments
Nginx acts as a reverse proxy, routing public traffic to the Flask container
A GitHub Actions pipeline automatically runs tests and deploys to AWS EC2 on every push to main


Tech Stack
Tool
Purpose
Python / Flask
Web application
Docker
Containerization
Nginx
Reverse proxy
AWS EC2
Cloud server
GitHub Actions
CI/CD pipeline



Project Structure
flask-Docker-practice/

├── app.py                  # Flask application

├── Dockerfile              # Container configuration

├── requirements.txt        # Python dependencies

├── conftest.py             # Pytest configuration

├── .dockerignore           # Files excluded from Docker build

├── tests/

│   └── test_app.py         # Automated tests

└── .github/

    └── workflows/

        └── deploy.yml      # CI/CD pipeline definition


API Endpoints
Endpoint
Method
Response
/
GET
{"message": "Hello from my CI/CD pipeline!", "status": "running"}
/health
GET
{"status": "healthy"}



How the Pipeline Works
Push to main branch

       │

       ▼

GitHub Actions triggered

       │

       ▼

  [Test Job]

  - Spin up fresh Ubuntu runner

  - Install dependencies

  - Run pytest

       │

  Tests pass?

  ├── No  → Pipeline stops. Nothing is deployed.

  └── Yes ▼

  [Deploy Job]

  - SSH into EC2 server

  - Pull latest code

  - Stop existing container

  - Build new Docker image

  - Start new container with restart policy

       │

       ▼

Live URL updated automatically


Infrastructure
Server: AWS EC2 t2.micro (Ubuntu 24.04 LTS)
Reverse Proxy: Nginx listening on port 80, forwarding to Flask on port 5000
Container: Docker with --restart always policy (auto-restarts on crash or reboot)
Secrets: EC2 host, username, and SSH key stored as GitHub repository secrets


Running Locally
Clone the repository:

git clone https://github.com/Ca15y/flask-Docker-practice.git

cd flask-Docker-practice

Run with Python:

python -m venv venv

venv\Scripts\activate

pip install -r requirements.txt

python app.py

Run with Docker:

docker build -t flask-docker-practice .

docker run -p 5000:5000 flask-docker-practice

Visit http://localhost:5000

Run tests:

pytest tests/


Key Concepts Demonstrated
Containerization — Docker ensures the app runs identically in development and production
Reverse proxying — Nginx decouples the public-facing server from the application layer
CI/CD — Automated testing gates every deployment, preventing broken code from reaching production
Least privilege — EC2 access is handled via SSH key pairs, not passwords
Fail fast — The deploy job is blocked by the test job; a failed test stops the pipeline immediately

