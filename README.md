# app-example-aws

Example project for deploying Grouparoo on `AWS` with `ElasticBeanstalk` & `CodeDeploy`

## Notes

- need special install permissions (see `.npmrc`)
- Set the ENV first before setting up `AWS CodePipeline`
- no need for a `Procfile`, we use `npm start`
- Don't use `package.json` - `engines`, AWS will manage this for you
