# Topics
### Creating and Running Migrations
1. Modify Models
2. Ensure that app is included in settings.INSTALLED_APPS
3. Run `python manage.py makemigrations <app_name>`. This generates the migration files
4. (optional) Inspect migration with file `python manage.py sqlmigrate polls 0001`
5. Run the actual migration `python manage.py migrate`

---
# Notes on using Django
1. Probably need a deployment script to handle static assets well (even when using Heroku) Goal is to ensure you concatenate and minify (and versioning?) assets locally first
2. Ensure entire dev team uses pipenv to manage dependencies (and not to use PIP or Conda to install dependencies for the project)(A lot of tutorials and source code READMEs will use PIP to install dependency, don't if using Pipenv!)
https://robots.thoughtbot.com/how-to-manage-your-python-projects-with-pipenv
3. Have 2 versions of `settings.py` to prevent using development settings in production or heavily rely on dynamic settings through env variables. 
**Using env way:**
https://godjango.com/blog/working-with-environment-variables-in-python/
https://github.com/joke2k/django-environ (don't think this is a good idea, it breaks conventional style, making deployment difficult without fully following this)
**Using separate settings file (still use env variables to prevent secret keys being committed into Git!)**
https://docs.djangoproject.com/en/20/topics/settings/#designating-the-settings
https://thinkster.io/tutorials/configuring-django-settings-for-production

For safety:
```python
if os.environ.get('ENVIRONMENT') == 'Production':
    DEBUG = False
elif os.environ.get('ENVIRONMENT') == 'Development':
    DEBUG = True
else:
    #Raise an exception
```

---
# Pros and Cons
### What I like about Django over Rails
1. The way you generate migration by first defining fields (columns) in models first is amazing. The visibility you get about your fields within model files is great. Although, in Rails, there are Gems (used in development) to generate field/column information in Model files from schema.
2. It doesn't handle do asset precompilation for you makes working with JS frameworks a lot easier especially when using Django as a API to a frontend application although Webpack is now default in later versions of Rails.
3. The way you organize MVC by 'apps' which prevents project directory becoming a giant MVC mess in bigger projects
4. Importing modules manually in each file makes it obvious what you're using (less magical)
5. Routes are extremely explicit although verbose. My concern is the common pattern of abstracting routes into modules by apps may make investigating all routes more tedious unless there is an equivalent of `rake routes` (to be researched)
6. Because of how Django is more explicit, it would be easier to onboard an engineer onto an existing project

### What I dislike about Django compared to Rails
1. Default authentication feels rigid (still need to explore more). And I don't prefer that it generates an `auth_user` table with default (some required and some optional) fields and a permission system for me if I use `django.contrib.auth` / `django.contrib.admin` app(s).
Questions:
    1. How does it affect my ability to create very customized sign up flows (e.g. username is compulsory, potentially delay creation or fake username first)
2. Ruby as a language and community has a default dependency manager, Bundler. In Rails, Gemfiles are auto generated. Hence, the way you manage dependencies are a default. In Django, however, there isn't a default (because of Python). Heroku recommends using Pipenv, which is fine but when you generate a new Django project, it doesn't generate a Pipfile for you automatically. This non-default requires dev teams to enforce a standard for all devs to use Pipenv to install new dependencies.
3. While it can be an advantage, the fact that Django doesn't help with asset (CSS, JS) concatenation, minification is also a bit annoying. (Not sure how caching is affected. In Rails, all assets are concatenated, minified and versioned). https://stackoverflow.com/questions/34586114/whats-the-point-of-djangos-collectstatic
https://devcenter.heroku.com/articles/django-assets

4. The way you use access environment variables by default in Django / Python `os.environ.get('NAME',3)` is very verbose compared to Rails' `ENV[:NAME]`. (we can override default but not a good idea to not follow the framework's conventions)
5. It has a single entry `settings` entry point for all environments by default rather than it being like Rails. In a self-managed production environment, gotta be careful with settings like `DEBUG`, `SECRET_KEY`
https://docs.djangoproject.com/en/2.0/topics/settings/#designating-the-settings
6. Static assets are not served in production automatically
7. You gotta import modules manually unlike Rails
8. Default templating language sucks
http://jinja.pocoo.org/docs/2.10/faq/#how-compatible-is-jinja2-with-django
9. Structuring code needs more thinking. For e.g. Need to make upfront decisions about where models should go to if they are shared across `apps` within the project
10. 3rd party dependencies are not as well updated, which potentially causes security, missed performance, and updating issues
11. `URLConfig` or Django's routes system doesn't differentiate different HTTP methods (GET, POST etc+) instead enforcing them on `views` (or controller in Rails) level via decorators. This maybe be a stylistic preference but it doesn't make available endpoints and their corresponding HTTP methods immediately obvious.