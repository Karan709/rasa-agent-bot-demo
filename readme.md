
# Rasa Agent Bot Demo 

This is a sample implementation of agent bot capabilities in chatwoot using [rasa](https://rasa.com/) . Rasa Open Source is a machine learning framework to automate text- and voice-based assistants.

You can refer the [rasa documentation](https://rasa.com/docs/rasa/user-guide/installation/) to get it up and running in your machine. 

This implementation isn't a recommended set up for production, but just to illustrate the capabilities of the platform. Please build on top of this ideas discussed to have in running in production.

## Steps to get the integration up and running

Follow the given steps to get your agent bot integration up and running

### Get a rasa project up and running. 

Go to a new directory and create a rasa project. If you have rasa installed in your machine you can get it up and running by follow in commands.  Refer [docs](https://rasa.com/docs/rasa/user-guide/rasa-tutorial/) to get the installation up and running. 

```
mkdir rasa
cd rasa
rasa init --no-prompt
```

go to `credentials.yml` file in the directory and ensure the following value is set. This is to ensure we can communicate with rasa through rest api

```
rest:
#  # you don't need to provide anything here - this channel doesn't
#  # require any credentials
```

start the rasa server with following command

```
 rasa run -m models --enable-api --log-file out.log
```

###  Get your chatwoot up and create an agent bot

go to your chatwoot directory and ensure your local server is running.  Start a rails console in your directory.

```
bundle exec rails c
```

Inside the rails console, type the following commands to create an agent bot and get its access token. Save the retrieved token as you would need it in further step.

```
bot = AgentBot.create!(name: "Rasa Bot", outgoing_url: "http://localhost:8000")
bot.access_token.token
```

Connect Agent Bot to your inbox by running the following command

```
AgentBotInbox.create!(inbox: Inbox.first, agent_bot: bot)
```

### Clone this repo into your machine and run the rasa router script. 

clone repo using the following command. 

```
git clone git@github.com:chatwoot/rasa-agent-bot-demo.git
```

open up the `rasa-router/index.php` file in your editor and change the follow values with appropriate ones. 

```
$rasa_url = 'http://localhost:5005';
$chatwoot_url = 'http://localhost:3000';
$chatwoot_bot_token = '<your agent bot token>';
```

run the php server  in the rasa-router directory

```
cd rasa-router
php -S localhost:8000
```

### Try connecting to your chatwoot webwidget and start a conversation. 

if you are on your local machine, you can access the widget through the test page

```
http://localhost:3000/widget_tests
```

## Notes 

You can also refer to the  [RasaHQ / rasa-demo](https://github.com/RasaHQ/rasa-demo) for adding additional capabilities to your bot. If training rasa through scripts isn’t your thing, check the exciting rasa projects which gives a UI to create your rasa stories. 
- [Botfront](https://github.com/botfront/botfront)
- [Articulate](https://github.com/samtecspg/articulate)

You can build on top of the ideas discussed here to implement your solutions. Refer to the [chatwoot api](https://www.chatwoot.com/developers/api/) to see the available options in chatwoot for your bots.Pretty excited to see what you guys come up with. 
