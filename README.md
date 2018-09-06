# Deploying Django to Heroku!


## Set up Heroku

Sign up for [Heroku](https://id.heroku.com/login)

Install the [heroku toolbelt](https://devcenter.heroku.com/articles/heroku-cli)

Open terminal in your project and login to your account. 

``` bash 
$ heroku login
Enter your Heroku credentials.
Email: {your heroku email}
Password (typing will be hidden):
Authentication successful.

```

## Preparing The Application

When your project is ready for deployment here are the key steps you have to take. 

- Add a procfile
- Add a Pipfile
- Install Gunicorn and Heroku DJango
- Add a runtime.txt to specify the correct Python version in project root
- Configure whitenoise to serve static files

### The Procfile 
___

Create a file named Procfile in the root with the following info..

```
web: gunicorn {name of your project folder}.wsgi --log-file -
```

### The Pipfile
___

Create a Pipfile with the following..

```
[[source]]

url = "https://pypi.python.org/simple"
verify_ssl = true


[packages]

django = "*"
gunicorn = "*"
django-heroku = "*"


[requires]

python_version = "3.6"

```
*With your versions inplace*

### The Runtime.txt
___

In terminal run..

```
python -V
``` 
this will give you your python version you are using for your project. 

Create a runtime.txt in the project root folder with the following info about your python project.

```
python-{your version number}

```

[Supported Python Runtimes](https://devcenter.heroku.com/articles/python-support#supported-runtimes)

### Install Gunicorn and Heroku Django

In your project folder run the following..

```
$ pip3 install django-heroku

```
This will install the helper addons for heroku. Now we must add it into our project. 
Add to your settings.py 

```
//after import os
import django_heroku

//at the bottom of the file
django_heroku.settings(locals())
```
Now to install gunicorn! 

```
$ pip3 install gunicorn

```
___

### Set up Static Assets
___

We need to configure the Static-related settings in settings.py. At the bottom of the file add...

``` python 
PROJECT_ROOT = os.path.dirname(os.path.abspath(__file__))

    
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
STATIC_URL = '/static/'

STATICFILES_DIRS = (os.path.join(PROJECT_ROOT, 'static')),


```

Install Whitenoise 

``` bash
$ pip3 install whitenoise
```

Add whitenoise to your Django settings.py extensions

```
MIDDLEWARE = [
	//previous middleware
    'whitenoise.middleware.WhiteNoiseMiddleware',
]

```

Now add to the bottom of settings.py...

``` python
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

```

Finally we have to add a folder for the static files to go. under your main project folder create a folder namned static.

## Time to Deploy! 

Inside your project folder run..

```
heroku create --buildpack heroku/python

```
This will take some time as heroku sets up your heroku remote git.

Once that is complete its time to add a Database to our heroku application.

``` bash
heroku addons:create heroku-postgresql:hobby-dev

```

Now you can git add and git commit.

Once that is complete you can now push to Heroku! 

``` bash
$ git push heroku master

```

Once that is completed we need to migrate our database just as we would on our own devices. 

```
$ heroku run python manage.py migrate

```

And viola! Your Django application is officially online! You can run the following command to view it. 

``` bash
$ heroku open

```

Happy Coding! 

![coder](https://media.giphy.com/media/ZVik7pBtu9dNS/giphy.gif)
