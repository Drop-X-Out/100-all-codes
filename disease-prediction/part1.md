# How to create a Disease Prediction Application using React, Django and PostgreSQL

This application will be built using `ReactJs` & `TailwindCSS` on the frontend and `Django` `PostgreSQL` on the backend. We will using `SVM` Machine Learning Model with a Dataset to predict diseases

## Setting up the Environment

The root directory will be the `server` directory which would contain the `frontend` directory. 

To get started, open up your terminal and paste the following command

```jsx
django-admin startproject my-project
```

This will setup your directory like the below tree

```bash
my-project/
├──my-project/
├──manage.py
```

Rename the inner `my-project` directory to `Backend`. This will serve as the root for our backend application.

Now we will build the Accounts Directory and Disease Predictor directory. Open your terminal and run the following commands

```jsx
python3 manage.py startapp Accounts
```

```jsx
python3 manage.py startapp DiseasePredictor
```

Create `.gitignore , mode.pkl and requirements.txt` file in the root  like the below tree.

```jsx
Application/
├── Accounts/
├── Backend/
├── DiseasePredictor/
├── frontend/ ( will create it using vite)
├── .gitignore
├── [manage.py](http://manage.py/)
├── model.pkl
├── [readme.md](http://readme.md/)
└── requirements.txt
```

`.gitingore`

```bash
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# C extensions
*.so

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
share/python-wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# PyInstaller
#  Usually these files are written by a python script from a template
#  before PyInstaller builds the exe, so as to inject date/other infos into it.
*.manifest
*.spec

# Installer logs
pip-log.txt
pip-delete-this-directory.txt

# Unit test / coverage reports
htmlcov/
.tox/
.nox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
*.py,cover
.hypothesis/
.pytest_cache/
cover/

# Translations
*.mo
*.pot

# Django stuff:
*.log
local_settings.py
db.sqlite3
db.sqlite3-journal

# Flask stuff:
instance/
.webassets-cache

# Scrapy stuff:
.scrapy

# Sphinx documentation
docs/_build/

# PyBuilder
.pybuilder/
target/

# Jupyter Notebook
.ipynb_checkpoints

# IPython
profile_default/
ipython_config.py

# pyenv
#   For a library or package, you might want to ignore these files since the code is
#   intended to run in multiple environments; otherwise, check them in:
# .python-version

# pipenv
#   According to pypa/pipenv#598, it is recommended to include Pipfile.lock in version control.
#   However, in case of collaboration, if having platform-specific dependencies or dependencies
#   having no cross-platform support, pipenv may install dependencies that don't work, or not
#   install all needed dependencies.
#Pipfile.lock

# poetry
#   Similar to Pipfile.lock, it is generally recommended to include poetry.lock in version control.
#   This is especially recommended for binary packages to ensure reproducibility, and is more
#   commonly ignored for libraries.
#   https://python-poetry.org/docs/basic-usage/#commit-your-poetrylock-file-to-version-control
#poetry.lock

# pdm
#   Similar to Pipfile.lock, it is generally recommended to include pdm.lock in version control.
#pdm.lock
#   pdm stores project-wide configurations in .pdm.toml, but it is recommended to not include it
#   in version control.
#   https://pdm.fming.dev/#use-with-ide
.pdm.toml

# PEP 582; used by e.g. github.com/David-OConnor/pyflow and github.com/pdm-project/pdm
__pypackages__/

# Celery stuff
celerybeat-schedule
celerybeat.pid

# SageMath parsed files
*.sage.py

# Environments
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# Spyder project settings
.spyderproject
.spyproject

# Rope project settings
.ropeproject

# mkdocs documentation
/site

# mypy
.mypy_cache/
.dmypy.json
dmypy.json

# Pyre type checker
.pyre/

# pytype static type analyzer
.pytype/

# Cython debug symbols
cython_debug/

# PyCharm
#  JetBrains specific template is maintained in a separate JetBrains.gitignore that can
#  be found at https://github.com/github/gitignore/blob/main/Global/JetBrains.gitignore
#  and can be added to the global gitignore or merged into this file.  For a more nuclear
#  option (not recommended) you can uncomment the following to ignore the entire idea folder.
#.idea/
# Ignore Django Migrations in Development if you are working on team

# Only for Development only
**/migrations/**
!**/migrations
!**/migrations/__init__.py
```

`requirements.txt`

```bash
django
django-cors-headers
djangorestframework
numpy
scikit-learn
python-decouple
pandas
django-pandas
imbalanced-learn
scikit-learn
psycopg2-binary
python-dotenv
pickle5
```

`*model.pkl`file for Disease Prediction Model*

[model.pkl](model.pkl)

## Setting up the Frontend of the App

Execute the command, open a new terminal and follow these instructions:

- Enter "frontend" when prompted for the Project name
- Select React as the framework and JavaScript as the variant

```bash
npm create vite@latest

✔ Project name: … frontend
✔ Select a framework: › React
✔ Select a variant: › JavaScript
```

This will create the frontend directory in your root `frontend` folder. Run the following commands after it.

```bash
cd application
npm install
npm run dev
```

This will install the initial dependencies and start the development server of Vite running on `localhost:5173`. Open your browser and write the `localhost` URL to see the development server.

> 
> 

### Setting up the Frontend Directory

You can also setup the components directory outside the assets directory if you prefer that way.

```jsx
frontend/
├── src/
│   ├── assets/
│     ├── components/
│     └── img/
│   ├── App.css
│   ├── App.jsx
│   ├── index.css
│   └── main.jsx
│
├── public/
├── .gitignore
├── index.html
├── package-lock.json
├── package.json
├── postcss.config.js
├── tailwind.config.js
└── vite.config.js
```

### Installing the Dependencies

```bash
npm install @emotion/react @emotion/styled @mui/material @tanstack/react-table axios bootstrap chart.js jwt-decode react react-bootstrap react-chartjs-2 react-dom react-router-dom uuid

npm install --save-dev @types/react @types/react-dom @vitejs/plugin-react autoprefixer postcss tailwindcss vite
```

> If you get any versioning error make sure your version match to the below `package.json`
> 

```jsx
{
  "name": "getogether-frontend",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite --host",
    "build": "tsc && vite build",
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview",
    "format": "prettier --write ."
  },
  "dependencies": {
    "@emailjs/browser": "^4.3.3",
    "@emotion/react": "^11.11.3",
    "@emotion/styled": "^11.11.0",
    "@mui/icons-material": "^5.15.4",
    "@mui/material": "^5.15.4",
    "@mui/styles": "^5.15.18",
    "@mui/x-data-grid": "^7.11.0",
    "@mui/x-date-pickers": "^7.5.0",
    "@radix-ui/react-accordion": "^1.1.2",
    "@radix-ui/react-avatar": "^1.0.4",
    "@radix-ui/react-checkbox": "^1.0.4",
    "@radix-ui/react-dialog": "^1.0.5",
    "@radix-ui/react-dropdown-menu": "^2.0.6",
    "@radix-ui/react-icons": "^1.3.0",
    "@radix-ui/react-label": "^2.0.2",
    "@radix-ui/react-popover": "^1.0.7",
    "@radix-ui/react-select": "^2.0.0",
    "@radix-ui/react-separator": "^1.0.3",
    "@radix-ui/react-slot": "^1.0.2",
    "@radix-ui/react-tabs": "^1.0.4",
    "@reduxjs/toolkit": "^2.0.1",
    "axios": "^1.6.7",
    "chart.js": "^4.4.3",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "date-fns": "^3.6.0",
    "file-saver": "^2.0.5",
    "lodash": "^4.17.21",
    "lucide-react": "^0.379.0",
    "mui-one-time-password-input": "^2.0.2",
    "react": "^17.0.0 || ^18.0.0",
    "react-chartjs-2": "^5.2.0",
    "react-confirm": "^0.3.0-7",
    "react-date-range": "^2.0.1",
    "react-day-picker": "^8.10.1",
    "react-dom": "^17.0.0 || ^18.0.0",
    "react-hot-toast": "^2.4.1",
    "react-icons": "^5.0.1",
    "react-redux": "^9.1.0",
    "react-router-dom": "^6.21.2",
    "react-spreadsheet-import": "^4.6.1",
    "recharts": "^2.12.7",
    "socket.io-client": "^4.7.5",
    "tailwind-merge": "^2.3.0",
    "tailwindcss-animate": "^1.0.7",
    "vaul": "^0.9.1"
  },
  "devDependencies": {
    "@types/file-saver": "^2.0.7",
    "@types/node": "^20.12.12",
    "@types/react": "^18.2.43",
    "@types/react-dom": "^18.2.17",
    "@typescript-eslint/eslint-plugin": "^6.14.0",
    "@typescript-eslint/parser": "^6.14.0",
    "@vitejs/plugin-react": "^4.2.1",
    "autoprefixer": "^10.4.16",
    "eslint": "^8.55.0",
    "eslint-plugin-react-hooks": "^4.6.0",
    "eslint-plugin-react-refresh": "^0.4.5",
    "postcss": "^8.4.33",
    "prettier": "^3.2.5",
    "tailwindcss": "^3.4.1",
    "typescript": "^5.2.2",
    "vite": "^5.0.8"
  },
  "peerDependencies": {
    "react": "^17.0.0 || ^18.0.0",
    "react-dom": "^17.0.0 || ^18.0.0"
  },
  "prettier": {
    "semi": false,
    "singleQuote": false,
    "trailingComma": "all",
    "jsxSingleQuote": false,
    "tabWidth": 2
  }
}

```

## Building the Server of the App

### Installing Dependencies

We will now start with the server of the application, run the following command in your terminal to install the dependencies

```bash
pip install django django-cors-headers djangorestframework numpy scikit-learn python-decouple pandas django-pandas imbalanced-learn scikit-learn psycopg2-binary python-dotenv pickle5
```

### Setting up Backend Directory

This is the root of the server part. Setup the `Backend` directory according to the following component tree

```bash
Backend/
├── __init__.py
├── asgi.py
├── settings.py
├── urls.py
├── views.py
└── wsgi.py
```

`__init__.py, asgi.py` and `wsgi.py` are set by default and doesn’t needs to be changed. We will start with the other files.

### Backend Components

Paste the following code inside the mentioned file

`settings.py` - Index file for server

```python
"""
Django settings for Backend project.

Generated by 'django-admin startproject' using Django 4.1.1.

For more information on this file, see
https://docs.djangoproject.com/en/4.1/topics/settings/

For the full list of settings and their values, see
https://docs.djangoproject.com/en/4.1/ref/settings/
"""

from pathlib import Path
from decouple import config
from dotenv import load_dotenv
import os

# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent

# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/4.1/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'django-insecure-_7_br&7lg2-ndm@osr=x$0#gvs=j^29+i4*j8!d00-)wafeemo'
# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = ['*']

CSRF_COOKIE_NAME = 'csrftoken'

CSRF_TRUSTED_ORIGINS = ['http://127.0.0.1:5173',]

CORS_ALLOW_ALL_ORIGINS = True

CORS_ALLOW_CREDENTIALS = True

script_dir = os.path.dirname(os.path.abspath(__file__))

# Get the absolute path of the root directory
root_dir = os.path.abspath(os.path.join(script_dir, '..'))

# Construct the absolute file path of .env
env_file_path = os.path.join(root_dir, '.env')

# Load environment variables from .env file
load_dotenv(env_file_path)

# Access environment variables
DATABASE_NAME = os.getenv('DATABASE_NAME')
USER = os.getenv('USER')
PASS = os.getenv('PASS')

SECURE_CROSS_ORIGIN_OPENER_POLICY = "same-origin-allow-popups"

# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'corsheaders',
    'Accounts',
    'DiseasePredictor',
]

MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'Backend.urls'

import os

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'frontend/dist')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'Backend.wsgi.application'

# Database
# https://docs.djangoproject.com/en/4.1/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': DATABASE_NAME,
        'USER': USER,
        'PASSWORD': PASS,
        'HOST': 'localhost',
        'PORT': 5432,
    }
}

AUTH_USER_MODEL = 'Accounts.AppUser'

REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework.authentication.SessionAuthentication',
    ),
}

# Password validation
# https://docs.djangoproject.com/en/4.1/ref/settings/#auth-password-validators

AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]

# Internationalization
# https://docs.djangoproject.com/en/4.1/topics/i18n/

LANGUAGE_CODE = 'en-us'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_TZ = True

# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/4.1/howto/static-files/

STATIC_URL = 'assets/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'frontend/dist/assets')
]

# Default primary key field type
# https://docs.djangoproject.com/en/4.1/ref/settings/#default-auto-field

DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
```

`urls.py` - Routes for the server

```python
from django.contrib import admin
from django.urls import path, include, re_path
from .views import index, load_icon
from django.views.generic.base import RedirectView
from django.contrib.staticfiles.storage import staticfiles_storage

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', index),
    path("", include("Accounts.urls")),
    path("", include("DiseasePredictor.urls")),
    path("contactdoctor", index),
    path("dashboard", index),
    path('icon.svg', load_icon),
]

```

`views.py`

```python
from django.shortcuts import render
from django.http import FileResponse
from django.conf import settings
import os

def index(request):
    return render(request, 'index.html')

def load_icon(request):
    icon_path = os.path.join(settings.BASE_DIR, 'frontend/dist/icon.svg')
    return FileResponse(open(icon_path, 'rb'), content_type='image/svg+xml')
```

### Accounts Directory

Setup the accounts directory as the below ASCII tree and paste the code mentioned along the following file.

```python
Accounts/
├── migrations/
├── __init__.py
├── admin.py
├── apps.py
├── models.py
├── serializers.py
├── tests.py
├── urls.py
├── validations.py
└── views.py
```

`admin.py`

```python
from django.contrib import admin
from .models import AppUser, PatientProfile, DoctorProfile, symptoms_diseases

admin.site.register(AppUser)
admin.site.register(PatientProfile)
admin.site.register(DoctorProfile)
admin.site.register(symptoms_diseases)

# Register your models here.
```

`models.py`

```python
from django.db import models
from django.contrib.auth.base_user import BaseUserManager
from django.contrib.auth.models import AbstractBaseUser, PermissionsMixin
from django.contrib.postgres.fields import ArrayField
from django.db.models import JSONField

class AppUserManager(BaseUserManager):
    def create_user(self, email, password=None):
        if not email:
            raise ValueError('An email is required.')
        if not password:
            raise ValueError('A password is required.')
        email = self.normalize_email(email)
        user = self.model(email=email)
        user.set_password(password)
        user.save()
        # import PatientProfile here to avoid circular import
        from .models import PatientProfile
        profile = PatientProfile.objects.create(user=user)
        
        return user

    def create_superuser(self, email, password=None):
        if not email:
            raise ValueError('An email is required.')
        if not password:
            raise ValueError('A password is required.')
        user = self.create_user(email, password)
        user.is_staff = True
        user.is_superuser = True
        user.save()
        return user

class AppUser(AbstractBaseUser, PermissionsMixin):
    user_id = models.AutoField(primary_key=True)
    email = models.EmailField(max_length=50, unique=True)
    username = models.CharField(max_length=50)
    is_staff = models.BooleanField(default=False)
    is_superuser = models.BooleanField(default=False)

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = []
    objects = AppUserManager()

    def __str__(self):
        return self.username

class PatientProfile(models.Model):
    user = models.OneToOneField(AppUser, on_delete=models.CASCADE, related_name='profile')
    age = models.IntegerField(default=0)
    sex = models.CharField(max_length=20, default='Not to say')
    first_name = models.CharField(max_length=20, default='a')
    last_name = models.CharField(max_length=20, default='a')
    medical_history = ArrayField(models.CharField(max_length=200), blank=True, default=list)
    dob_day = models.IntegerField(default=0)
    dob_month = models.IntegerField(default=0)
    dob_year = models.IntegerField(default=0)
    height = models.IntegerField(default=0)
    weight = models.IntegerField(default=0)
    current_med = ArrayField(models.CharField(max_length=200), blank=True, default=list)
    exercise = models.CharField(max_length=200, default='no exercise')
    diet = models.CharField(max_length=200, default='no diet')
    smoke_cons  = models.CharField(max_length=200, default='no smoke')
    alcohol_cons = models.CharField(max_length=200, default='no alcohol')
    bp_log = JSONField(default=dict)
    blood_glucose = JSONField(default=dict)
    new_patient = models.BooleanField(default=True)

class DoctorProfile(models.Model):
    name = models.CharField(max_length=200, default='NA')
    speciality = models.CharField(max_length=200, default='NA')
    sex = models.CharField(max_length=200, default='NA')
    experience = models.IntegerField(default=0)
    work_address = models.CharField(max_length=200, default='NA')
    mobile_no = models.CharField(max_length=200, default='0000000000')
    image_link = models.URLField(max_length=200)
    profile_link = models.URLField(max_length=200)

class symptoms_diseases(models.Model):
    itching = models.IntegerField()
    skin_rash = models.IntegerField()
    shivering = models.IntegerField()
    chills = models.IntegerField()
    joint_pain = models.IntegerField()
    stomach_pain = models.IntegerField()
    acidity = models.IntegerField()
    ulcers_on_tongue = models.IntegerField()
    muscle_wasting = models.IntegerField()
    vomiting = models.IntegerField()
    burning_micturition = models.IntegerField()
    spotting_urination = models.IntegerField()
    fatigue = models.IntegerField()
    weight_gain = models.IntegerField()
    anxiety = models.IntegerField()
    cold_hands_and_feets = models.IntegerField()
    mood_swings = models.IntegerField()
    weight_loss = models.IntegerField()
    restlessness = models.IntegerField()
    lethargy = models.IntegerField()
    patches_in_throat = models.IntegerField()
    irregular_sugar_level = models.IntegerField()
    cough = models.IntegerField()
    high_fever = models.IntegerField()
    sunken_eyes = models.IntegerField()
    breathlessness = models.IntegerField()
    sweating = models.IntegerField()
    dehydration = models.IntegerField()
    indigestion = models.IntegerField()
    headache = models.IntegerField()
    yellowish_skin = models.IntegerField()
    dark_urine = models.IntegerField()
    nausea = models.IntegerField()
    loss_of_appetite = models.IntegerField()
    pain_behind_the_eyes = models.IntegerField()
    back_pain = models.IntegerField()
    constipation = models.IntegerField()
    abdominal_pain = models.IntegerField()
    diarrhoea = models.IntegerField()
    mild_fever = models.IntegerField()
    yellow_urine = models.IntegerField()
    yellowing_of_eyes = models.IntegerField()
    acute_liver_failure = models.IntegerField()
    fluid_overload = models.IntegerField()
    swelling_of_stomach = models.IntegerField()
    swelled_lymph_nodes = models.IntegerField()
    malaise = models.IntegerField()
    blurred_and_distorted_vision = models.IntegerField()	
    phlegm = models.IntegerField()	
    throat_irritation = models.IntegerField()	
    redness_of_eyes  = models.IntegerField()	
    sinus_pressure = models.IntegerField()	
    runny_nose = models.IntegerField()	
    congestion = models.IntegerField()	
    chest_pain = models.IntegerField() 	
    weakness_in_limbs = models.IntegerField()	
    fast_heart_rate = models.IntegerField()	
    pain_during_bowel_movements = models.IntegerField()	
    pain_in_anal_region = models.IntegerField()	
    bloody_stool = models.IntegerField()	
    irritation_in_anus = models.IntegerField()	
    neck_pain = models.IntegerField()	
    dizziness = models.IntegerField()	
    cramps = models.IntegerField()	
    bruising = models.IntegerField()	
    obesity = models.IntegerField()	
    swollen_legs = models.IntegerField()	
    swollen_blood_vessels = models.IntegerField()	
    puffy_face_and_eyes = models.IntegerField()	
    enlarged_thyroid = models.IntegerField()	
    brittle_nails = models.IntegerField()	
    swollen_extremeties = models.IntegerField()	
    excessive_hunger = models.IntegerField()	
    extra_marital_contacts = models.IntegerField()	
    drying_and_tingling_lips = models.IntegerField()	
    slurred_speech = models.IntegerField()	
    knee_pain = models.IntegerField()	
    hip_joint_pain = models.IntegerField()	
    muscle_weakness = models.IntegerField()	
    stiff_neck = models.IntegerField()	
    swelling_joints = models.IntegerField()	
    movement_stiffness = models.IntegerField()	
    spinning_movements = models.IntegerField()	
    loss_of_balance = models.IntegerField()
    unsteadiness = models.IntegerField()
    weakness_of_one_body_side = models.IntegerField()
    loss_of_smell = models.IntegerField()
    bladder_discomfort = models.IntegerField()
    foul_smell_of_urine = models.IntegerField()
    continuous_feel_of_urine = models.IntegerField()
    passage_of_gases = models.IntegerField()
    internal_itching = models.IntegerField()
    toxic_look = models.IntegerField()
    depression = models.IntegerField()
    irritability = models.IntegerField()
    muscle_pain = models.IntegerField()
    altered_sensorium = models.IntegerField()
    red_spots_over_body = models.IntegerField()
    belly_pain = models.IntegerField()
    abnormal_menstruation = models.IntegerField()
    dischromic_patches = models.IntegerField()
    watering_from_eyes = models.IntegerField()
    increased_appetite = models.IntegerField()
    polyuria = models.IntegerField()
    family_history = models.IntegerField()
    mucoid_sputum = models.IntegerField()
    rusty_sputum = models.IntegerField()
    lack_of_concentration = models.IntegerField()
    visual_disturbances = models.IntegerField()
    receiving_blood_transfusion = models.IntegerField()
    receiving_unsterile_injections = models.IntegerField()
    coma = models.IntegerField()
    stomach_bleeding = models.IntegerField()
    distention_of_abdomen = models.IntegerField()
    history_of_alcohol_consumption = models.IntegerField()
    blood_in_sputum = models.IntegerField()
    prominent_veins_on_calf = models.IntegerField()
    palpitations = models.IntegerField()
    painful_walking = models.IntegerField()
    pus_filled_pimples = models.IntegerField()
    blackheads = models.IntegerField()
    scurring = models.IntegerField()
    skin_peeling = models.IntegerField()
    silver_like_dusting = models.IntegerField()
    small_dents_in_nails = models.IntegerField()
    inflammatory_nails = models.IntegerField()
    blister = models.IntegerField()
    red_sore_around_nose = models.IntegerField()
    yellow_crust_ooze = models.IntegerField()
    prognosis = models.CharField(max_length=100)

    class Meta:
        db_table = 'symptoms_diseases'

class Predicted_Diseases(models.Model) :
    diseases = ArrayField(models.CharField(max_length=200), blank=True, default=list)
    diseases_prob = ArrayField(models.FloatField(default=0), blank=True, default=list)
    consult_doctor = models.CharField(max_length=100, default="")
```

`serializers.py`

```python
from django.core.exceptions import ValidationError
from rest_framework import serializers
from django.contrib.auth import get_user_model, authenticate
from .models import PatientProfile, Predicted_Diseases, DoctorProfile

UserModel = get_user_model()

class UserRegisterSerializer(serializers.ModelSerializer):
    class Meta:
        model = UserModel
        fields = '__all__'
    def create(self, clean_data):
        user_obj = UserModel.objects.create_user(email=clean_data['email'], password=clean_data['password'])
        user_obj.username = clean_data['username']
        user_obj.save()
        return user_obj

class UserLoginSerializer(serializers.Serializer):
    email = serializers.EmailField()
    password = serializers.CharField()
    ##
    def check_user(self, clean_data):
        user = authenticate(username=clean_data['email'], password=clean_data['password'])
        if not user:
            raise ValidationError('user not found')
        return user

class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = UserModel
        fields = ('email', 'username')

class PatientSerializer(serializers.ModelSerializer):
    class Meta:
        model = PatientProfile
        fields = ('__all__')

    def create(self, validated_data):
        user = self.context['request'].user
        profile = PatientProfile.objects.create(user=user, **validated_data)
        return profile

    def update(self, instance, validated_data):
        for attr, value in validated_data.items():
            setattr(instance, attr, value)
        instance.save()
        return instance

class PredictionSerializer(serializers.ModelSerializer):
    class Meta:
        model = Predicted_Diseases
        fields = ('diseases', 'diseases_prob', 'consult_doctor')

class DoctorProfileSerializer(serializers.ModelSerializer):
    class Meta:
        model = DoctorProfile
        fields = '__all__'
```

`urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('register', views.UserRegister.as_view(), name='register'),
    path('login', views.UserLogin.as_view(), name='login'),
    path('logout', views.UserLogout.as_view(), name='logout'),
    path('user', views.UserView.as_view(), name='user'),
    path('patient', views.PatientProfile.as_view(), name='patient'),
    path('doctor/<str:sp>/', views.DoctorProfileListAPIView.as_view(), name='doctor'),
    path('insert', views.insert_data, name='data'),
    path('check_email', views.check_email, name='check_email'),
    path('check_admin', views.check_admin, name='check_admin'),
]
```

`validations.py`

```python
from django.core.exceptions import ValidationError
from django.contrib.auth import get_user_model
UserModel = get_user_model()

def custom_validation(data):
    email = data['email'].strip()
    username = data['username'].strip()
    password = data['password'].strip()
    ##
    if not email or UserModel.objects.filter(email=email).exists():
        print(UserModel.objects.filter(email=email).exists())
        raise ValidationError('choose another email')
    ##
    if not password or len(password) < 8:
        raise ValidationError('choose another password, min 8 characters')
    ##
    if not username:
        raise ValidationError('choose another username')
    return data

def validate_email(data):
    email = data['email'].strip()
    if not email:
        raise ValidationError('an email is needed')
    return True

def validate_username(data):
    username = data['username'].strip()
    if not username:
        raise ValidationError('choose another username')
    return True

def validate_password(data):
    password = data['password'].strip()
    if not password:
        raise ValidationError('a password is needed')
    return True
```

`views.py`

```python
from django.db import connection
from django.shortcuts import render

# Create your views here.
from django.contrib.auth import get_user_model, login, logout
from rest_framework.authentication import SessionAuthentication
from rest_framework.views import APIView
from rest_framework.response import Response
from .serializers import UserRegisterSerializer, UserLoginSerializer, PatientSerializer, UserSerializer, DoctorProfileSerializer
from rest_framework import permissions, status, generics
from .validations import custom_validation, validate_email, validate_password
from .models import DoctorProfile, AppUser
from django.http import JsonResponse

class UserRegister(APIView):
    permission_classes = (permissions.AllowAny,)

    def post(self, request):
        clean_data = custom_validation(request.data)
        serializer = UserRegisterSerializer(data=clean_data)
        if serializer.is_valid(raise_exception=True):
            user = serializer.create(clean_data)
            if user:
                return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(status=status.HTTP_400_BAD_REQUEST)

class UserLogin(APIView):
    permission_classes = (permissions.AllowAny,)
    authentication_classes = (SessionAuthentication,)
    ##

    def post(self, request):
        data = request.data
        assert validate_email(data)
        assert validate_password(data)
        serializer = UserLoginSerializer(data=data)
        if serializer.is_valid(raise_exception=True):
            user = serializer.check_user(data)
            login(request, user)
            return Response(serializer.data, status=status.HTTP_200_OK)

class UserLogout(APIView):
    permission_classes = (permissions.AllowAny,)
    authentication_classes = ()

    def post(self, request):
        logout(request)
        return Response(status=status.HTTP_200_OK)

class UserView(APIView):
    permission_classes = (permissions.IsAuthenticated,)
    authentication_classes = (SessionAuthentication,)
    ##

    def get(self, request):
        serializer = UserSerializer(request.user)
        return Response({'user': serializer.data}, status=status.HTTP_200_OK)

def check_email(request):
    email = request.GET.get('email')
    if email:
        email_exists = AppUser.objects.filter(email=email).exists()
        response_data = {'email_exists': email_exists}
        return JsonResponse(response_data)
    else:
        response_data = {'error': 'Email parameter is missing'}
        return JsonResponse(response_data, status=400)

class PatientProfile(APIView):
    permission_classes = (permissions.IsAuthenticated,)
    authentication_classes = (SessionAuthentication,)

    def get(self, request):
        profile = request.user.profile

        if not profile:
            return Response({'error': 'User does not have a profile'}, status=status.HTTP_400_BAD_REQUEST)

        serializer = PatientSerializer(profile)
        return Response(serializer.data, status=status.HTTP_200_OK)

    def put(self, request):
        serializer = PatientSerializer(
            request.user.profile, data=request.data, partial=True)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_200_OK)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

class DoctorProfileListAPIView(generics.ListAPIView):
    serializer_class = DoctorProfileSerializer

    def get_queryset(self):
        speciality = self.kwargs.get('sp', '')
        if speciality == 'All':
            queryset = DoctorProfile.objects.all()
        else:
            queryset = DoctorProfile.objects.filter(speciality__icontains=speciality)
        
        queryset = queryset.order_by('?')[:12]
        
        return queryset

def insert_data(request):
    query = """
        INSERT INTO "Accounts_doctorprofile" (name, speciality, sex, experience, work_address, mobile_no, image_link, profile_link)
        VALUES (%s, %s, %s, %s, %s, %s, %s, %s);
    """
    values = [
    # Family Medicine
    ('Dr. John Smith', 'Family Medicine', 'male', 15, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-john-smith'),
    ('Dr. Emma Thompson', 'Family Medicine', 'female', 12, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-emma-thompson'),
    ('Dr. David Wilson', 'Family Medicine', 'male', 10, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-david-wilson'),
    ('Dr. Lily Rodriguez', 'Family Medicine', 'female', 8, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-lily-rodriguez'),
    

    # Internal Medicine
    ('Dr. Emily Johnson', 'Internal Medicine', 'female', 20, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-emily-johnson'),
    ('Dr. Benjamin Davis', 'Internal Medicine', 'male', 18, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-benjamin-davis'),
    ('Dr. Sophia Thompson', 'Internal Medicine', 'female', 22, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-sophia-thompson'),
    ('Dr. Daniel Wilson', 'Internal Medicine', 'male', 16, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-daniel-wilson'),
    

    # Pediatrician
    ('Dr. Michael Johnson', 'Pediatrician', 'male', 18, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-michael-johnson'),
    ('Dr. Abigail Smith', 'Pediatrician', 'female', 15, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-abigail-smith'),
    ('Dr. Ethan Davis', 'Pediatrician', 'male', 10, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-ethan-davis'),
    ('Dr. Isabella Wilson', 'Pediatrician', 'female', 12, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-isabella-wilson'),
    
    # Obstetricians/gynecologist (OBGYNs)
    ('Dr. Olivia Davis', 'Gynecologist', 'female', 25, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-olivia-davis'),
    ('Dr. Liam Johnson', 'Gynecologist', 'male', 22, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-liam-johnson'),
    ('Dr. Ava Brown', 'Gynecologist', 'female', 20, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-ava-brown'),
    ('Dr. Noah Thompson', 'Gynecologist', 'male', 18, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-noah-thompson'),

    # Cardiologist
    ('Dr. Benjamin Smith', 'Cardiologist', 'male', 22, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-benjamin-smith'),
    ('Dr. Charlotte Johnson', 'Cardiologist', 'female', 20, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-charlotte-johnson'),
    ('Dr. Samuel Brown', 'Cardiologist', 'male', 18, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-samuel-brown'),
    ('Dr. Grace Thompson', 'Cardiologist', 'female', 16, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-grace-thompson'),
    
    # Oncologist
    ('Dr. William Wilson', 'Oncologist', 'male', 30, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-william-wilson'),
    ('Dr. Sophia Johnson', 'Oncologist', 'female', 28, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-sophia-johnson'),
    ('Dr. Alexander Brown', 'Oncologist', 'male', 25, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-alexander-brown'),
    ('Dr. Mia Thompson', 'Oncologist', 'female', 22, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-mia-thompson'),
    
    # Gastroenterologist
    ('Dr. Olivia Smith', 'Gastroenterologist', 'female', 25, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-olivia-smith'),
    ('Dr. Liam Johnson', 'Gastroenterologist', 'male', 22, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-liam-johnson'),
    ('Dr. Ava Brown', 'Gastroenterologist', 'female', 20, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-ava-brown'),
    ('Dr. Noah Thompson', 'Gastroenterologist', 'male', 18, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-noah-thompson'),
    
    # Pulmonologist
    ('Dr. Benjamin Wilson', 'Pulmonologist', 'male', 20, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-benjamin-wilson'),
    ('Dr. Charlotte Johnson', 'Pulmonologist', 'female', 18, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-charlotte-johnson'),
    ('Dr. Samuel Brown', 'Pulmonologist', 'male', 16, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-samuel-brown'),
    ('Dr. Grace Thompson', 'Pulmonologist', 'female', 14, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-grace-thompson'),
    
    # Infectious Disease
    ('Dr. William Smith', 'Infectious Disease', 'male', 18, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-william-smith'),
    ('Dr. Sophia Johnson', 'Infectious Disease', 'female', 16, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-sophia-johnson'),
    ('Dr. Alexander Brown', 'Infectious Disease', 'male', 14, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-alexander-brown'),
    ('Dr. Mia Thompson', 'Infectious Disease', 'female', 12, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-mia-thompson'),
    

    # Nephrologist
    ('Dr. Olivia Smith', 'Nephrologist', 'female', 25, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-olivia-smith'),
    ('Dr. Liam Johnson', 'Nephrologist', 'male', 22, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-liam-johnson'),
    ('Dr. Ava Brown', 'Nephrologist', 'female', 20, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-ava-brown'),
    ('Dr. Noah Thompson', 'Nephrologist', 'male', 18, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-noah-thompson'),
    
    # Endocrinologist
    ('Dr. Benjamin Wilson', 'Endocrinologist', 'male', 20, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-benjamin-wilson'),
    ('Dr. Charlotte Johnson', 'Endocrinologist', 'female', 18, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-charlotte-johnson'),
    ('Dr. Samuel Brown', 'Endocrinologist', 'male', 16, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-samuel-brown'),
    ('Dr. Grace Thompson', 'Endocrinologist', 'female', 14, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-grace-thompson'),
    
    # Ophthalmologist
    ('Dr. William Smith', 'Ophthalmologist', 'male', 18, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-william-smith'),
    ('Dr. Sophia Johnson', 'Ophthalmologist', 'female', 16, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-sophia-johnson'),
    ('Dr. Alexander Brown', 'Ophthalmologist', 'male', 14, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-alexander-brown'),
    ('Dr. Mia Thompson', 'Ophthalmologist', 'female', 12, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-mia-thompson'),
    
    # Otolaryngologist
    ('Dr. Olivia Smith', 'Otolaryngologist', 'female', 25, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-olivia-smith'),
    ('Dr. Liam Johnson', 'Otolaryngologist', 'male', 22, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-liam-johnson'),
    ('Dr. Ava Brown', 'Otolaryngologist', 'female', 20, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-ava-brown'),
    ('Dr. Noah Thompson', 'Otolaryngologist', 'male', 18, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-noah-thompson'),
    
    # Dermatologist
    ('Dr. Benjamin Wilson', 'Dermatologist', 'male', 20, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-benjamin-wilson'),
    ('Dr. Charlotte Johnson', 'Dermatologist', 'female', 18, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-charlotte-johnson'),
    ('Dr. Samuel Brown', 'Dermatologist', 'male', 16, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-samuel-brown'),
    ('Dr. Grace Thompson', 'Dermatologist', 'female', 14, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-grace-thompson'),
    
    # Psychiatrist
    ('Dr. William Smith', 'Psychiatrist', 'male', 18, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-william-smith'),
    ('Dr. Sophia Johnson', 'Psychiatrist', 'female', 16, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-sophia-johnson'),
    ('Dr. Alexander Brown', 'Psychiatrist', 'male', 14, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-alexander-brown'),
    ('Dr. Mia Thompson', 'Psychiatrist', 'female', 12, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-mia-thompson'),
    
    # Neurologist
    ('Dr. Olivia Smith', 'Neurologist', 'female', 25, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-olivia-smith'),
    ('Dr. Liam Johnson', 'Neurologist', 'male', 22, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-liam-johnson'),
    ('Dr. Ava Brown', 'Neurologist', 'female', 20, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-ava-brown'),
    ('Dr. Noah Thompson', 'Neurologist', 'male', 18, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-noah-thompson'),
    
    # Radiologist
    ('Dr. Benjamin Wilson', 'Radiologist', 'male', 20, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-benjamin-wilson'),
    ('Dr. Charlotte Johnson', 'Radiologist', 'female', 18, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-charlotte-johnson'),
    ('Dr. Samuel Brown', 'Radiologist', 'male', 16, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-samuel-brown'),
    ('Dr. Grace Thompson', 'Radiologist', 'female', 14, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-grace-thompson'),
    
    # Anesthesiologist
    ('Dr. William Smith', 'Anesthesiologist', 'male', 18, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-william-smith'),
    ('Dr. Sophia Johnson', 'Anesthesiologist', 'female', 16, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-sophia-johnson'),
    ('Dr. Alexander Brown', 'Anesthesiologist', 'male', 14, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-alexander-brown'),
    ('Dr. Mia Thompson', 'Anesthesiologist', 'female', 12, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-mia-thompson'),
    
    # Surgeon
    ('Dr. Olivia Smith', 'Surgeon', 'female', 25, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-olivia-smith'),
    ('Dr. Liam Johnson', 'Surgeon', 'male', 22, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-liam-johnson'),
    ('Dr. Ava Brown', 'Surgeon', 'female', 20, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-ava-brown'),
    ('Dr. Noah Thompson', 'Surgeon', 'male', 18, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-noah-thompson'),
    
    # Physician Executive
    ('Dr. Benjamin Wilson', 'Physician Executive', 'male', 20, '123 Main Street, City, State', '1234567890', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-benjamin-wilson'),
    ('Dr. Charlotte Johnson', 'Physician Executive', 'female', 18, '456 Elm Street, City, State', '9876543210', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-charlotte-johnson'),
    ('Dr. Samuel Brown', 'Physician Executive', 'male', 16, '789 Oak Street, City, State', '2468135790', 'https://svgshare.com/i/tfW.svg', 'https://example.com/doctors/dr-samuel-brown'),
    ('Dr. Grace Thompson', 'Physician Executive', 'female', 14, '321 Maple Street, City, State', '1357924680', 'https://svgshare.com/i/teF.svg', 'https://example.com/doctors/dr-grace-thompson')
]

    with connection.cursor() as cursor:
        cursor.executemany(query, values)

        return render(request, 'index.html')

def check_admin(request):
    email = request.GET.get('email')
    if email:
        try:
            user = AppUser.objects.get(email=email)  # Retrieve the user by email
            is_superuser = user.is_superuser  # Access the is_superuser attribute
            response_data = {'email_exists': True, 'is_superuser': is_superuser}
            return JsonResponse(response_data)
        except AppUser.DoesNotExist:
            response_data = {'email_exists': False, 'is_superuser': False}
            return JsonResponse(response_data)
    else:
        response_data = {'error': 'Email parameter is missing'}
        return JsonResponse(response_data, status=400)
```

`__init__.py`, `apps.py` and `tests.py` can remain with the default value they were initialized with. We don’t need to edit it.

### Disease Predictor Directory

This will the application directory we will use for the disease prediction model. Setup the directory according to the below component tree

```python
DiseasePredictor/
├── Training.csv
├── __init__.py
├── admin.py
├── apps.py
├── models.py
├── tests.py
├── urls.py
└── views.py
```

Training Dataset for the Model

`Training.csv`

[Training.csv](Training.csv)

Create and Paste the following code in the components mentioned

`urls.py`

```python
from django.urls import path
from .views import predict, insert_patient_data, train

urlpatterns = [
    path('prediction/<str:symptoms>/', predict),
    path('insertpd', insert_patient_data),
    path('train', train),
]
```

`views.py`

```python
from Accounts.models import symptoms_diseases, Predicted_Diseases
from Accounts.serializers import PredictionSerializer
from django.shortcuts import render
import pandas as pd
import numpy as np
from django_pandas.io import read_frame
from imblearn.over_sampling import RandomOverSampler
from sklearn.preprocessing import StandardScaler
from sklearn.svm import SVC
from rest_framework.decorators import api_view
from rest_framework.response import Response
import csv
from django.db import transaction
import os
import pickle

def insert_patient_data(request):
    data = os.path.join(os.path.dirname(
        os.path.realpath(__file__)), 'Training.csv')
    with open(data, 'r') as file:
        reader = csv.reader(file)
        next(reader)  # Skip the header row

        with transaction.atomic():
            for row in reader:
                # Map the values from the CSV row to the model fields
                # Exclude the last column
                symptom_values = [int(value) for value in row[:-1]]
                prognosis = row[-1]

                # Create a new instance of the model
                field_names = [field.name for field in symptoms_diseases._meta.get_fields(
                ) if field.name != 'id' and field.name != 'prognosis']
                field_values = dict(zip(field_names, symptom_values))
                instance = symptoms_diseases.objects.create(
                    prognosis=prognosis, **field_values)

                # Save the instance to the database
                instance.save()

            return render(request, 'index.html')

def scale_dataset(dataframe, oversample=False):
    X = dataframe[dataframe.columns[:-1]].values
    y = dataframe[dataframe.columns[-1]].values

    scaler = StandardScaler()
    X = scaler.fit_transform(X)

    if oversample:
        ros = RandomOverSampler()
        X, y = ros.fit_resample(X, y)

    data = np.hstack((X, np.reshape(y, (-1, 1))))

    return data, X, y

svm_model = None

def train(request):
    global svm_model
    data = pd.DataFrame.from_records(
        symptoms_diseases.objects.all().values()).drop('id', axis=1)

    train, X, Y = scale_dataset(data, oversample=True)

    svm_model = SVC(probability=True)
    svm_model = svm_model.fit(X, Y)

    with open('model.pkl', 'wb') as f:
        pickle.dump(svm_model, f)

    return render(request, 'index.html')

@api_view()
def predict(request, symptoms=''):

    with open('model.pkl', 'rb') as f:
        svm_model = pickle.load(f)

    x = np.asarray(list(symptoms), dtype=np.int_)
    x = x[1:]
    x = x.reshape(-1, 1)

    scaler = StandardScaler()
    x = scaler.fit_transform(x)

    x_ = x.reshape(1, -1)
    Y_ = svm_model.predict(x_)

    probas = svm_model.predict_proba(x_)

    top5_indices = np.argsort(probas, axis=1)[:, -5:]
    top5_values = np.take_along_axis(probas, top5_indices, axis=1)

    # Get the corresponding class labels
    top5_labels = svm_model.classes_[top5_indices]

    # Print the top 5 class labels for the first sample in the test data
    pd = top5_labels[0][::-1].tolist()
    predicted_disease = pd[0]

    Rheumatologist = ['Osteoarthristis', 'Arthritis']

    Cardiologist = ['Heart attack', 'Bronchial Asthma', 'Hypertension ']

    ENT_specialist = [
        '(vertigo) Paroymsal  Positional Vertigo', 'Hypothyroidism']

    Neurologist = ['Varicose veins',
                   'Paralysis (brain hemorrhage)', 'Migraine', 'Cervical spondylosis']

    Allergist_Immunologist = ['Allergy', 'Pneumonia', 'AIDS',
                              'Common Cold', 'Tuberculosis', 'Malaria', 'Dengue', 'Typhoid']

    Urologist = ['Urinary tract infection', 'Dimorphic hemmorhoids(piles)']

    Dermatologist = ['Acne', 'Chicken pox',
                     'Fungal infection', 'Psoriasis', 'Impetigo']

    Gastroenterologist = ['Peptic ulcer diseae', 'GERD', 'Chronic cholestasis', 'Drug Reaction', 'Gastroenteritis', 'Hepatitis E',
                          'Alcoholic hepatitis', 'Jaundice', 'hepatitis A',
                          'Hepatitis B', 'Hepatitis C', 'Hepatitis D', 'Diabetes ', 'Hypoglycemia']

    if predicted_disease in Rheumatologist:
        consultdoctor = "Rheumatologist"

    if predicted_disease in Cardiologist:
        consultdoctor = "Cardiologist"

    elif predicted_disease in ENT_specialist:
        consultdoctor = "ENT specialist"

    elif predicted_disease in Neurologist:
        consultdoctor = "Neurologist"

    elif predicted_disease in Allergist_Immunologist:
        consultdoctor = "Allergist/Immunologist"

    elif predicted_disease in Urologist:
        consultdoctor = "Urologist"

    elif predicted_disease in Dermatologist:
        consultdoctor = "Dermatologist"

    elif predicted_disease in Gastroenterologist:
        consultdoctor = "Gastroenterologist"

    else:
        consultdoctor = "other"

    pd_prob = top5_values[0][::-1].astype(float).tolist()
    Predicted_Diseases.objects.all().delete()
    Predicted_Diseases(diseases=pd, diseases_prob=pd_prob, consult_doctor=consultdoctor).save()
    data = Predicted_Diseases.objects.all()
    serializer = PredictionSerializer(data, many=True)
    return Response(serializer.data, template_name=None)
```

All the files left needs no changes and can remain by default

### Database Migrations

Run the following command in your terminal to make database migrations

```bash
python manage.py makemigrations

python manage.py migrate
```

## Environment Variables

Add the following environment variables to your `.env`

```bash
USER = <Postgres_Username>
DATABASE_NAME = <Database_Name>
PASS = <Password>
```

## Building the Frontend

In the components directory, create the following components and pages. Copy and paste the provided code

### Pages and Components

`About.jsx`

```jsx
import patternImg from "../img/pattern.svg";
const About = () => {
  return (
    <div
      id="about"
      className="w-full flex justify-center mt-10
    "
    >
      <div className="about-container flex flex-col-reverse md:flex-row items-center w-3/4">
        <div className="hero flex flex-col justify-center  md:w-2/3 ">
          <div className="hero-text text-3xl text-center md:text-left mt-5 lg:text-5xl mb-4 md:mb-5">
            About Medware
          </div>
          <div className="hero-stanza  lg:text-lg flex items-center md:w-4/5 pl-5 md:pl-0 mb-7">
            Your one-stop healthcare provider. Our innovative medical dashboard
            and disease predictor offer personalized insights into your health.
            Convenient doctor consultations and a range of healthcare services
            are just a click away. Experience the difference in exceptional care
            and advanced technologies with Medware.
          </div>
        </div>
        <div className="img-wrapper w-80 mb-5 md:w-1/3 ">
          <img src={patternImg} alt="hero-image" className="block w-full" />
        </div>
      </div>
    </div>
  );
};

export default About;
```

`BP_Log.jsx`

```jsx
import React from "react";

const BP_Log = ({ responseData }) => {
  if (
    !responseData ||
    !responseData.bp_log ||
    !responseData.bp_log.date ||
    responseData.bp_log.date.length === 0
  ) {
    return (
      <p className="h-full w-full grid place-content-center italic bg-pink-50 text-gray-500 md:text-lg">
        Add your first value
      </p>
    );
  }

  return (
    <div className="p-2 mb-1 rounded-lg">
      {responseData.bp_log.date.map((date, index) => {
        const currentHigh = responseData.bp_log.high[index];
        const currentLow = responseData.bp_log.low[index];

        // Skip the log entry if high and low values are empty
        if (!currentHigh && !currentLow) {
          return null;
        }

        const isFirstValueOfDay =
          index === 0 || responseData.bp_log.date[index - 1] !== date;

        return (
          <div key={index} className="flex flex-col mb-1">
            {isFirstValueOfDay && (
              <div className="flex items-center mb-2 bg-slate-200 rounded-md mx-1 p-2">
                <div className="h-2 w-2 bg-gray-700 rounded-full mr-2"></div>
                <h2 className="text-lg font-semibold text-gray-900">{date}</h2>
              </div>
            )}

            <div className="ml-1">
              <div
                className={`text-sm text-gray-700 border border-gray-400 rounded-md p-3 flex justify-between items-center ${
                  currentHigh > 190 && currentLow > 90
                    ? "bg-purple-100"
                    : currentHigh > 190
                    ? "bg-red-100"
                    : currentLow > 90
                    ? "bg-orange-100"
                    : ""
                }`}
              >
                <div className="flex">
                  <p className="font-semibold">{currentHigh}</p>
                  <span className="text-gray-500 mx-1">/</span>
                  <p>{currentLow}</p>
                </div>
                <div>High / Low</div>
              </div>
            </div>
          </div>
        );
      })}
    </div>
  );
};

export default BP_Log;
```

`BP_chart.jsx`

```jsx
import React from "react";
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  BarElement,
  Title,
  Tooltip,
  Legend,
} from "chart.js";
import { Bar } from "react-chartjs-2";

export default function BP_chart({ chartData }) {
  ChartJS.register(
    CategoryScale,
    LinearScale,
    PointElement,
    BarElement,
    Title,
    Tooltip,
    Legend
  );

  const { low, date, high } = chartData;

  const options = {
    responsive: true,
    maintainAspectRatio: false,
    interaction: {
      mode: "index",
      intersect: false,
    },
    plugins: {
      title: {
        display: true,
        text: "Blood Pressure",
      },
    },
    scales: {
      y: {
        type: "linear",
        display: false,
      },
      y1: {
        type: "linear",
        display: true,
        position: "left",
        grid: {
          drawOnChartArea: false,
        },
      },
    },
  };

  const data = {
    labels: date,
    datasets: [
      {
        label: "Low",
        data: low,
        backgroundColor: "rgb(252, 99, 255, 0.7)",
        yAxisID: "y1",
        barPercentage: 0.6, // Adjust the bar width (0.6 means 60% of the available space)
        borderRadius: 10, // Adjust the border radius to make the bars slightly curved
      },
      {
        label: "High",
        data: high,
        backgroundColor: "rgba(99, 99, 255, 0.7)",
        yAxisID: "y1",
        barPercentage: 0.6,
        borderRadius: 10,
      },
    ],
  };

  return <Bar options={options} data={data} />;
}