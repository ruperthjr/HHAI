# Precision Telemedicine 2.0

**Precision Telemedicine 2.0** is an AI-driven telehealth platform designed to provide hyper-personalized health recommendations. By leveraging **Google Generative AI (GenAI)**, real-time health trends, and user-specific health data, the platform delivers tailored advice on medications, lifestyle improvements, and dietary choices. The app ensures a seamless user experience through advanced API integrations, intuitive interfaces, and secure, scalable authentication mechanisms.

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
    - **AI Insights Viewer**: Showcases **Google Generative AI**-generated recommendations.
    - **Authentication Pages**: Includes Login, Signup, Password Reset, and Account Settings.

### Backend
- **Framework**: Django with Django Rest Framework (DRF)
  - **Features**:
    - Modular APIs with role-based access control.
    - Scalable health data processing for large datasets.
    - Seamless integration with **Google Generative AI** for personalized recommendations.
  - **Services**:
    - **Health Trends API**: Fetches data from health-related trends and analyses.
    - **AI Recommendation Engine**: Provides advanced, personalized suggestions using **Google GenAI**.
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
- **App Passwords**: 
  - Secure authentication mechanism built with Django. Users can create and manage app-specific passwords for secure access to the platform. These passwords are uniquely generated and used in place of the main login credentials for added security. 
  - **App Password Flow**: Users can generate app passwords through the platform's settings interface, which are stored securely in the database and hashed using Django's password hashing algorithms.
  
- **Role-Based Access Control (RBAC)**:
  - **Patient**: Patients have access to personalized recommendations, trends, health records, and AI-driven insights. This includes interaction with their health profiles, recommendations for medication, diet, and lifestyle changes.
  - **Admin**: Admin users can manage the platform's overall configuration, including user management, health trends, data processing, and system-level settings. Admins have broader privileges to view and modify sensitive data.
  - **Additional Roles** (Optional): Future role types like "Healthcare Provider" or "Moderator" can be added to the system to allow for more customized user experiences.

- **Token-Based Authentication**:
  - **JWT Tokens**: JSON Web Tokens (JWT) are used to authenticate users. Once a user logs in, a JWT is generated and returned to the frontend, which is stored securely and used in subsequent API requests. The token contains user identification, role, and permissions.
  - **App Password Tokens**: When users authenticate using app passwords, the platform generates a separate app-specific JWT, limiting access to only app functionalities and reducing exposure of sensitive data. 
  - **Token Expiration & Refresh**: JWT tokens are issued with expiration times to enforce security, typically 1 hour for access tokens and 24 hours for refresh tokens. The refresh tokens are securely stored and can be used to obtain a new access token when needed, without requiring the user to log in again.
  
- **Password Security**:
  - All passwords, including app passwords and user account passwords, are securely hashed using Django's built-in `PBKDF2` password hashing algorithm. This ensures that even in the case of a data breach, passwords remain secure.

---

### Authentication Flow

1. **Signup**:
   - New users create accounts using a secure registration form. Required fields include username, email, and a strong password. The system validates these fields and ensures that the username and email are unique.
   - Upon successful registration, the user’s information is stored securely, and an email is sent for account verification.

2. **Login**:
   - **Email & Password**: The user logs in using their email and password. If the login is successful, a JWT is generated and returned to the client, which is then used for authenticating all subsequent API requests.
   - **App Passwords**: Alternatively, users may choose to authenticate with app passwords for better security. App passwords are used to generate specific access tokens for each app. This allows more granular control over which apps can access the system.

3. **Role Assignment**:
   - During onboarding, users are assigned a default role based on their information. Patients are automatically assigned the "Patient" role, while admins are assigned the "Admin" role.
   - Role assignments can be adjusted by system admins in the backend, giving flexibility in managing user permissions.

4. **Session Validation**:
   - Every API request that requires authentication includes the JWT token in the request headers. The middleware verifies the validity of the token, ensuring that the user is authenticated and authorized to perform the requested actions.
   - If the token is expired, the user will need to authenticate again, or use a valid refresh token to obtain a new access token.

5. **Logout**:
   - When the user logs out, their session is invalidated. This can be done by either deleting the stored token on the client-side or by sending a request to the backend to blacklist the token for additional security.

6. **Token Management**:
   - Users have the option to view and manage their tokens in the app settings. They can revoke old or unused tokens to ensure that no unauthorized access occurs in case of device compromise.
   - This allows for increased transparency and control over personal data security.
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

### Google Generative AI (GenAI) Integration

**Precision Telemedicine 2.0** integrates **Google Generative AI** for delivering personalized health insights. The AI engine analyzes user health data and recommends:
- **Context-Aware Recommendations**: Tailored advice on medications, treatments, and lifestyle based on personal health data.
- **NLP Pipelines for Research Summarization**: Transforms medical research into understandable, actionable insights for users.
- **Proactive Health Risk Prediction**: Predicts potential health risks by analyzing users' historical health data and current conditions.

With **Google Generative AI**, our platform is designed to provide smarter, more accurate health predictions and lifestyle optimization suggestions.
