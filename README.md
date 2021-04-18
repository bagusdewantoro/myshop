AWS S3
- AWS Management Console
- Search: Storage S3
- Create Bucket (ignore all of the options)
- Permission --> CORS --> edit and paste JSON code below</br>

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


- If you don't have existing IAM, search IAM --> Users --> Add user
- Fill in 'User name', check 'Programmatic access' box
- Set permissions, choose 'Attach existing policies directly'
- Choose AmazonS3FullAccess --> no need to Add Tags --> Create User
- Check/download csv for Access Key ID & Secret Access Key
- Open Environment Variables from cmd</br>

    rundll32.exe sysdm.cpl,EditEnvironmentVariables

- Click 'New', make 3 new variables below:
</br>

    Variable Name = AWS_ACCESS_KEY_ID
    Value = (copy from Access Key ID)

    Variable Name = AWS_SECRET_ACCESS_KEY
    Value = (copy from Secret Access Key)

    Variable Name = AWS_STORAGE_BUCKET_NAME
    Value = (fill in bucket name, i.e django-shop-bagus)

- Install boto3 and django-storages
</br>

    pip install boto3
    pip install django-storages

    INSTALLED_APPS = [
        ...
        'storages',
    ]

    AWS_S3_FILE_OVERWRITE = False
    AWS_DEFAULT_ACL = None

    DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'

- Add initial object by dragging subdirectories and files from 'media' directory (don't drag 'media' directory directly)
