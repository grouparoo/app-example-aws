# app-example-aws

Example project for deploying Grouparoo on `AWS` with `ElasticBeanstalk` & `CodeDeploy`.

Goal: To create a scalable and flexible Grouparoo deployment that:

- Has no servers to directly manage
- Can be auto-scaled as needed
- Will be automatically deployed when the code changes
- Is a [12-factor app](https://12factor.net/) with all configuration stored in the Environment

## Steps

1. Configure your VPC
2. Create the Grouparoo Database
   - Use Aurora + Postgres
   - Use `Provisioned` servers, we recommend `db.t3.large`
3. Create the Grouparoo Redis Server
   - Create 1 replica
   - We recommend `cache.t2.small` servers
4. Create the Elastic Beanstalk Project

   - Application
     - `Node.js 14 running on 64bit Amazon Linux 2`
   - Environment:
     - Choose to deploy the sample project first
     - `t2.micro` instances should be OK
     - Set your Environment variables to connect to Redis and Postgres

5. Create teh CodeDeploy Pipeline to update the application

## Notes

- need special install permissions (see `.npmrc`)
- Set the ENV first before setting up `AWS CodePipeline`
- no need for a `Procfile`, we use `npm start`
- Don't use `package.json/engines`, AWS will manage this for you
