# Simple website deployed to Cloud Foundry via Concourse

This is an example of a simple website that you can host on your cloud foundry environment, using Concourse for automated deployments.  Just check in your code and let Concourse take care of the rest.

Prerequisites:
1. A Cloud Foundry environment you can use
2. A Concourse environment you can use
3. The Concourse Fly CLI installed.  You can install from your concourse home page, see the icons in the lower right corner.

Try it!

1. Fork the repo to your own github account

2. Clone the repo to your workstation

```
cd
git clone https://github.com/YOUR-GITHUB-ACCOUNT/website-demo
cd website-demo
```

3. Create a credentials.yml file with the following contents in the `website-demo` directory.  Replace the placeholders with your cloud foundry environment information.  **Be sure to include credentials.yml in your .gitignore file in the website-demo directory so you don't upload it when you commit your changes.**

```
cf-api: https://YOUR-CF-API-ENDPOINT
cf-username: YOUR-CF-USERID
cf-password: YOUR-CF-PASSWORD
cf-organization: YOUR-CF-ORG
cf-space: YOUR-CF-SPACE
```

4. Login to concourse using the Fly CLI.  You'll be prompted for your concourse userid and password.  If you are logging in to a specific team, add ` -n YOUR-CONCOURSE-TEAM-NAME ` at the end.

```
fly -t YOUR-CONCOURSE-TARGET login -c http://YOUR-CONCOURSE-ENDPOINT
```

5. Create the pipeline

```
fly set-pipeline --target YOUR-CONCOURSE-TARGET --config pipeline.yml --pipeline deploy-website-demo --non-interactive --load-vars-from ./credentials.yml
fly unpause-pipeline --target YOUR-CONCOURSE-TARGET --pipeline deploy-website-demo
```

6. To see your pipeline in concourse, copy URL from the output of the command above to your browser.  Login using same userid/password you did for the fly CLI.

7. Click on deploy-website-demo, then kick off a manual build by clicking on the plus sign at the upper right corner of the page.  The build should start after a few seconds.  Look at the build output to see your website pushed to cloud foundry.

8. Now you're ready to start automating.  Edit the `index.html` file to change the text or color, then commit and sync your changes.  Within a minute or so, you should see a new build kick off.  Get the URL for your app from the output of the build, and bring it up in a browser.  The website should have the changes you just made.

9. Now you just need to check in your code, and Concourse will handle your deployment to Cloud Foundry.

10. When you're done, you can destroy the pipeline:

```
fly -t YOUR-CONCOURSE-TARGET destroy-pipeline -p deploy-website-demo
```

11. Explore the [fly CLI](http://concourse.ci/fly-cli.html).
