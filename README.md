# AWS + Deepgram Voice AI Hackathon: Pipecat Quickstart Project

This is the quickest way to get started building a Pipecat voice AI agent for the AWS + Deepgram Voice AI Hackathon.

It was built with the [Pipecat CLI](https://github.com/pipecat-ai/pipecat-cli), and customized to make it as fast as possible to get a working bot.

## Dependencies
- Python 3.10+
- `uv`
- docker (to deploy to Pipecat Cloud)

## Run your bot locally

```bash
git clone git@github.com:pipecat-ai/aws-deepgram-sa-hackathon.git
cd aws-deepgram-sa-hackathon
```

### Server
In your first terminal window:
```bash
cd server
cp env.example .env # and fill in the values
uv sync
uv run bot.py --transport daily
```

You should see something like:
```bash
INFO     | pipecat:<module>:14 - ᓚᘏᗢ Pipecat 0.0.102 (Python 3.12.0 (main, Oct  2 2023, 20:56:14) [Clang 16.0.3 ]) ᓚᘏᗢ

🚀 Bot ready!
   → Open http://localhost:7860 in your browser to start a session

INFO:     Started server process [91430]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://localhost:7860 (Press CTRL+C to quit)
```

### Client
Then, in another terminal window:

```bash
cd ../client
cp env.example .env.local # and fill in the values
npm i
npm run dev
```

You should see something like:
```bash
  VITE v7.3.1  ready in 206 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: http://192.168.0.16:5173/
  ➜  Network: http://100.115.25.125:5173/
  ➜  press h + enter to show help
```

Now visit http://localhost:5173 in your browser and click **Connect** to start talking to your bot!

## Customize the bot

The `server/bot.py` file contains your Pipecat bot. To customize it, look for the comment `#### Customize bot prompt here! Update "content"`. That `messages` variable is used by the bot's context manager, which stores the conversation between the bot and the user. Change the `content` property of that first message to update your bot's system prompt.

Next, you'll almost certainly want to use function calling to extend your bot's functionality. Search for the comments `#### Customize function here!` to see how this bot can answer questions about the weather (using fake data). Read more about function calling in [the Pipecat docs page about it](https://docs.pipecat.ai/guides/learn/function-calling#function-calling).

## Add AWS functionality

Coming soon!

## Deploy to Pipecat Cloud

For the hackathon, you can perform your live demo by running the bot locally on your computer. To deliver a hosted version, use [Pipecat Cloud](https://pipecat.daily.co).

To deploy your bot, first you'll need to login to DockerHub:

```
docker login
# create a personal access token at settings/personal-access-tokens/create
```

Then you'll want to set up the Pipecat CLI if you haven't already:

```bash
uv tool install "pipecat-ai-cli[tail]"
pipecat cloud auth login
pipecat cloud secrets image-pull-secret my-image-pull-secret https://index.docker.io/v1/ # use your dockerhub username, but for password, put your personal access token
pipecat cloud docker build-push -u <your_dockerhub_username>
nvim pcc-deploy.toml # and add your username to the image path
pipecat cloud secrets set --file .env aws-deepgram-2026-03-secrets
pipecat cloud deploy # pcc_deploy.toml is configured to use my-image--pull-secret
```
Talk to your agent in the Pipecat Cloud Sandbox:

```bash
# create a public API key so you can start bot sessions
```

Then, configure and deploy your frontend:

Coming soon!

or this for github image build: https://github.com/daily-co/pipecat-cloud-deploy-action

## Troubleshooting

### required .env variables for Server
- `DAILY_API_KEY` : found at https://pipecat.daily.co/your-org/settings/keys in `Daily` section.
- `DEEPGRAM_API_KEY`
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `AWS_REGION`

### required .env variables for Client
- For local dev (these are already set in `client/env.example`):
	- VITE_BOT_START_URL="http://localhost:7860/start"
	- VITE_PIPECAT_TRANSPORT=daily

- For Pipecat Cloud:
	- `VITE_BOT_START_URL` : "https://api.pipecat.daily.co/v1/public/{agentName}/start"
	- `VITE_PIPECAT_TRANSPORT` : (same as local dev)
	- `VITE_BOT_START_PUBLIC_API_KEY` : create at https://pipecat.daily.co/{yourOrgName}/settings/keys
