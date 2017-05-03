SparkPost Bounce Forwarding Service
===================================

A small Heroku service that will consume webhook POSTs for in-band and out-of-band
bounces and forward them through the Transmissions API to a mailbox.

Deployment
----------

Start by clicking on the following button:

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy?template=https://github.com/SparkPost/sp-bounce-forwarding-service)

Once the deployment completes click on the "View" button under "Your app
was successfully deployed". (Alternatively browse to
`https://<your-app-name>.herokuapp.com/index.html`.)

If a `FORWARD_FROM` address was chosen other than the default
`forward@sparkpostbox.com` then a sending domain will need to be
[created](https://support.sparkpost.com/customer/portal/articles/1933318)
and verified. To get to the SparkPost UI browse to the "Resources" tab
of the newly created app in the [Heroku
Dashboard](https://dashboard.heroku.com/apps), and then click
"SparkPost".

Deploying Manually
------------------

1.  Register for an account with [Heroku](https://signup.heroku.com) and
    install the Heroku [Toolbelt](https://toolbelt.heroku.com) for your
    operating system. Then log in:

        heroku login

2.  Clone the repository and install:

        git clone git@github.com:SparkPost/sp-bounce-forwarding-service.git
        cd sp-bounce-forwarding-service
        npm install

3.  Create the heroku app:

        heroku create

4.  Configure the required add-ons:

        heroku addons:create heroku-redis:hobby-dev
        heroku addons:create sparkpost:free

    Note: The SparkPost add-on will automatically create a SparkPost
    account and set up the appropriate key. If you already have an
    account and wish to use that do not run the
    `addons:create sparkpost:free` command. Then set the following
    config var:

        heroku config:set SPARKPOST_API_KEY=<your-api-key-here>

5.  Configure the app:

        heroku config:set FORWARD_FROM=<the-from-address-to-use>
        heroku config:set FORWARD_TO=<the-recipient-of--forward-messages>

6.  Deploy the app:

        git push heroku master

7.  Complete the setup by browsing to the following page:

        curl https://<your-app-name>.herokuapp.com/index.html

## SparkPost Enterprise
At step 5 above - manually create the following additional env variables:

```bash
heroku config:set SPARKPOST_API_URL=https://demo.sparkpostelite.com         # Example only - put your domains here
heroku config:set RETURN_PATH=steve@demo-t.sparkpostelite.com
heroku config:set BINDING=outbound
```

## Bounce types

If you really want only one type of bounce to be reported, you could change the webhook creation in `function addWebhook(appUrl)` 
or else edit the webhook grants after creation.

