# Healthcare API

A Django REST Framework backend for managing patients, doctors, and their relationships. JWT authentication is used for secure access. Swagger UI is available for API documentation and testing.

---

## Folder Structure

```
Healthcare/
├── healthcare/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
├── core/
│   ├── __init__.py
│   ├── models.py
│   ├── views.py
│   ├── serializers.py
│   └── migrations/
├── manage.py
└── staticfiles/
```

---

## Setup & Running

1. **Install dependencies:**
   ```
   pip install -r requirements.txt
   ```
2. **Apply migrations:**
   ```
   python manage.py migrate
   ```
3. **Create a superuser (optional):**
   ```
   python manage.py createsuperuser
   ```
4. **Run the server:**
   ```
   python manage.py runserver
   ```
5. **Access Swagger UI:**
   ```
   http://localhost:8000/api/swagger/
   ```

---

## Authentication

- Uses JWT (JSON Web Token) via `rest_framework_simplejwt`.
- Obtain token via `/api/auth/login/` (POST).
- Refresh token via `/api/auth/refresh/` (POST).
- Register new users via `/api/auth/register/` (POST).

**Swagger Authorization:**  
Copy your access token from the login response, click "Authorize" in Swagger, and paste it as:

```
Bearer <access_token>
```

(include the word `Bearer`, a space, then your token).

---

## API Endpoints

### 1. User Registration

- **POST** `/api/auth/register/`
- **Request Body:**
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "yourpassword"
  }
  ```
- **Response:**
  ```json
  {
    "id": 1,
    "email": "john@example.com"
  }
  ```

### 2. Login (JWT Token)

- **POST** `/api/auth/login/`
- **Request Body:**
  ```json
  {
    "email": "john@example.com",
    "password": "yourpassword"
  }
  ```
- **Response:**
  ```json
  {
    "access": "<access_token>",
    "refresh": "<refresh_token>"
  }
  ```

### 3. Refresh Token

- **POST** `/api/auth/refresh/`
- **Request Body:**
  ```json
  {
    "refresh": "<refresh_token>"
  }
  ```
- **Response:**
  ```json
  {
    "access": "<new_access_token>"
  }
  ```

### 4. Patients

- **GET** `/api/patients/`  
  List all patients (admin) or only your patients (regular user).
- **POST** `/api/patients/`  
  Create a new patient.
- **GET** `/api/patients/{id}/`  
  Retrieve patient details.
- **PUT/PATCH** `/api/patients/{id}/`  
  Update patient details.
- **DELETE** `/api/patients/{id}/`  
  Delete a patient.

**Patient Model Fields:**

- `id`
- `owner` (username)
- `name`
- `age` (0-100)
- `gender` ("male", "female", "other")
- `address`
- `created_at`

### 5. Doctors

- **GET** `/api/doctors/`  
  List all doctors.
- **POST** `/api/doctors/`  
  Create a new doctor.
- **GET** `/api/doctors/{id}/`  
  Retrieve doctor details.
- **PUT/PATCH** `/api/doctors/{id}/`  
  Update doctor details.
- **DELETE** `/api/doctors/{id}/`  
  Delete a doctor.

**Doctor Model Fields:**

- `id`
- `name`
- `specialization`
- `phone`
- `created_at`

### 6. Patient-Doctor Mappings

- **GET** `/api/mappings/`  
  List all patient-doctor mappings.
- **GET** `/api/mappings/{id}/doctors_for_patient/`  
  List all mappings for a particular patient.
- **POST** `/api/mappings/`  
  Create a new mapping.
- **GET** `/api/mappings/{id}/`  
  Retrieve mapping details.
- **PUT/PATCH** `/api/mappings/{id}/`  
  Update mapping.
- **DELETE** `/api/mappings/{id}/`  
  Delete mapping.

**Mapping Model Fields:**

- `id`
- `assigned_at`
- `patient` (ID)
- `patient_name`
- `doctor` (ID)
- `doctor_name`
- `assigned_by` (ID)

---

## Example Mapping Response

```json
{
  "id": 1,
  "assigned_at": "2025-09-16T12:26:26.083016Z",
  "patient": 1,
  "patient_name": "John Doe",
  "doctor": 1,
  "doctor_name": "Dr. Smith",
  "assigned_by": 1
}
```

---

## Notes

- All endpoints (except registration and login) require JWT authentication.
- Use the Swagger UI for interactive API exploration and testing.
- For production, configure environment variables and static file serving as shown in `healthcare/settings.py`.

---

## License

MIT License (or specify your license here)
