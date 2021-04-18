**AWS S3**
* AWS Management Console
* Search: Storage S3
* Create Bucket (ignore all of the options)
* Permission --> CORS --> edit and paste JSON code below
```
  [
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "POST",
            "PUT"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
  ]
```

* If you don't have existing IAM, search IAM --> Users --> Add user
* Fill in 'User name', check 'Programmatic access' box
* Set permissions, choose 'Attach existing policies directly'
* Choose AmazonS3FullAccess --> no need to Add Tags --> Create User
* Check/download csv for Access Key ID & Secret Access Key
* Open Environment Variables from cmd
```
  rundll32.exe sysdm.cpl,EditEnvironmentVariables
```

* Click 'New', make 3 new variables below:
```
  Variable Name = AWS_ACCESS_KEY_ID
  Value = (copy from Access Key ID)
```
```
  Variable Name = AWS_SECRET_ACCESS_KEY
  Value = (copy from Secret Access Key)


  Variable Name = AWS_STORAGE_BUCKET_NAME
  Value = (fill in bucket name, i.e django-shop-bagus)
```

* Install boto3 and django-storages
```
  pip install boto3
```
```
  pip install django-storages
```
```
  INSTALLED_APPS = [
      ...
      'storages',
  ]
```
```
  AWS_S3_FILE_OVERWRITE = False
  AWS_DEFAULT_ACL = None

  DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
```

* Add initial object by dragging subdirectories and files from 'media' directory (don't drag 'media' directory directly)

**GIT PULL**
```
git init
git remote add origin (repository link)
git pull origin master
```

**HEROKU DEPLOYMENT**
* Install HEROKU
* Install gunicorn
pip install gunicorn

* Login to heroku using powershell or cmd
heroku login -i

* Freeze requirements
pip freeze > requirements.txt

* Create heroku app
heroku create bagusdjangoshop

* Open https://bagusshop.herokuapp.com/ in your browser
* Push local repo to heroku
git add .
git commit -am 'update'
git push heroku master

* Create proclife file (no extention). Put this text inside
web: gunicorn myshop.wsgi

* Edit settings.py
DEBUG = False
...
ALLOWED_HOSTS = ['https://bagusshop.herokuapp.com/']

* Get new secret key from python shell, then copy paste to environment variables
python
import secrets
secrets.token_hex(24)

* Add to environment variables, edit settings.py
SECRET_KEY = os.environ.get('SECRET_KEY')

* Add configuration to heroku
heroku config:set SECRET_KEY="paste new generated key here"
heroku config:set AWS_ACCESS_KEY_ID="paste access key here"
heroku config:set AWS_SECRET_ACCESS_KEY="paste secret key here"
heroku config:set AWS_STORAGE_BUCKET_NAME_SHOP="paste bucket name here"
