# Ding Dong Slack Bot

This Slack bot allows a student to ring the doorbell to alert the staff to open the door and gives them the ability to click a button to let the staff know they got in. A student will also be able to check in and out of class. The check in and out times will be logged for each student. A student will be able to submit a daily stand up report. Staff can view all submitted stand ups for the day in a private channel.

## Setup

Install all dependencies:

```
$ npm install
```

Add environment variables to `.env`. See engineers for credentials. The STUDENTS_CHANNEL is where the students make requests from. The KEY_CHANNEL is where the staff is alerted if somebody rings the door. The ADMIN_REPORTS_CHANNEL is where the staff can view the daily standups that have been submitted.

```
OAUTH_ACCESS_TOKEN=
BOT_USER_OAUTH_ACCESS_TOKEN=
STUDENTS_CHANNEL= <current students channel ID>
KEY_CHANNEL= <key channel ID>
ADMIN_REPORTS_CHANNEL=< admin reports channel ID>
SLACK_URL= <base url for Slack Workspace>
BASE_URL= <server host base url>
TEST_USER_ID= <any user ID in knock channel>
LAT= <Latitude for checkin/checkout location>
LONG= <Longitude for checkin/checout location>

Optional
EXTERNAL_LOGGING= <truthy - only runs if value is present>
EXTERNAL_LOGGING_URL= <url to send external logs>
EXTERNAL_LOGGING_TOKEN= <token sent in Post body to external logging url>

```

### Create a production bot

1.  Create an app at https://api.slack.com/apps
    - Add a distinct app name

2.  Create a bot in ‘Bot Users’
    - Add a distinct bot Display Name
    - Add a bot Default Name (should be same or similar to Display Name)

3.  Invite bot to workplace
    - Go to ‘Basic Information
    - Select ‘Install your app to your workspace’
    - Click ‘Install App to Workspace’ button
    - Confirm identity

4.  Invite bot to current students, key, and admin reports channels
    - Go to workplace in the Slack App
    - Click on current students channel
    - Click on settings (gear icon)
    - Select Apps and click on ‘Add app’
    - Select your bot and click ‘Add’
    - Repeat for key channel and admin reports channel
    - NOTE: Save student, key, and admin reports channel IDs for environment variables: right click on the channel and open the url (channel ID will be at the end of the url after '/messages')

5.  Add Button and Slash Command urls in the Slack API
    - Return to https://api.slack.com/apps
    - ‘Interactive Components’\
    Request Url: https://dingdong-slack-bot.herokuapp.com/slack/interactive
    - ‘Slash Commands’\
    /doorbell : https://dingdong-slack-bot.herokuapp.com/slack/doorbell \
    /checkin : https://dingdong-slack-bot.herokuapp.com/slack/checkin \
    /checkout : https://dingdong-slack-bot.herokuapp.com/slack/checkout \
    /standup : https://dingdong-slack-bot.herokuapp.com/slack/standup 

6.  Add Environmental Variables to .env

### Create a local dev bot for each coder for testing
Note - Names for App, Bot, Interactivity, and Slash Commands for local dev bots must be distinct from production bot (use your initials example: jsdingdongbot or /jsdoorbell)

1.  Create an app at https://api.slack.com/apps
    - Add a distinct app name

2.  Create a bot in ‘Bot Users’
    - Add a distinct bot Display Name
    - Add a bot Default Name (should be same or similar to Display Name)

3.  Invite bot to workplace
    - Go to ‘Basic Information
    - Select ‘Install your app to your workspace’
    - Click ‘Install App to Workspace’ button
    - Confirm identity

4.  Invite bot to current students, key, and admin reports channels
    - Go to workplace in the Slack App
    - Click on students channel
    - Click on settings (gear icon)
    - Select Apps and click on ‘Add app’
    - Select your bot and click ‘Add’
    - Repeat for key channel and admin reports channel
    - NOTE: Save student, key, and admin reports channel IDs for environment variables: right click on the channel and open the url (channel ID will be at the end of the url after '/messages')
    
5.  Add Button and Slash Command localtunnel urls in the Slack API
    Remember - use initials for the local dev bot Slash Commands.
    
    - Return to https://api.slack.com/apps
    - ‘Interactive Components’\
    Request Url: {localtunnel url}/slack/interactive
    - ‘Slash Commands’\
    /{initials}doorbell : {localtunnel url}/slack/{initials}doorbell\
    /{initials}checkin : {localtunnel url}/slack/{initials}checkin\
    /{initials}checkout : {localtunnel url}/slack/{initials}checkout\
    /{initials}standup : {localtunnel url}/slack/{initials}standup

6.  Add Environmental Variables to .env

## Start Server
### Production Server
```
npm run build
```

```
npm run start
```

### Dev Server
1.  Install Local Tunnel (or Ngrok)
```
npm install -g localtunnel
```

2.  Run Local Tunnel
```
lt --port 3000 --subdomain <devsubdomain>
```
3.  Run Dev Server (on 2nd terminal)
```
npm run start
```

## Usage
 DingDongbot lives in *Slack* 
  Upon installation it has access to the *Current Students* (Students Channel), *Key* channel and *Admin Reports* channel (where the magic happens).
 
 ## The commands are entered: ##
 >in the *message input* in any sdcs slack channel.
 
**Type in** 
> - **/doorbell** to ring the bell (This sends a message to the *key* channel - alerting the staff to open front door). 
> - a *"Got in"* button will be displayed in a private channel to the user, this is pressed to notify the staff that the user is or is not in the building. 

> - Type in **/checkin** to check in.
(a link to the geolocation landing page to verify location and checkin ability will be displayed.) Click the link to go to the Geolocation website.  Click the find me button, wait for the coordinates to appear, and then click submit to check in.
(This creates a timestamp on the server, the user will also be sent a reminder by 9:00am to checkin, if they haven't before hand) :clock9:

> - Type in **/checkout** to check out. 
(This creates a timestamp on the server, the user will also be sent a reminder by 5:50PM to checkout before they leave.)
Click the link to go to the Geolocation website.  Click the find me button, wait for the coordinates to appear, and then click submit to check out.

-if they havent fully checked out by 6:15pm another reminder will be sent (Are you still here?) with two buttons (Yes/No).

-If **yes** is selected (they will be sent a link to the geolocation landing page to verify location and checkout will be displayed.)

-If **no** it will automatically check them out and add a location penalty to their log.

-If the user is still not checked out by 6:20pm,  a message will be displayed informing them that they are automatically checked out. Also a penalty will added to their log. :clock630:

> - Type in **/standup** to trigger the standup dialog.
(a survey form will appear with three stand up questions for the students to submit to the staff. Students can cancel the form and be prompted to remember to submit later on in the day.)

- All submitted stand ups will be cleared from cashe at 11:59pm.

## Back End

Loopback used for server setup. Please view server/boot folder for models/relationships.

:copyright: 2019 NOWW/SDCS

