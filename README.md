# Brainbase K8S Toy Problem Template

This assignment requires you to create a Kubernetes-based task management system using a simplified architecture and a Next.js client application.

<img width="1328" alt="Untitled (3)" src="https://github.com/user-attachments/assets/6942b0a2-f03e-438c-9bcb-9df8da8d9110">

## Introduction

The goal of this project is to develop a Kubernetes-based system that handles tasks using a microservices architecture. You will build a system where tasks are managed by an API server, processed by a task engine, communicated via RabbitMQ, and stored in a MongoDB database. Additionally, you will create a Next.js client application to interact with the system.

## Installing the template

### Prerequisites

Ensure you have the following tools installed on your machine:
- Docker
- Kubernetes (Minikube or any other local K8S setup)
- Node.js
- MongoDB

### Installation

Clone the repository to your local machine:

```bash
git clone https://github.com/BrainbaseHQ/template-k8s-toy-problem
cd template-k8s-toy-problem
```

### Setting Up Kubernetes Cluster

Start your Kubernetes cluster (using Minikube or any other local K8S setup):

```bash
minikube start
```

## Project Structure

Your project should have the following structure:

```
template-k8s-toy-problem/
│
├── api-server/
│   ├── Dockerfile
│   ├── index.js
│   ├── package.json
│   └── k8s/
│       ├── deployment.yaml
│       └── service.yaml
│
├── task-engine/
│   ├── Dockerfile
│   ├── index.js
│   ├── package.json
│   └── k8s/
│       ├── deployment.yaml
│       └── service.yaml
│
├── client/
│   ├── Dockerfile
│   ├── pages/
│   │   └── index.js
│   ├── package.json
│   └── k8s/
│       ├── deployment.yaml
│       └── service.yaml
│
├── k8s/
│   ├── mongodb-deployment.yaml
│   ├── rabbitmq-deployment.yaml
│   └── ingress.yaml
│
└── README.md
```

### Components

- **api-server**: Handles client requests and interacts with RabbitMQ and MongoDB.
- **task-engine**: Processes tasks from RabbitMQ, calls OpenAI to generate code, and updates MongoDB.
- **client**: Next.js application for interacting with the system.
- **RabbitMQ**: Message broker for task distribution.
- **MongoDB**: Database for storing task information.

### Task Schema

Tasks in MongoDB should follow this schema:

```json
{
    "_id": "unique-task-id",
    "user_id": "XXX",
    "objective": "Create a function that sums up three numbers.",
    "language": "python",
    "created_at": "2024-07-14T00:00:00Z",
    "completed_at": null,
    "status": "pending",
    "code": ""
}
```

## Requirements

### RabbitMQ Configuration

- RabbitMQ should feed the task engine with 10 tasks at a time.
- Tasks should have a TTL (time-to-live) of 5 minutes.
- Tasks should retry until acknowledged.

### Task Engine

- The task engine is responsible for taking tasks, calling OpenAI to write the code in the specified language for the given objective, and updating the `code` field in the task.
- Task status should be updated to "running" when processing starts and to "completed" or "failed" after processing.

### Client

- The client should be a single-page Next.js application with two input boxes for objective and language.
- The client should display a list of the user's tasks.
- All requests to MongoDB should go through the API server.

## Milestones

### Milestone 1: Basic API Server

#### Criteria
- [ ] API server can handle `/tasks/add` and `/tasks/get` endpoints.
- [ ] Tasks are stored in MongoDB.

### Milestone 2: Task Distribution

#### Criteria
- [ ] API server sends tasks to RabbitMQ.
- [ ] Task Engine retrieves tasks from RabbitMQ.

### Milestone 3: Task Processing

#### Criteria
- [ ] Task Engine retrieves tasks from RabbitMQ.
- [ ] Task Engine processes tasks, calls OpenAI to generate code, updates MongoDB with the code, and updates the task status.

### Milestone 4: Next.js Client

#### Criteria
- [ ] Next.js client with input boxes for objective and language.
- [ ] Client displays a list of the user's tasks.
- [ ] Client communicates with the API server.

## Final Run

Once all four milestones are successfully completed, you should have a fully functional Kubernetes-based task management system with a client application.

## Notes

Ensure that the system is scalable, fault-tolerant, and easy to maintain. This aligns with Brainbase's goal of providing reliable and efficient automation solutions.
