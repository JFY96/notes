Popular cloud-based PaaS service.

# Setup

Make account and install the Heroku CLI

Then to confirm installation by checking version
```bash
heroku version
```

## Add SSH Key to Heroku

Similar to GitHub, adding your SSH key lets Heroku know what machine the commands are coming from:

```bash
heroku keys:add
```

# Why Heroku?

- Has free tier (*really* free but with limitations)
- *PaaS* so it takes care of alot of the web infrastructure for you. Easier to get started as you don't need to worry about servers, load balancers, reverse proxies, restarting on crashes, or any of other web infrastructure provided.
- The limitations on a free tier may not affect applications with low use or for demonstration:
	- free tier only provides short-lived storage. User uploaded files are not safely stored on Heroku itself.
	- free tier will sleep an inactive web app if there are no requests within a half-hour period, and may take serveral seconds to respond
	- free tier limits website to a certain amount of hours of runtime each month ("asleep" time is not calculated)

Heroku (free tier) is perfect for hosting demonstrations but may not be ideal for a real website. However, it is easy to set up and scale, so you can pay to add speed or uptime or add-on features.

# How it works

Heroku runs websites with "Dynos" - isolated, virtualized unix containers that provide the environment required to run an application. These are completely isolated and have an ephemeral file system (short-lived, cleaned and emptied every time dyno restarts). Since nothing is shared between them, it is easy to scale horizontally by adding more. A load balancer is used internally to distribute web traffic to all web dynos, and one thing they do share are the app configuration variables.

Since file system is ephemeral, services (e.g. databases, queues, caching, storage, email services) are considered add-ons. Once attached (Charges may apply to services), add on services are accessed via environment variables.

In order to execute your application, Heroku requires configuration to set up the environment for your application dependencies, and needs to know how to start it.

For Node apps, all it needs is `package.json`.

Heroku is integrated with `git` and the Heroku client will use git to synchronize changes you upload. When you want to deploy your site, you sync your changes to the Heroku repository.

# Create and upload website

After doing any dev work to get your app ready (e.g. for `node` apps, setting node version, getting db uri from env variable in code), we can start running `herkou` commands.

To create the app:

```bash
heroku create
```

This will create a git remote named `heroku` in our local git environment.

We can then push the app to the Heroku repo. This will upload the app, get all dependencies, package it in a dyno, and start the website. 
```bash
git push heroku main
```
Note if we need to update the code or the build/deployment failed we can run this again after making changes.

If it is successfully deployed, then visit the website using
```bash
heroku open
```

## Setting configuration variables

Inspecting configuration variables
```bash
heroku config
```

Setting a variable
```bash
heroku config:set NODE_ENV='production'
```
Updating a variable will restart your app

## Debugging

```bash
heroku logs # show current logs
heroku logs --tail # show current logs and keep updating with new results
heroku ps # display dyno status
```


