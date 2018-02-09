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
## 3. Start the Django App
$ docker-compose up
