# Project Outline: Precision Telemedicine 2.0

## Overview
**Precision Telemedicine 2.0** is an AI-driven telehealth platform designed to provide hyper-personalized health recommendations. By leveraging Google Generative AI, real-time health trends, and user-specific health data, the platform delivers tailored advice on medications, lifestyle improvements, and dietary choices. The app ensures a seamless user experience through advanced API integrations, intuitive interfaces, and secure, scalable authentication mechanisms.

---

## Architecture Overview

### Frontend
- **Framework**: React.js
  - **Features**:
    - Tailwind CSS for elegant, responsive UI design.
    - React Router for dynamic navigation.
    - Axios for efficient API communication and real-time updates.
  - **Components**:
    - **Dashboard**: Displays trending health topics, AI-powered medication insights, and lifestyle recommendations.
    - **Health Profile**: Secure interface for capturing/updating health history.
    - **AI Insights Viewer**: Showcases Google Generative AI-generated recommendations.
    - **Authentication Pages**: Includes Login, Signup, Password Reset, and Account Settings.

### Backend
- **Framework**: Django with Django Rest Framework (DRF)
  - **Features**:
    - Modular APIs with role-based access control.
    - Scalable health data processing for large datasets.
    - Seamless Google Generative AI integration for personalized recommendations.
  - **Services**:
    - **Health Trends API**: Fetches data from Google Trends.
    - **AI Recommendation Engine**: Provides advanced, personalized suggestions.
    - **User Management**: Manages authentication, roles, and user health data securely.

### Database
- **Development**: SQLite3 for development; PostgreSQL in production.

- **Schema Overview**:
  1. **Users**: Stores authentication, roles, and account metadata.
  2. **HealthProfiles**: Detailed user health conditions and histories.
  3. **TrendingTopics**: Logs health-related trends fetched from APIs.
  4. **Recommendations**: Tracks AI-generated suggestions for users.
  5. **MedSuggestions**: Tracks medication data and dosages.
  6. **MedOptimization**: Optimizes medication data with cost insights.

---

## Authentication

### Key Features
- **App Passwords**: Secure authentication mechanism using Django.
- **Role-Based Access Control**:
  - **Patient**: Access to personalized recommendations and trends.
  - **Admin**: Management of trends, users, and system configurations.
- **Password Security**: Encrypted using Django’s hashing tools.

### Flow
1. **Signup**: New users create accounts.
2. **Login**: Authenticated sessions initiated via secure tokens.
3. **Role Assignment**: Patient or Admin roles assigned during onboarding.
4. **Session Validation**: Middleware validates tokens for each request.

---

## Database Schema

### Users Table
| Field         | Type          | Constraints           |
|---------------|---------------|-----------------------|
| id            | Integer       | Primary Key           |
| username      | Varchar(150)  | Unique, Not Null      |
| email         | Varchar(255)  | Unique, Not Null      |
| password      | Varchar(255)  | Hashed                |
| role          | Varchar(50)   | Default='Patient'     |
| created_at    | DateTime      | Auto-timestamp        |

### HealthProfiles Table
| Field         | Type          | Constraints           |
|---------------|---------------|-----------------------|
| id            | Integer       | Primary Key           |
| user_id       | Integer       | Foreign Key (Users)   |
| condition     | Varchar(255)  | Not Null              |
| details       | Text          | Optional              |
| updated_at    | DateTime      | Auto-timestamp        |

### TrendingTopics Table
| Field         | Type          | Constraints           |
|---------------|---------------|-----------------------|
| id            | Integer       | Primary Key           |
| topic         | Varchar(255)  | Not Null              |
| search_volume | Integer       | Not Null              |
| created_at    | DateTime      | Auto-timestamp        |

### Recommendations Table
| Field              | Type          | Constraints           |
|--------------------|---------------|-----------------------|
| id                 | Integer       | Primary Key           |
| user_id            | Integer       | Foreign Key (Users)   |
| topic              | Varchar(255)  | Not Null              |
| recommendation_text| Text          | Not Null              |
| created_at         | DateTime      | Auto-timestamp        |

### MedSuggestions Table
| Field         | Type          | Constraints           |
|---------------|---------------|-----------------------|
| id            | Integer       | Primary Key           |
| name          | Varchar(150)  | Unique, Not Null      |
| quantity      | Double        | Not Null              |
| dosification  | Varchar(255)  | Not Null              |
| details       | Text          | Optional              |
| created_at    | DateTime      | Auto-timestamp        |
| updated_at    | DateTime      | Auto-timestamp        |

### MedOptimization Table
| Field                | Type          | Constraints           |
|----------------------|---------------|-----------------------|
| id                   | Integer       | Primary Key           |
| comercial_name       | Varchar(150)  | Unique, Not Null      |
| cost                 | Double        | Not Null              |
| quantity             | Double        | Not Null              |
| dosification         | Varchar(255)  | Not Null              |
| optimizationDetails  | Text          | Optional              |
| created_at           | DateTime      | Auto-timestamp        |

---

## API Endpoints

### Authentication
1. **POST** `/api/auth/signup/`
2. **POST** `/api/auth/login/`
3. **POST** `/api/auth/logout/`

### Health Trends
1. **GET** `/api/trends/`
2. **GET** `/api/trends/:id/`

### Recommendations
1. **POST** `/api/recommendations/`
2. **GET** `/api/recommendations/:user_id/`

### User Health Profile
1. **GET** `/api/health-profile/:user_id/`
2. **PUT** `/api/health-profile/:user_id/`

---

## AI Integration

### Features:
1. **Context-Aware Recommendations**
2. **NLP Pipelines for Research Summarization**
3. **Proactive Health Risk Prediction**

---

