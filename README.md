# app-example-aws

Example project for deploying Grouparoo on `AWS` with `ElasticBeanstalk` & `CodeDeploy`

## Notes

- No `Procfile` can be included
- Cannot use `$PORT` in Dockerfile references (AWS parses that and passes it on to NGINX)
