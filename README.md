# LearnDaily – AI-Powered Learning & Q&A Platform

LearnDaily is a backend-focused AI-powered learning platform built as a set of Spring Boot microservices. It supports user authentication, AI-generated learning questions, answer evaluation, practice sessions, and learning history.

## Architecture

```text
                         ┌──────────────────────┐
                         │      LearnDaily      │
                         │   Client / Frontend  │
                         └──────────┬───────────┘
                                    │
                    ┌───────────────┼────────────────┐
                    │               │                │
                    ▼               ▼                ▼
          ┌────────────────┐ ┌──────────────┐ ┌────────────────┐
          │  Auth Service  │ │ Topic / LLM  │ │ Session Service│
          │    :8086       │ │    :8088     │ │     :8087      │
          └───────┬────────┘ └──────┬───────┘ └───────┬────────┘
                  │                 │                 │
                  │                 │                 │
                  ▼                 ▼                 ▼
             MySQL / JPA       OpenAI API        PostgreSQL /
                                                   CockroachDB
                                    ▲
                                    │
                                    └── OpenFeign
```

The **Session Service** coordinates practice flows and communicates with the **Topic / LLM Service** for question generation and answer evaluation.

## Microservices

| Service | Responsibility | Technology | Repository |
|---|---|---|---|
| Auth Service | Registration, login, JWT authentication and authorization | Java 17, Spring Boot, Spring Security, JPA, MySQL | [QnAauthService](https://github.com/annubelgaonkar/QnAauthService) |
| Topic / LLM Service | AI question generation and answer evaluation | Java 17, Spring Boot, WebClient, OpenAI API | [QnATopicService](https://github.com/annubelgaonkar/QnATopicService) |
| Session Service | Practice sessions, answers, tutor flow and history | Java 17, Spring Boot, OpenFeign, JPA, PostgreSQL/CockroachDB | [QnASessionService](https://github.com/annubelgaonkar/QnASessionService) |

## Key Features

- JWT-based authentication with Spring Security and BCrypt password hashing
- AI-generated questions using the OpenAI Chat Completions API
- AI-based answer evaluation and feedback
- Practice session creation and progress tracking
- Session history with pagination
- Tutor-style learning flow
- Inter-service communication using Spring Cloud OpenFeign
- RESTful APIs built with Spring Boot
- Environment-variable based configuration for credentials and secrets
- Unit and controller testing with JUnit, Mockito and Spring Security Test

## API Overview

### Auth Service – `8086`

- `POST /auth/register` – Register a user
- `POST /auth/login` – Authenticate and receive a JWT

### Topic / LLM Service – `8088`

- `POST /llm/generate` – Generate learning questions
- `POST /llm/evaluate` – Evaluate an answer
- `POST /llm/generateTutor` – Generate tutor questions
- `POST /llm/evaluateForTutor` – Evaluate tutor answers

### Session Service – `8087`

- `POST /session/start` – Start a practice session
- `POST /session/update` – Submit/update an answer
- `GET /session/history?page=0&size=5` – Retrieve session history
- `GET /session/{sessionId}` – Retrieve session details
- `POST /tutor/start` – Start a tutor session
- `GET /tutor/questions/{questionId}` – Retrieve tutor question details
- `POST /tutor/evaluate` – Evaluate a tutor response
- `POST /tutor/end/{sessionId}` – End a tutor session

## Technology Stack

**Backend:** Java 17, Spring Boot, Spring MVC, Spring Data JPA, Hibernate

**Security:** Spring Security, JWT, BCrypt

**AI Integration:** OpenAI API, Spring WebClient

**Microservices Communication:** Spring Cloud OpenFeign

**Databases:** MySQL, PostgreSQL / CockroachDB

**Testing:** JUnit, Mockito, Spring Security Test

**Build Tool:** Maven

## Configuration

Secrets and environment-specific values are externalized rather than hard-coded.

Typical configuration includes:

### Auth Service
```text
SERVER_URL_LOCAL
DB_USERNAME
DB_PASSWORD
JWT_SECRET
JWT_EXPIRATION
```

### Topic / LLM Service
```text
OPENAI_API_KEY
LLM_PROMPT_QUESTION
LLM_PROMPT_FEEDBACK
```

### Session Service
```text
SERVER_URL_REMOTE
COCKROACHDB_USERNAME
COCKROACHDB_PASSWORD
LLM_SERVICE_BASE_URL
JWT_SECRET
TRUNCATE_LENGTH
```

Refer to each service repository for its complete application configuration and setup instructions.

## Running Locally

Each microservice is maintained in its own repository.

1. Clone all three service repositories.
2. Configure the required environment variables.
3. Start the Auth Service on port `8086`.
4. Start the Session Service on port `8087`.
5. Start the Topic / LLM Service on port `8088`.
6. Use Postman or another REST client to exercise the APIs.
7. Ensure the Session Service can reach the Topic / LLM Service through `LLM_SERVICE_BASE_URL`.

## Project Structure

This repository acts as the **project showcase / documentation hub** for LearnDaily. The production code is maintained in the three dedicated service repositories:

```text
Learn-Daily/
└── README.md

QnAauthService/
QnATopicService/
QnASessionService/
```

Keeping the services separate demonstrates a microservices-oriented architecture while this repository provides one entry point for the complete system.

## Repository Links

- [Authentication Service](https://github.com/annubelgaonkar/QnAauthService)
- [Topic / LLM Service](https://github.com/annubelgaonkar/QnATopicService)
- [Session Service](https://github.com/annubelgaonkar/QnASessionService)

## Author

**Anuradha Belgaonkar**

[GitHub Profile](https://github.com/annubelgaonkar)
