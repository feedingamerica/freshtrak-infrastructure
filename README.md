# AWS Infrastructure

Cloudfomation templates used to deploy the freshtrak infrastructure.
Currently there is very little custom infrastructure needed to run the project so this project can be deployed using the aws cli.

## Deployment

#### UI

```
aws cloudformation deploy \
  --region us-east-2 \
  --template-file cloudfront.yml \
  --stack-name freshtrak-ui-beta \
  --parameter-overrides CertificateArn=arn:aws:acm:us-east-1:903047886911:certificate/9e2ef676-5aa9-460b-8e17-72ca479cd97d
```
```
aws cloudformation deploy \
  --region us-east-2 \
  --template-file cloudfront.yml \
  --stack-name freshtrak-ui \
  --parameter-overrides CertificateArn=arn:aws:acm:us-east-1:903047886911:certificate/0ebf1c18-6130-4551-83e8-86d07e827bed \
    UrlPrefix="" \
    Name=freshtrak-ui
```
*Note*: Running this stack requires that there is a pre-existing ssl certificate for the domain being deployed. Since cloudfront is global the ACM certificate must be in `us-east-1` even though the cloudformation stack is in `us-east-2`.

#### Deployment Pipelines

```
aws cloudformation deploy \
  --region us-east-2 \
  --template-file codepipeline.yml \
  --capabilities CAPABILITY_IAM \
  --stack-name freshtrak-pantry-finder-api-pipeline \
  --parameter-overrides Repo=freshtrak-pantry-finder-api
```
```
aws cloudformation deploy \
  --region us-east-2 \
  --template-file codepipeline.yml \
  --capabilities CAPABILITY_IAM \
  --stack-name freshtrak-registration-api-pipeline \
  --parameter-overrides Repo=freshtrak-registration-api
```
```
aws cloudformation deploy \
  --region us-east-2 \
  --template-file codepipeline.yml \
  --capabilities CAPABILITY_IAM \
  --stack-name freshtrak-client-pipeline \
  --parameter-overrides Repo=freshtrak-client
```
