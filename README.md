# AWS + Deepgram Voice AI Hackathon: Pipecat Quickstart Project

This is the quickest way to get started building a Pipecat voice AI agent for the AWS + Deepgram Voice AI Hackathon. It was built with the [Pipecat CLI](https://github.com/pipecat-ai/pipecat-cli), and customized to make it as fast as possible to get a working bot.

To start, clone this repo, then `cd aws-deepgram-sa-hackathon`. From there, in your first terminal window:

## Running your bot locally 

```bash
cd server
cp example.env .env # and fill in the values
uv sync
uv run bot.py --transport daily
```

Then, in another terminal window:

```bash
cd client
npm i
cp example.env .env.local # and fill in the values
npm run dev
```

Then, visit http://localhost:3000 in your browser and click *Connect* to start talking to your bot!

If you're new to Pipecat, there's a great explanation of the botfile here, in the Pipecat docs:

https://docs.pipecat.ai/getting-started/quickstart#understanding-the-quickstart-bot

## Customizing the bot


The `server/bot.py` file contains your Pipecat bot. To get started customizing it, look for the phrase `You are a friendly AI assistant`. That `messages` variable is used by the bot's context manager, which stores the conversation between the bot and the user. You can change the `content` property of that first message to change your bot's system prompt.

Next, you'll almost certainly want to use function calling to extend your bot's functionality. Search for the word `weather` to see how this bot can answer questions about the weather (using fake data). You can read a lot more about function calling in [the Pipecat docs page about it](https://docs.pipecat.ai/guides/learn/function-calling#function-calling).

## Adding AWS functionality

TKTKTK

## Deploying to Pipecat Cloud

For the hackathon, you can perform your live demo by running the bot locally on your computer. But if you want to deploy your bot and deliver a hosted version, you can use [Pipecat Cloud](https://pipecat.daily.co).

To deploy your bot, first you'll need to login to DockerHub:

```
docker login
# create a personal access token at settings/personal-access-tokens/create
```

Then you'll want to set up the Pipecat CLI if you haven't already:

```
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

TKTKTK URL

Then, configure and deploy your frontend:

TKTKTK deploy frontend?!??!!?

⚠️  Cross-platform build support missing
   Building ARM64 images on x86_64 requires QEMU and binfmt_misc support.

   This is a one-time setup that persists across Docker restarts.


public bot key:
pk_8ddefb8b-f176-45e6-839d-7b0596429026


or this for github image build: https://github.com/daily-co/pipecat-cloud-deploy-action


