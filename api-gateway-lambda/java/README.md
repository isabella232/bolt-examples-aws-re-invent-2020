# Deploying Bolt for Java apps to API Gateway + AWS Lambda

# Create a new Slack app

## Create and enable a Slack app

* Go to https://api.slack.com/apps
* Create a new Slack app
* Go to *Features* > *OAuth & Permissions*
* Add `app_mentions:read` to *Scopes* > *Bot Token Scopes*
* Add `chat:write` to *Scopes* > *Bot Token Scopes*
* Go to *Settings* > *Install App*
* Click *Install to Workspace*
* Complete the installation process

## Grab the credentials

* SLACK_SIGNING_SECRET: *Settings* > *Basic Information* > *App Credentials* > *Signing Secret*
* SLACK_BOT_TOKEN: *Settings* > *Install App* > *OAuth Tokens for Your Team* > *Bot User OAuth Access Token*

# How to deploy this app to AWS

## Deploy the app

```bash
# install aws-cli v2 https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-mac.html
aws configure

npm i
export SLACK_BOT_TOKEN=xoxb-
export SLACK_SIGNING_SECRET=
export SERVERLESS_STAGE=dev
export SLS_DEBUG=*
./gradlew clean build &&
  ./node_modules/serverless/bin/serverless.js deploy --stage ${SERVERLESS_STAGE} --verbose &&
  ./node_modules/serverless/bin/serverless.js invoke --stage ${SERVERLESS_STAGE} --function warmup
```

## Configure the *Request URL* for Events API

* Go to https://api.slack.com/apps and choose your app
* Go to *Features* > *Event Subscriptions*
* Turn the feature on
* Set *Request URL* as `https://{your domain}.execute-api.us-east-1.amazonaws.com/dev/slack/events`
* Click *Subscribe to bot events*
* Add `app_mention` event
* Click *Save Changes* button

# Local development using Jetty

```bash
export SLACK_BOT_TOKEN=xoxb-
export SLACK_SIGNING_SECRET=
./gradlew run
# run `ngrok http 3000` in another terminal
```