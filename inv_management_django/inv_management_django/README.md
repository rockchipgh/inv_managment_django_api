# Inventory Management System API

A simple backend API for managing an inventory system. The API supports CRUD operations, JWT-based authentication, Redis caching for performance optimization, and PostgreSQL for persistent data storage.

## Technologies Used
- **Django**: Web framework
- **Django Rest Framework (DRF)**: API development
- **PostgreSQL**: Database
- **Redis**: Caching
- **JWT**: Authentication
- **Docker**: Container management

## Features
- **CRUD Operations**: Create, read, update, and delete inventory items.
- **JWT Authentication**: Secure the API endpoints using JWT-based authentication.
- **Redis Caching**: Cache frequently accessed items to enhance performance.
- **Logging**: Integrated logging for monitoring and debugging.
- **Unit Tests**: Comprehensive unit tests using Django's test framework.

## Prerequisites
Ensure you have the following installed:
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Python 3.8+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installation/)

## Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/inventory-management-system.git
cd inventory-management-system
```

## Technologies Used
- **Django**: Web framework
- **Django Rest Framework (DRF)**: API development
- **PostgreSQL**: Database
- **Redis**: Caching
- **JWT**: Authentication
- **Docker**: Container management

## Features
- **CRUD Operations**: Create, read, update, and delete inventory items.
- **JWT Authentication**: Secure the API endpoints using JWT-based authentication.
- **Redis Caching**: Cache frequently accessed items to enhance performance.
- **Logging**: Integrated logging for monitoring and debugging.
- **Unit Tests**: Comprehensive unit tests using Django's test framework.

## Prerequisites
Ensure you have the following installed:
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Python 3.8+](https://www.python.org/downloads/)

## Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/inventory-management-system.git
cd inventory-management-system
```
### 2. Create a Python Virtual Environment and Activate It
```bash
python3 -m venv venv
source venv/bin/activate  # On Windows use: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Set Up the PostgreSQL Database
Make sure PostgreSQL is installed and running on your system. You can also run it in Docker if needed.

#### Create a new PostgreSQL database:

```SQL
# Start psql and run the following commands:
CREATE DATABASE inventory_db;
CREATE USER inventory_user WITH PASSWORD 'your_password';
ALTER ROLE inventory_user SET client_encoding TO 'utf8';
ALTER ROLE inventory_user SET default_transaction_isolation TO 'read committed';
ALTER ROLE inventory_user SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE inventory_db TO inventory_user;
```

#### Update the DATABASES configuration in settings.py:
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'inventory_db',
        'USER': 'inventory_user',
        'PASSWORD': 'your_password',
        'HOST': 'localhost',  # If running PostgreSQL in Docker, this might be 'db'
        'PORT': '5432',
    }
}
```

### 5. Run Redis in Docker
Start the Redis container using Docker:

```bash
docker run -d --name redis_container -p 6379:6379 redis
```
Alternatively, if using Docker Compose, the Redis container will be started automatically (see below).

### 6. Update Django Cache Settings for Redis
In settings.py, ensure the Redis cache settings are correct:

```python
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://localhost:6379/1',  # Use 'redis' if using Docker Compose
        'OPTIONS': {
            'CLIENT_CLASS': 'django_redis.client.DefaultClient',
        }
    }
}
```

### 7. Run Database Migrations

```bash
python manage.py migrate
```

### 8. Create a Superuser (for Admin Access)
```bash
python manage.py createsuperuser
Follow the prompts to create a superuser.
```

### 9. Run the Development Server
```bash
python manage.py runserver
```
The API will be available at http://127.0.0.1:8000/.

### 10. Access Django Admin
You can access the Django admin interface at http://127.0.0.1:8000/admin/ using the superuser credentials you created earlier.

## Using Docker Compose
You can use Docker Compose to manage both Redis and PostgreSQL containers along with Django.

### 1. Create a docker-compose.yml File
Here's an example docker-compose.yml:

```yaml
version: '3'

services:
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - redis
      - db

  redis:
    image: "redis:latest"
    ports:
      - "6379:6379"

  db:
    image: postgres
    environment:
      POSTGRES_DB: inventory_db
      POSTGRES_USER: inventory_user
      POSTGRES_PASSWORD: your_password
    ports:
      - "5432:5432"
```

### 2. Run Docker Compose
```bash
docker-compose up --build
```
This will start Django, Redis, and PostgreSQL in Docker containers.

## API Endpoints
- POST /items/: Create a new inventory item.
- GET /items/{item_id}/: Get details of a specific item.
- PUT /items/{item_id}/: Update an existing item.
- DELETE /items/{item_id}/: Delete an item.
- POST /auth/login/: User login to obtain JWT token.
- POST /auth/register/: Register a new user.

## Running Unit Tests
To run the unit tests:

```bash
python manage.py test
```

## Logging
Logs will be automatically generated based on your settings. You can customize the logging level and format in settings.py.

