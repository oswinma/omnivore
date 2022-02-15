# Omnivore

[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/omnivore-app/omnivore/Run%20tests)](https://github.com/omnivore-app/omnivore/actions/workflows/run-tests.yaml)
[![Discord](https://img.shields.io/discord/844965259462311966?label=Join%20our%20Discord)](https://discord.gg/nyqRrjujNe)
[![Twitter Follow](https://img.shields.io/twitter/follow/omnivoreapp)](https://twitter.com/OmnivoreApp)
![GitHub](https://img.shields.io/github/license/omnivore-app/omnivore)

<img align="right" src="https://avatars.githubusercontent.com/u/70113176?s=400&u=506b21d9f019f3160963c010ef363667fb24c7c9&v=4" height="150px" alt="Omnivore Logo">


[Omnivore](https://omnivore.app) is a complete, open source read-it-later solution for people who like text.

We built Omnivore because we love reading and we want it to be more social. Join us!

- Highlighting, notes, search, and sharing
- Full keyboard navigation
- Automatically saves your place in long articles
- Add articles via email (with substack support!)
- PDF support
- [Web app](https://omnivore.app/) written in node and typescript
- [Native iOS app](https://omnivore.app/install/ios)
- Progressive web app for Android users
- Browser extensions for [Chrome](https://omnivore.app/install/chrome), [Safari](https://omnivore.app/install/safari), [Firefox](https://omnivore.app/install/firefox), and [Edge](https://omnivore.app/install/edge)
- Tagging (coming soon!)
- Offline support (coming soon!)

Every single part is fully open source! Fork it, extend it, or deploy it to your own server.

We also have a free hosted version of Omnivore at [omnivore.app](https://omnivore.app/) -- try it now!

<img width="981" alt="omnivore-readme-screenshot" src="https://user-images.githubusercontent.com/75189/153696698-9e4f1bdd-5954-465b-8ab0-b4eacc60f779.png">

## Join us on Discord!

We're building our community on Discord. [Join us!](https://discord.gg/nyqRrjujNe)

Read more about Omnivore on our blog. <https://blog.omnivore.app/p/getting-started-with-omnivore>

## How to setup local development

The easiest way to get started with local development is to use `docker-compose up`. This will start a postgres container, our web frontend, an API server, and our content fetching microservice.

### Requirements for development

Omnivore is written in TypeScript and JavaScript.

* [Node](https://nodejs.org/) -- currently we are using nodejs v14.18
* [Chromium](https://www.chromium.org/chromium-projects/) -- see below for installation info

###  Running the web and API services

### 1. Start docker-compose

```bash
git clone https://github.com/omnivore-app/omnivore
cd omnivore
docker-compose up
```
This will start postgres, initialize the database, and start the web and api services.

### 2. Open the browser

Open <http://localhost:3000> and confirm Omnivore is running

### 3. Create a test account

Omnivore uses social login, but for testing there is an email + password
option.

Go to <http://localhost:3000/email-registration> in your browser.


### Frontend Development

If you want to work on just the frontend of Omnivore you can run the backend services
with docker compose and the frontend locally:

```bash
docker-compose up api content-fetch
cd packages/web
cp .env.local .env
yarn dev
```

### Running the puppeteer-parse service outside of Docker

To save pages you need to run the `puppeteer-parse` service.

### 1. Install and configure Chromium

```
brew install chromium --no-quarantine
export PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true
export CHROMIUM_PATH=`which chromium`
```

### 2. Navigate to the service directory, setup your env file, and install dependencies

```
cd packages/puppeteer-parse
cp .env.example .env
yarn
```

### 3. Start the service

```
yarn start
```

This will start the puppeteer-parse service on port 9090.

In your browser go to <http://localhost:3000/home>, click the `Add Link` button,
and enter a URL such as `https://blog.omnivore.app/p/getting-started-with-omnivore`.

You should see a Chromium window open and navigate to your link. When the service
is done fetching your content you will see it in your library.


## How to deploy to your own server

Omnivore was originally designed to be deployed on GCP and takes advantage
of some of GCP's PaaS features. We are working to make Omnivore more portable
so you can easily run the service on your own infrastructure. You can track
progress here: https://github.com/omnivore-app/omnivore/issues/25

To deploy Omnivore on your own hardware you will need to deploy three
dockerized services and configure access to a postgres service. To handle
PDF documents you will need to configure access to a Google Cloud Storage
bucket.

- `packages/api` - the backend API service
- `packages/web` - the web frontend (can easily be deployed to vercel)
- `packages/puppeteer-parse` - the content fetching service (can easily
be deployed as an AWS lambda or GCP Cloud Function)

Additionally, you will need to run our database migrations to initialize
your database. These are dockerized and can be run with the
`packages/db` service.

## License

Omnivore and our extensions to Readability.js are under the AGPL-3.0 license.

