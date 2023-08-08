Deploying a server: 
`python3 manage.py runserver`

Creating an app
` python manage.py startapp polls`

1. Make app-view (view.py) 
2. Map view to a URL (URLconf, app/urls.py)
3. Point root URLconf to app-urls module (site/urls.py)
	1. django.urls import path, include 
	2. You should always use `include()` when you include other URL patterns. `admin.site.urls` is the only exception to this.


<h1> The path argument </h1>
path(route,view(HttpRequest),kwargs,name)


<h1> Making a database </h1>
1. Change time-zone in site/settings.py. Copenhagen is CET
2. Create data-base tables: `python3 manage.py migrate`
	1. This command makes all migrations
3. Create data models per app - app/models.py
	1. Every class variable represents a databse field in the model 
	2. Each variable field has an instance of a Field class (CharField, DateTimeField, etc.), ie. datatype 
	3. The variable name of the instance is the column name of the field, e.g. question
4. Add needed apps in INSTALLED_APPS settings, polls.apps.PollsConfig
5. Run `makemigrations app` for storing data-base changes for models as a migration
6. Table-names are appname_modelname
	1. Can be inspected running python3 manage.py sqlmigrate polls 0001 (migration number)
7. Issue `python manage.py check` to check for problems in project without making migrations / editing database
8. Generate tables for models in database by running `python3 manage.py migrate`


<h1> Playing with the API</h1>
1. Issue `python3 manage.py shell` (this automatically gives you the import path to site/settings.py)
2. Import models `from polls.models import Choice, Question`
3. Check status of question-objects: `Question.objects.all()`
4. Create a new question with a time-zone. this is done by calling Question-constructor and making object.save 
5. You can edit entries and save again. 
6. adding a __str__-method to model-class determines print output
7. Some usefull data-base commands: 

| Command on model-object | Description |   
| -------- | -------- |   
| filter(condition) | find corresponding set of entries, e.g. objects.filter(id=1) |   
| textfield__startswith="str"| Get set of entries starting with str |
| get(condition) | returns the question object |
8. In interactive shell, set choices corresponding to questions by calling create on a choice-object 
9. Deleting a choice object again: use a filter, save as variable, delete variable c.delete()



<h1> Creating an admin user</h1>
1. `python3 manage.py createsuperuser`
2. Enter username, email, password (2x)
3. logging in through localhost:8000/admin

<h1> Making app modifiable in admin-panel </h1>
1. Add an admin interface to the app/admin.py 
2. `from django.contrib import admin from .models import Question admin.site.register(Question)`

<b> Status: tutorial page 3, making a user interface </b>






