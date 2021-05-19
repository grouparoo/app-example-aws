# Grouparoo `app-example-aws`

_An example project for deploying Grouparoo on Amazon Web Services (`AWS`) with `ElasticBeanstalk` & `CodeDeploy`._

Goal: To create a scalable and flexible Grouparoo deployment that:

- Has no servers to directly manage
- Can be auto-scaled as needed
- Will be automatically deployed when the code changes
- Is a [12-factor app](https://12factor.net/) with all configuration stored in the Environment

## How We Got Here

1. Create a new Grouparoo project. Learn more @ https://www.grouparoo.com/docs/installation.

```
npm install -g grouparoo
grouparoo init .
```

2. Install the Grouparoo plugins you want, e.g.: `grouparoo install @grouparoo/postgres`. Learn more @ https://www.grouparoo.com/docs/installation/plugins

3. Remove any `engines` declaration from your `package.json`
4. Create the `.npmrc` to enable `unsafe-perm`

## Running this Repo

Assuming you have node.js installed (v12+) and the Grouparoo CLI (npm install -g grouparoo):

1. `git clone https://github.com/grouparoo/app-example-aws.git`
2. `cd app-example-aws`
3. `npm install`
4. `cp .env.example .env`
5. `grouparoo run`

## Deployment Steps

1. Configure your VPC
   - Create an internal Security Group that redis and postgres can use to communicate with the app servers from other members of the security group (port 5432/tcp and 6379/tcp)
     - [Learn More](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.ElastiCache.html)
2. Create the Grouparoo Database
   - Use Aurora + Postgres
   - Use `Provisioned` servers, we recommend `db.t3.large`
   - Create a postgres user + password for the Grouparoo application to use
   - Apply the internal Security Group made in step 1
3. Create the Grouparoo Redis Server
   - Create 1 replica
   - We recommend `cache.t2.small` servers
   - Apply the internal Security Group made in step 1
4. Create the Elastic Beanstalk Application + Environments

   - Application
     - `Node.js 14 running on 64bit Amazon Linux 2`
   - Environment:
     - Choose to deploy the sample project first
     - `t2.small` instances should be OK
   - Enable Config / Managed Updates
   - Deploy the Example Project
   - Set the Environment variables to link up the application to the Postgres and Redis server, as well as enable the web server and workers (while the example project is deployed).

   You can create multiple Environments in the same Application so you can have a WEB and WORKER environment.

5. Create the CodeDeploy Pipeline to update the application from your git repository

   - Do not use a `build` step, all you need is `Source` and `Deploy`
   - If you you created multiple Elastic Beanstalk Environments, you can create a second Deploy step within a single CodeDeploy pipeline.

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
