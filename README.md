
# Django Vercel Deployment Template

A starter template for deploying your django application on Vercel (for free).


## Overview

The Django Vercel Deployment Template simplifies the process of deploying your Django projects to Vercel, offering a hassle-free experience to get your application up and running in no time. Leveraging the power of Vercel's serverless infrastructure, this template allows you to deploy your Django applications quickly and efficiently, all while taking advantage of Vercel's generous free tier.



## Prerequisites

Make sure you have the following requirements:
- [Git](https://git-scm.com/)
- [Python](https://www.python.org/downloads/)
- [Vercel account](https://vercel.com/)


## Quick Start
    
Create virtual environment
```bash
virtualenv venv

```

Activate virtual environment
```bash
venv\Scripts\activate
```

Install Django (Note: Vercel only supports Django 4)
```bash
pip install django==4.2.10
```

Install Whitenoise which will be used to serve static files (CSS, JS, Images, etc)
```bash
pip install whitenoise
```

Create a Django project
```bash
django-admin startproject your-project-name
```

Navigate into your Django project directory

```bash
cd path/to/your/project
```


## Vercel Configuration

you will need to add the following files to your project root directory

1. `build.sh`
    ```bash
    pip install -r requirements.txt
    python3 manage.py collectstatic --no-input

    ```

2. `vercel.json`
    ```bash
    {
    "builds": [
        {
        "src": "vercel_demo/wsgi.py",
        "use": "@vercel/python",
        "config": { "maxLambdaSize": "15mb", "runtime": "python3.9" }
        },
        {
        "src": "build.sh",
        "use": "@vercel/static-build",
        "config": { "distDir": "staticfiles" }
        }
    ],
    "routes": [
        {
        "src": "/(.*)",
        "dest": "vercel_demo/wsgi.py"
        },
        {
        "src": "/static/(.*)",
        "dest": "/static/$1"
        }
    ]
    }

    ```



## WSGI and Static Files Configuration

Browse into your project folder where you will find settings.py and wsgi.py files.

**Note:** make sure to change your-project-name to your actual project name



1. `wsgi.py`
    ```bash
    import os

    from django.core.wsgi import get_wsgi_application

    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'your-project-name.settings')

    application = get_wsgi_application()

    app = application
    ```


2. `settings.py`

    Add this import at the top.

    ```bash
    import os
    ```

    Adding '.vercel.com' in order to host our project.

    ```bash
    ALLOWED_HOSTS = ['.vercel.app', '127.0.0.1']
    ```

    Adding 'whitenoise' as a middlware in order to serve the staticfiles.

    ```bash
    MIDDLEWARE = [
    'whitenoise.middleware.WhiteNoiseMiddleware',
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    ]
    ```

    update wsgi from '.application' to '.app'

    ```bash
    WSGI_APPLICATION = 'your-project-name.wsgi.app'
    ```

    removing database configuration. Since we are not utilizing database as of now. 

    ```bash
    DATABASES = {}
    ```

    static files configuration

    ```bash
    STATIC_URL = 'static/'

    STATICFILES_DIRS = [
        BASE_DIR/ 'static'
    ]

    STORAGES = {
        "staticfiles": {
            "BACKEND": "whitenoise.storage.CompressedStaticFilesStorage", 
        },
    }

    STATIC_ROOT = os.path.join(BASE_DIR, "staticfiles")
    ```







## Dependencies

Before deploying we need another file 'requirements.txt' so that vercel knows what packages need to be installed.


Run the following command which will create a requirements file with the necessary dependencies.
```bash
pip freeze > requirements.txt
```


## Run on Local Server

Now before moving on let's check if everything is working well by running the local server.

```bash
python manage.py runserver
```

If you are not encountering any error then you should be able to see this page on [localhost](http://localhost:8000).

![App Screenshot](https://i.ibb.co/KXLdTkK/localhost.png)


## Git Setup

Create a git repository and push the code to your remote [github](https://github.com) repository


```bash
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin your-github-repository-https-url
git push -u origin main
```


## Vercel Deployment

Go to your [vercel](https://vercel.com) account and add a new project

![App Screenshot](https://i.ibb.co/NKYQq9P/add-new-project.png)

<br>
<br>

You will have to first give access to your gihub account/repositories

Now import the repository.

![App Screenshot](https://i.ibb.co/qWMG6Pd/import-repository.png)


<br>
<br>



After importing, configure your project name and hit the deploy button.

![App Screenshot](https://i.ibb.co/2jG20Pm/configure-deployment.png)

<br>
<br>


**Note:** you may face deployment error saying "Couldn't import Django".

![App Screenshot](https://i.ibb.co/JpbTbWG/deployment-error.png)

<br>
<br>


To fix this error, go to your project settings and change the `Node.js Version` from `20.x` to `18.x`
since, vercel requires node version 18.x to run django.

![App Screenshot](https://i.ibb.co/1M4MXKR/select-node-version-18x.png)

<br>
<br>


Go back to your project deployments tab and click on `redeploy`

Congratulation! Your project is now live. To visit the website click on `visit` button.

![App Screenshot](https://i.ibb.co/7XWP4g1/visit-website.png)

<br>
<br>


**Note:** you can change your website domain from `project settings --> domain`

Happy Deployment! 👏












