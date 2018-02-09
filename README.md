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

## 5. Debugging

#### "host not allowed"
maybe you get an error message regarding the host, then you have to add the host in the setting.py under the point "ALLOWED_HOSTS =[]"

#### to migrate the database on the server
you can use the following command:
```
$ docker-compose exec web python manage.py makemigrations 
$ docker-compose exec web python manage.py migrate
```
or:

Using an entrypoint script
Another option is to use an entrypoint script and run the migration there, before the server is started. This is the way to go if you'd prefer things to be more automatic.

Dockerfile:
```python
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
```
entrypoint.sh:
```python
#!/bin/sh
python manage.py makemigrations
python manage.py migrate
exec "$@"
```
docker-compose.yml (under 'web'):
```
entrypoint: /entrypoint.sh
```
In this scenario, when the container starts, the entrypoint script will run, handle your migration, then hand off to the command (which in this case is Django runserver).


