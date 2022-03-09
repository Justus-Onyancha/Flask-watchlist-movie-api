# Justus Deployment prepared for students

## Extra resouce by Albert Byrone
https://github.com/Albert-Byrone/Deploy_to_Heroku_Flask

## Useful resource

https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-20-04

### Deployment
If you wish to deploy your app on heroku, you can follow the steps on this file

Requirements

1. gunicorn
2. Heroku Account
3. Heroku CLI

pip install gunicorn

pip freeze > requirements.txt


***Go into your _requirements.txt_ file and remove the pkg-resources==0.0.0dependency. This is a small bug that will prevent us from deploying our applications.***

* Create Procfile an root directory
web: gunicorn manage:app

* Heroku CLI
heroku login

heroku create <name-of-app>

heroku config:set CONFIG=<config-name>

***If your app has a postgresdb***

heroku addons:create heroku-postgresql


* Go to config.py and set the db URI in your ProdConfig class
class ProdConfig(Config):
    SQLALCHEMY_DATABASE_URI = os.environ.get("DATABASE_URL")
 
 
* Don’t forget to switch to ‘production’ in manage.py
git add .
git commit -m "deployment to heroku"
git push origin master
git push heroku master


* Finally, upgrade db in heroku
heroku run python3 manage.py db upgrade


***note***
avoid deployment using master to Heroku

failed to push some refs to git@heroku.com

*Solution*

git push heroku main

     instead of

git push heroku master