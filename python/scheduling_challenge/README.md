recommended dependency: python/rest_api_challenge

1. Set a scheduled job, cron or airflow, to extract a daily snapshot of all available data from a personal rest api of choice.
    - An ideal API would provide daily data that may change in the past.
    - An example would be a nutrition app which can have meals added into the past

2. Extract as much meaningful/or slightly meaningful/ data as possible, everyday into a local json or csv file.

3. upload the json or csv file into an s3 bucket, everyday as well

4. delete the local file

(optional) 5. Don't store the file locally, but rather stream the file contents into S3 with a stringBuffer
