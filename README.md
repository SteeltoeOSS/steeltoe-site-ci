## Steeltoe Site CI

Contains the concourse pipeline file.

#### Location:
This is being built on Concourse instance (spring) under the team `steeltoe`.

Docker image is public and on DockerHub under the `steeltoeoss` organization.

S3 bucket is under the application platform team AWS account.

PCF organization is `steeltoe.io`

#### Pipeline
Contains a staging pipeline and a production pipeline for the website (including documentation).

##### Process (same for both staging and production):

  1. Pulls down the docker image
  2. Clones the steeltoe-site git repo into image
  3. Builds the docs
  4. Tars up the docs and puts them on s3
  5. Publish task pulls in the docker image
  6. Publish task then pulls documentation package down from s3 onto image
  7. Publish task CF pushes the documentation along with the static buildpack to the steeltoe.io organization on PCF (staging or production).

#### Fly Command

```
 fly -t TARGET_NAME set-pipeline -p PIPELINE -c steeltoe-site-pipeline.yml --var "github_username=GIT_USERNAME" --var "github_password=GIT_USER_OAUTH_TOKEN" --var "cf_username=CF_USERNAME" --var "cf_password=CF_PASSWORD" --var "access_key_id=AWS_KEY" --var "secret_access_key=AWS_SECRET_KEY" --var "slack_url=SLACK_HOOK_URL"
```