# django-heroku with PIPENV
Minimum configuration to host a Django project on Heroku with pipenv

# Create the project directory
* `mkdir directory_name && cd directory_name`

## Create and activate your pipenv
### Installation pipenv :
 
 * If you're on MacOS, you can install Pipenv easily with Homebrew:
    ``` 
    $ brew install pipenv 
    ```
 * Or, if you're using Debian Buster+:
    ```
    $ sudo apt install pipenv
    ```
* Or, if you're using Fedora 28:
    ```
    $ sudo dnf install pipenv
    ```
* Or, if you're using FreeBSD:
    ```
    # pkg install py36-pipenv
    ```
[link for more information about pipenv](https://github.com/pypa/pipenv)
 
* `python3.7 -m venv .venv && . .venv/bin/activate`

 ## Installing django
 * `pipenv install django`

## Create the django project
* `django-admin startproject myproject`

## Creating the Git repository
* `git init`

* Create a file called `.gitignore` with the following content:

```
# See the name for you IDE
.idea
# If you are using sqlite3
*.sqlite3
# Name of your virtuan env
.venv
*pyc
```
* `git add .`
* `git commit -m 'First commit'`

## Hidding instance configuration
* `pipenv --help`
* `pipenv install python-decouple`

* Create an .env file at the root path and insert the following variables
    ```
    - SECRET_KEY=Your$eCretKeyHere (Get this secrety key from the settings.py)
    - DEBUG=False
    ```
### settings.py
```
 from decouple import config
 SECRET_KEY = config('SECRET_KEY')
 DEBUG = config('DEBUG', default=False, cast=bool)
```

## Configuring the Data Base
* `pipenv install dj-database-url`

### settings.py
```
 from dj_database_url import parse as dburl

 default_dburl = 'sqlite:///' + os.path.join(BASE_DIR, 'db.sqlite3')

 DATABASES = {
    'default': config('DATABASE_URL', default=default_dburl, cast=dburl),
 }
 ```

## Static files 
* `pipenv install dj-static`

### wsgi.py
```
 from dj_static import Cling
 application = Cling(get_wsgi_application())
```
### settings.py
```
 STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```
## Add two more dependencies in the package:
* `pipenv install gunicorn`
* `pipenv install psycopg2`

## Create a file Procfile and add the following code
```
 web: gunicorn website.wsgi --log-file -
```
## Creating the app at Heroku
You should install heroku CLI tools in your computer previously ( See http://bit.ly/2jCgJYW ) 
* heroku apps:create app-name
Remember to grab the address of the app in this point

## Setting the allowed hosts
* include your address at the ALLOWED_HOSTS directives in settings.py - Just the domain, make sure that you will take the protocol and slashes from the string

## Heroku install config plugin
* `heroku plugins:install heroku-config`

### Sending configs from .env to Heroku ( You have to be inside tha folther where .env files is)
* `heroku config:push -a` or `heroku config:push`

### To show heroku configs do
* `heroku config` 

## Publishing the app
* `git add .`
* `git commit -m 'Configuring the app'`
* `git push heroku master --force`

## Creating the data base
* `heroku run python manage.py migrate`

## Creating the Django admin user
* `heroku run python manage.py createsuperuser`

## EXTRAS
### You may need to disable the collectstatic
* `heroku config:set DISABLE_COLLECTSTATIC=1`

### Changing a specific configuration
* `heroku config:set DEBUG=True`
