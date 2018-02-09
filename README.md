# Django-Docker
Docker django base image

## 1. create a new project 
$ docker-compose run web django-admin.py startproject <project_name>

## 2. setup the project_name/settings.py (database connection postgres)
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'postgres',
        'USER': 'postgres',
        'HOST': 'db',
        'POST': 5432,
    }
}

```
## 3. setup the path in the docker-compose.yml where the manage.py is located

```python
version: '3'

services:
  db:
    image: postgres
  web:
    build: .
    command: python3 ./show_app/manage.py runserver 0.0.0.0:8000
    volumes: 
      - .:/code
    ports:
      - "8000:8000"
    depends_on:
      - db
```
## 4. Start the Django App
$ docker-compose up
