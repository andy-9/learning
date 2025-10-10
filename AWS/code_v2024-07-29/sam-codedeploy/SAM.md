<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Step 1 - Download a sample application](#step-1---download-a-sample-application)
- [Step 2 - Build your application](#step-2---build-your-application)
- [Step 3 - Package your application](#step-3---package-your-application)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

From: https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-getting-started-hello-world.html

# Step 1 - Download a sample application
sam init --runtime python3.9

# Step 2 - Build your application
cd sam-app
sam build

# Step 3 - Package your application
sam deploy --guided