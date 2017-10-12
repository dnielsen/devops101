# Chapter 1: AWS EC2 Sample Deployment

## Introduction
This document provides detailed, step-by-step instructions on creating a new AWS EC2 instance and deploying a provided Symfony2 application on it using LAMP (Linux, Apache, MySQL, PHP) stack. The application also uses a separate Redis database for certain parts of it, but the connection to Redis is not included in these instructions. These instructions are detailed for use with Mac OS; however, once logged onto the virtual machine provided by your EC2, the instructions are universal. The provided application is a website call “Spoutlet” and it will be cloned from an online github repository.

## Sign up for AWS Free Tier 
- Sign up for Free Cloud Services – AWS Free Tier https://aws.amazon.com/free/
- “AWS Free Tier includes offers that expire 12 months following sign up and others that never expire.”

## Create AWS EC2 instance
- a) Log into AWS console and select “Services” > “EC2”
- b) Click on “Instances”

## Launch Instance
- a) Click on “Launch Instance”
- b) Select “Ubuntu Server 16.04...”
- c) Select the t2.medium size instance - VERY IMPORTANT
- click Next until you reach “Step 6: Configure Security Groups”
- Or Click on the “Step 6: Configure Security Groups” tab, located along the top of the screen.
- d) Change “security group name” to “spoutlet - sample deployment”
- e) Click “Add Rule”
- Add “Custom TCP Rule” with Port Range = 8000
- Change “Source” drop down menu to “Anywhere”
- Add “Custom TCP Rule” with Port Range = 0
- Change “Source” drop down menu to “Anywhere”
- f) Click “Add Rule”
- Add “HTTP”
- Change “Source” drop down menu to “Anywhere”
- g) Click “Review and Launch”, then review settings and click “Launch”
- h) A pop up window will appear asking about a security key
- Select “Create a new key pair” from the first drop down box
- Enter “spoutlet_sampleEC2” for the key pair name, then “Download Key Pair”
- ***IMPORTANT*: after downloading the key, place it in a safe and accessible place (perhaps create a backup copy and store it in the cloud); if lost, your key cannot be replaced, as AWS does not store your key after its creation
- **i) After storing your key in a safe and secure location, click “Launch Instances”




```bash
pip install -r requirements.txt
```

## Setting up Your App

Now that we've got our local environment set up we'll need to create a new Slack App. :tada:

### Creating a New Slack App on [api.slack.com](https://api.slack.com/apps?utm_source=events&utm_campaign=build-bot-workshop&utm_medium=workshop)

In your browser, on [api.slack.com/apps](https://api.slack.com/apps?utm_source=events&utm_campaign=build-bot-workshop&utm_medium=workshop) you'll find
a green button labeled [Create New App](https://api.slack.com/apps/new?utm_source=events&utm_campaign=build-bot-workshop&utm_medium=workshop) on the
top right of the page.

![create_new_slack_app](https://cloud.githubusercontent.com/assets/4828352/20548492/b4270b52-b0d8-11e6-94cb-ea307342ebb9.png)

_Push the tempting button_  :point_right:   :white_check_mark:

You'll be directed to your shiny new app's **Basic Information** page. We'll come back to you soon, _basic information_.

### Adding a Bot User

But first, let's get ourselves a shiny new **Bot User** so our app can communicate on Slack. On the left side navigation you'll find the **Bot Users** tab where you can create a new bot user for your app.

![app_settings_nav_bot_user](https://cloud.githubusercontent.com/assets/4828352/20548580/8826d680-b0d9-11e6-96bc-84cfdabff6f4.png)

![add_bot_user](https://cloud.githubusercontent.com/assets/4828352/20548602/c67f367a-b0d9-11e6-85eb-b2069120da1e.png)

Once you've got your fancy new automaton, we have it subscribe to events in Slack!

### Subscribe to Events

By using Slack's [Events API](https://api.slack.com/events-api?utm_source=events&utm_campaign=build-bot-workshop&utm_medium=workshop) we can ask Slack to send us a JSON payload of information when something particular happens inside of Slack. If we want our bot respond when a user says hello to our bot in a DM, we can have our bot subscribe to `message.im` and when a user posts a message, Slack will send the information about the message event to the URL we specify. If the message matches our criteria we can choose to respond to it with an additional step. Unneeded events are discarded.

On the left navigation bar of your app's settings page you'll find **Event Subscriptions**.
To start off right, go ahead and subscribe your bot to `message.im` events under the **Bot Events** section of the page.

![add_bot_events](https://cloud.githubusercontent.com/assets/4828352/23191396/364af9fc-f852-11e6-9206-5aeb4ca2018a.png)


After you've subscribed to all the events your app will need, make sure to **Save Changes**.

![save_changes](https://cloud.githubusercontent.com/assets/4828352/23178485/716f367a-f81f-11e6-8967-a35893a2cd41.png)


### App Credentials

Let's revisit that tempting **Basic Information** page. Here you'll find your app's **Client ID**, **Client Secret** and **Verification Token** under the _App Credentials_ section.

![app_credentials](https://cloud.githubusercontent.com/assets/4828352/22839775/bd5af4f0-ef7f-11e6-84bf-33fe79b374f4.png)

The Client ID and Client Secret are used to validate your app's authenticity during the OAuth negotiation process. The Verification Token is used to verify requests sent by Slack and received by your server.

Just like you wouldn't graffiti your email username and password at the bus stop, it's important to prevent your _App Credentials_ from becoming part of a public repository. To protect your app's secrets, this project exports these secrets to your local environment.

If you're using Bash or Zsh and your virtual environment is activated, you can export your app's secrets like this:

:warning: _Remember to have your virtualenv activated before exporting environment variables._

```bash
export CLIENT_ID='XXXXXXXXXXX.xxxxxxxxxx'
export CLIENT_SECRET='xxXXxxXXXXXxxxxXXX'
export VERIFICATION_TOKEN='xxxXXXxxXXxxX'
```

If you're using Windows, you can export your app's secret's like this:

```dos
set CLIENT_ID='XXXXXXXXXXX.xxxxxxxxxx'
set CLIENT_SECRET='xxXXxxXXXXXxxxxXXX'
set VERIFICATION_TOKEN='xxxXXXxxXXxxX'
```

Our app will grab these secrets from our environment.

## Let's Make an [app.py](app.py)


```bash
python app.py
```
![start_appy](https://cloud.githubusercontent.com/assets/4828352/20549064/cad48f8c-b0dd-11e6-8a85-25bff2815d2e.png)

Check our your shiny app is running locally in a browser by navigating to   [localhost:5000/install](http://localhost:5000/install). :boom:

## You Did It! :sparkles:

You're all set up! Time to check out the next chapter and move on to OAuth.

```bash
git checkout chapter-2
```

---
###### Documentation Navigation
**Next [Chapter 2](./../docs/Chapter-2.md)**  
**Previous [README](./../docs/README.md)** 
