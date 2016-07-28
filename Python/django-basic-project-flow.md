#Django

Basic Django Project Workflow
--------------------------------

1. Create virtualenv & Activate

  ```bash
  virtualenv env
  ..\scripts\activate.bat
  ```
2. Install Django

  ```bash
  pip install django==1.9
  ```
3. Create a new Project

  ```bash
  django-admin.exe startproject mysite
  cd mysite
  python manage.py 
  ```
4. Migrate Database

  ```bash
  python manage.py migrate
  ```
5. Run server

  ```bash
  python manage.py runserver
  ```
  go, localhost:8000, press ctrl+break to stop
6. Create an app

  ```bash
  python manage.py startapp myapp
  ```
  the command will create an app myapp, add this to INSTALLED_APPS
  
7. create migration(s)

  ```bash
  python manage.py makemigrations myapp
  #python manage.py sqlmigrate 0001
  python manage.py migrate
  ```
8. Create User Accounts

  ```bash
  python manage.py createsuperuser
  python manage.py migrate
  ```

9. Create your models in models.py
10. register site in admin.py
11. makemigrations & migrate
  ```bash
  python manage.py makemigrations
  python manage.py migrate
  ```
11. Run the server and configure django admin, go to your browser and go to http://localhost:8000

  ```bash
  python manage.py runserver
  ```
