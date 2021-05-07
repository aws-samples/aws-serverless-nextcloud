# Nextcloud Container Deployment on AWS - 100% Serverless

## Welcome

This repository demonstrates how an application like NextCloud can be run on AWS completely serverless.
With the right services and tools there is no need to manage servers or manually increase capacity.

## AWS Services & How they match the NextCloud design?

* Elastic Container Service - Fargate
    * Running and scaling the official nextcloud docker container (Apache, PHP)
* Elastic Filesystem - NFS
    * Persist basic settings and configuration, support official nextcloud upgrade mechanism
* S3
    * Primary data storage for cloud native data handling (archiving, tiering, versioning)
* Aurora Serverless - RDS (Postgres)
    * Cloud native full managed auto-scaled database system backing the nextcloud installation
* ElastiCache - Redis
    * Handles PHP Sessions to enable container cluster to scale easily without interruption for end-users
* Application Load Balancer, Route53, Amazon Certificate Manager
    * Secures the application with HTTPS, balances load, performs health checks, auto-certificate renewal

## Deployment

* `aws cloudformation package --template-file ecs-nextcloud.yml --s3-bucket <cfn-artifact-bucket-name> --output-file packaged.yaml `
* Deploy packaged CloudFormation file (`packaged.yaml`) with appropriate parameters

## Architecture

![Architecture Diagram](docs/aws-nextcloud.png)

## How to upgrade Nextcloud to newer version

1. Create backups of RDS and EFS
2. Suspend AutoScaling   
3. Scale in to 1 task
4. Update CFN stack with new version
5. Wait for Nextcloud come up
6. Scale out service to desired size

## Future Work

* Audit IAM roles for least privileged permissions
* Enhanced monitoring for fine granular auto-scaling (RDS and ECS)

## Datapoints

* Baseline costs xyz $

### Note:

* **While code samples in this repository has been tested and believe it works well, as always, be sure to test it in your environment before using it in production!**

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the [LICENSE](LICENSE) file.