# heroku-fluentd-coralogix

This is a [Heroku](https://heroku.com) application based on
[FluentD](https://fluentd.org), accept logs from [HTTPS
drains](https://devcenter.heroku.com/articles/log-drains#https-drains) 
and send it [Coralogix](https://coralogix.com/).

## Setup

Check out this repository and create a Heroku application from it:

```bash
$ export LOGGER_APPLICATION=fluentd-coralogix-`uuidgen`
$ heroku apps:create ${LOGGER_APPLICATION}
$ heroku config:set PRIVATE_KEY=YOUR_PRIVATE_KEY -a ${LOGGER_APPLICATION}
$ git push heroku master
```

Also you can overwrite `Application` and `Subsystem` names:

```bash
$ heroku config:set APP_NAME=YOUR_APP_NAME -a ${LOGGER_APPLICATION}
$ heroku config:set SUB_SYSTEM=YOUR_SUBSYSTEM -a ${LOGGER_APPLICATION}
```

## Usage

Configure Heroku HTTPS log drains to publish logs to this application:

```bash
$ heroku drains:add https://YOUR-LOGGING-APP-NAME/coralogix.APP_NAME.SUB_NAME -a YOUR-APP-NAME
```

## Private space logging

For logging in private space your need to deploy logging application and set it as logs drain:

```bash
$ export LOGGER_APPLICATION=fluentd-coralogix-`uuidgen`
$ heroku apps:create ${LOGGER_APPLICATION} --internal-routing --space YOUR_SPACE_NAME
$ heroku config:set PRIVATE_KEY=YOUR_PRIVATE_KEY -a ${LOGGER_APPLICATION}
$ heroku config:set APP_NAME=YOUR_SPACE_NAME -a ${LOGGER_APPLICATION}
$ git push heroku master
$ heroku drains:set https://${LOGGER_APPLICATION}.herokuapp.com/private --space YOUR_SPACE_NAME
```
