# Grouparoo `app-example-aws`

_An example project for deploying Grouparoo on Amazon Web Services (`AWS`) with `ElasticBeanstalk` & `CodeDeploy`._

Goal: To create a scalable and flexible Grouparoo deployment that:

- Has no servers to directly manage
- Can be auto-scaled as needed
- Will be automatically deployed when the code changes
- Is a [12-factor app](https://12factor.net/) with all configuration stored in the Environment

## Repository Configuration

1. Create a new Grouparoo project. Learn more @ https://www.grouparoo.com/docs/installation.

```
npm install -g grouparoo
grouparoo init .
```

2. Install the Grouparoo plugins you want, e.g.: `grouparoo install @grouparoo/postgres`. Learn more @ https://www.grouparoo.com/docs/installation/plugins

3. Remove any `engines` declaration from your `package.json`
4. Create the `.npmrc` to enable `unsafe-perm`

## Deployment Steps

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
     - `t2.small` instances should be OK
   - Deploy the Example Project
   - Set the Environment variables to link up the application to the Postgres and Redis server, as well as enable the web server and workers (while the example project is deployed).

5. Create the CodeDeploy Pipeline to update the application from your git repository

   - Do not use a `build` step, all you need is `Source` and `Deploy`

6. Configure Monitoring
7. Configure Load Balancer, SSL, and DNS

## Notes

- Elastic Beanstalk needs special install permissions (see `.npmrc`)
- Set the ENV first before setting up `AWS CodePipeline`
- No need for a `Procfile`, we use `npm start`
- Don't use `package.json/engines`, AWS will manage this for you
- You may want to modify logging behavior with:
  - `GROUPAROO_LOGS_STDOUT_DISABLE_TIMESTAMP=true`- AWS adds timestamps to all log messages
  - `GROUPAROO_LOGS_STDOUT_DISABLE_COLOR=true`- AWS will not render log messages in color

---

Visit https://github.com/grouparoo/app-examples to see other Grouparoo Example Projects.
