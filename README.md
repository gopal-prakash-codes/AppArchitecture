## Technology stack
The technology stack used includes:

- [`Python`](https://www.python.org) ver. 3.11
- [`Django`](https://www.djangoproject.com) ver. 4.2
- [`PostgreSQL`](https://www.postgresql.org) ver. 15
- [`Gunicorn`](https://gunicorn.org) ver. 22.0
- [`Traefik`](https://traefik.io/traefik/) ver. 2.9
- [`Caddy`](https://caddyserver.com) ver. 2.7 *(instead of Traefik if you wish)*

Nothing extra, only the essentials! You can easily add everything else yourself by expanding the existing configuration files:


> You can safely delete this application at any time. This application is present in the project as an example, used for testing and debugging.

So, what do you get by using this project as a template for your project? Let's take a look.

## Features
- Serving static files (and user-uploaded files) with Nginx
- Automatic database migration and static file collection when starting or restarting the Django container
- Automatic creation of the first user in Django with a default login and password
- Automatic creation and renewal of Let's Encrypt certificate 🔥
- Minimal dependencies
- Everything is set up as simply as possible - just a couple of commands in the terminal, and you have a working project 🚀

## How to use

### For development on your computer
```

2. Build the Docker container image with Django:
```console
docker build -t django-docker-template:master .
```

3. Create the first superuser:
```console
docker run -it --rm -v sqlite:/sqlite django-docker-template:master python manage.py createsuperuser
```

4. Run the Django development server inside the Django container:
```console
docker run -it --rm -p 8000:8000 -v sqlite:/sqlite -v $(pwd)/website:/usr/src/website django-docker-template:master python manage.py runserver 0.0.0.0:8000
```

<details markdown="1">
<summary>SQLite Usage Details</summary>

5. Run tests with pytest and coverage ✅:
```console
docker run --rm django-docker-template:master ./pytest.sh

```console
================== test session starts =====================================
platform linux -- Python 3.11.7, pytest-7.4.4, pluggy-1.3.0
django: version: 4.2.9, settings: website.settings (from ini)
rootdir: /usr/src/website
configfile: pytest.ini
plugins: django-4.7.0
collected 10 items

polls/tests.py .......... [100%]

================== 10 passed in 0.19s ======================================
Name                                       Stmts   Miss  Cover   Missing
------------------------------------------------------------------------
polls/__init__.py                              0      0   100%
polls/admin.py                                12      0   100%
polls/apps.py                                  4      0   100%
polls/migrations/0001_initial.py               6      0   100%
polls/migrations/0002_question_upload.py       4      0   100%
polls/migrations/__init__.py                   0      0   100%
polls/models.py                               20      2    90%   15, 33
polls/tests.py                                57      0   100%
polls/urls.py                                  4      0   100%
polls/views.py                                28      8    71%   39-58
website/__init__.py                            6      0   100%
website/settings.py                           52      2    96%   94, 197
website/urls.py                                6      0   100%
------------------------------------------------------------------------
TOTAL                                        199     12    94%
```

> If you don't want to use pytest (for some reason), you can run the tests without pytest using the command below:
```console
docker run --rm django-docker-template:master python manage.py test
```

6. Interactive shell with the Django project environment:
```console
docker run -it --rm -v sqlite:/sqlite django-docker-template:master python manage.py shell
```

7. Start all services locally (Postgres, Gunicorn, Traefik) using docker-compose:
```console
docker compose -f docker-compose.debug.yml up