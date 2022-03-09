***Deployment of IP2 in application***

****EXTRA RESOURCE BY ALBERT BYRONE****
https://github.com/Albert-Byrone/Deploy_to_Heroku_Flask

**Step 1**
 make sure its working locally without errors

 **Step 2**
 Login to heroku

 heroku login

 **step 3**
check on existing heroku apps

heroku apps

**step 4**
destroy one existing heroku if they are many and follow instructions

heroku apps:destroy <flask-movies-app>         flask-movies-app is just an example

**step 4**
create a new heroku app name
heroku create <app_name>

git add . && git commit -m"deploy"
git push origin master

then:
    heroku git:remote -a yourapp

git push heroku master
**step 5**
Since the __init__.py in app folder uses config as follows:-

def create_app(config_name):

==> it means we are using config.py to  run our app in manage.py

so head to manage.py ===> and change the following line


manage.py
# change this to production on deployment to heroku as follows
app = create_app('production')

**step 6**
Go to config.py > production class
make sure its configured as follows 

class ProdConfig(Config):
    '''
    Production  configuration child class
    Args:
        Config: The parent configuration class with General configuration settings
    '''
    SQLALCHEMY_DATABASE_URI = os.environ.get("DATABASE_URL")      <====


**step 7**

check if database is initialized in __init__.py

 db.init_app(app)

 then add following line in __init__.py


    app.config["SQLALCHEMY_DATABASE_URI"] = os.environ.get("DATABASE_URL")
    app.config["SQLALCHEMY_TRACK_MODIFICATIONS"] = False

   

    return app


**step 8**

add a new file called .env
  
    and store data example if you are storing email data:

    MAIL_USERNAME = "justus.onyancha@moringaschool.com"
    MAIL_PASSWORD = "password"

**Step 9**

if you head to heroku, you will realize the app is there but empty

heroku > new_created_heroku_app > settings > config variable 

you will see the gmail we pushed to .env and its password 

note:they arent in this particular file

**Step 10**
from thursday content on deployment

we still have to set up postgres in heroku 


add following command in terminal:
heroku addons:create heroku-postgresql:hobby-dev

error to expect with above command:

heroku addons:create heroku-postgresql:hobby-dev
 ›   Error: Missing required flag:
 ›    -a, --app APP  app to run command against
 ›   See more help with --help

 solution:  https://stackoverflow.com/questions/51815542/heroku-missing-required-flag-a

 solution2:
 
 heroku addons:create heroku-postgresql:hobby-dev --app flaskweek2         
 
 //flaskweek2 is the appname in heroku we created in terminal

 Note; if you head back to heroku > appname > settings > reveal configurations > you will see the database url added after that command


 **step 10**

 we run:
 pip freeze > requirements.txt

 **step 11**:

 git add . && git commit -m"fix for deployment"


then: 
 git push origin master 

then:

git push heroku master

expected error :

solution1: https://stackoverflow.com/questions/18406721/heroku-does-not-appear-to-be-a-git-repository

solution2: heroku login
           heroku git:remote -a yourapp

then repeat: git push heroku master


error expected if failed:
its with dependencies versions, make sure they are the right versions by either upgrading or downgrading the versions for dependencies


EDIT AND THEN RUN {  pip install -r requirements.txt  }
**step 12**


Expected error: 

remote: -----> $ python manage.py collectstatic --noinput
remote:        usage: manage.py [-?] {server,db,test,shell,runserver} ...
remote:        manage.py: error: argument {server,db,test,shell,runserver}: invalid choice: 'collectstatic' (choose from 'server', 'db', 'test', 'shell', 'runserver')
remote: 
remote:  !     Error while running '$ python manage.py collectstatic --noinput'.
remote:        See traceback above for details.
remote: 
remote:        You may need to update application code to resolve this error.
remote:        Or, you can disable collectstatic for this application:
remote: 
remote:           $ heroku config:set DISABLE_COLLECTSTATIC=1
remote: 
remote:        https://devcenter.heroku.com/articles/django-assets
remote:  !     Push rejected, failed to compile Python app.
remote: 
remote:  !     Push failed
remote: Verifying deploy...
remote: 


solution1: $ heroku config:set DISABLE_COLLECTSTATIC=1

again run: $ git push heroku master

**step 13**
if error(from above commands):

    delete the migrations folder in app

    then follw steps below


run the following command to upgrade databse

1st: $ python3 manage.py db init

2nd: $ python3 manage.py db revision

3rd: $ git add . && git commit -m"db initialized locally and revision file created"

4th: $ git push origin master

5th: $ git push heroku master

6th: $ heroku run python3 manage.py db revision

7th: $ heroku run python3 manage.py db upgrade

then:






