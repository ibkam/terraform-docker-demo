Full-Stack Application Deployment with Terraform, Docker, and PostgreSQL
Project Overview
This project demonstrates a complete full-stack application deployment using modern infrastructure-as-code and containerization technologies:

Infrastructure Provisioning: Terraform

Container Orchestration: Docker

Frontend: React.js

Backend: Node.js API

Database: PostgreSQL

Technology Stack
Infrastructure Layer
Terraform: Infrastructure as code for provisioning and managing cloud resources

Docker: Containerization of all application components

Docker Compose: Service orchestration (alternative to Terraform for local development)

Application Layer
Component	Technology	Description
Frontend	React 18	Modern single-page application
API	Node.js 18	RESTful backend service
Database	PostgreSQL 15	Relational database management system
Architecture Diagram
[Terraform] → [Docker Containers]
                     │
                     ├── [React Frontend] (3000)
                     ├── [Node.js API] (8000) → [PostgreSQL] (5432)
                     └── [Network Bridge]
Key Features
Infrastructure as Code:

Terraform scripts for reproducible infrastructure

Docker container definitions for each service

Microservices Architecture:

Decoupled frontend and backend

Independent scaling of components

Development/Production Parity:

Same containers used in development and production

Environment variables for configuration

Configuration Files
Terraform (main.tf)
hcl
resource "docker_image" "node_app" {
  name = "node-api"
  build {
    context = "./api"
  }
}

resource "docker_container" "api" {
  name  = "api"
  image = docker_image.node_app.name
  ports {
    internal = 3000
    external = 8000
  }
}
Docker Compose (docker-compose.yml)
yaml
version: '3.8'
services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
      
  api:
    build: ./api
    ports:
      - "8000:3000"
    depends_on:
      - db
      
  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - db_data:/var/lib/postgresql/data
Deployment Guide
1. Prerequisites
Docker Engine

Docker Compose

Terraform

Node.js 18+

2. Initial Setup
bash
# Clone repository
git clone https://github.com/your-repo/fullstack-app.git
cd fullstack-app

# Initialize Terraform
terraform init
3. Development Deployment
bash
# Option A: Using Docker Compose
docker-compose up --build

# Option B: Using Terraform
terraform apply
4. Production Deployment
bash
# Build production images
docker build -t frontend-prod -f frontend/Dockerfile.prod ./frontend
docker build -t api-prod -f api/Dockerfile.prod ./api

# Deploy with Terraform
terraform apply -var="environment=prod"
Troubleshooting Guide
Common Issues
Frontend Not Accessible

Verify container is running: docker ps

Check logs: docker logs frontend

Ensure port mapping: 0.0.0.0:3000->3000/tcp

API Database Connection Issues

Verify DB credentials

Check if database is initialized: docker exec db psql -U postgres -c "\l"

Terraform Deployment Errors

Reinitialize: terraform init

Check state: terraform state list

Maintenance
Updating Components
bash
# Frontend updates
cd frontend && npm update

# API updates
cd api && npm update

# Rebuild containers
docker-compose build
Monitoring
bash
# View logs
docker-compose logs -f

# Check resource usage
docker stats
License
MIT License - See LICENSE for details.
