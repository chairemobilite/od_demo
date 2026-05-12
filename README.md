# Demo Origin-Destination Survey

This is a very simple implementation of a travel survey with a trip diary from configuration, using the Evolution platform.

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
- [Environment Configuration](#environment-configuration)
- [Run the Application](#run-the-application)
  - [Participant Application](#participant-application)
  - [Administrative Application](#administrative-application)
- [Update the Application](#update-the-application)
- [Contributing to the Survey](#contributing-to-the-survey)
- [Using Generator](#using-generator)
- [Running Tests](#running-tests)

## Introduction

This survey uses the [evolution](https://github.com/chairemobilite/evolution) platform.

## Installation

To install, first clone this repository and the corresponding platform code:

```bash
git clone https://github.com/chairemobilite/od_demo.git
git submodule init
git submodule update --init --recursive

```

The installation process is the same as for the evolution platform. Follow the instructions in the `Installation` section of the README for the evolution version located in the `evolution` directory that was downloaded by the `git submodule` command above. This may not be the latest version of Evolution, and instructions on the platform main branch may not work.

Installation commands must be run from the root of this repository, not from the `evolution` directory. Do not forget to update the `.env` file with the correct values for your environment.

## Environment Configuration

If not already done, copy the sample file to create your environment configuration file.

```bash
cp .env.example .env
```

Do not forget to update `.env` with the correct values for your environment. You will likely need to change the following values:

```env
PG_CONNECTION_STRING_PREFIX = "postgres://postgres:@localhost:5432/"
EXPRESS_SESSION_SECRET_KEY = 'MYSECRETKEY'
GOOGLE_API_KEY = "MYGOOGLEAPIKEY"
GOOGLE_API_KEY_DEV = "MYGOOGLEAPIKEYFORDEVELOPMENT"
MAGIC_LINK_SECRET_KEY = "MYVERYLONGSECRETKEYTOENCRYPTTOKENTOSENDTOUSERFORPASSWORDLESSLOGIN"
GOOGLE_OAUTH_CLIENT_ID = "GOOGLEOAUTHCLIENTID"
GOOGLE_OAUTH_SECRET_KEY = "GOOGLEOAUTHSECRETKEY"
RESET_PASSWORD_FROM_EMAIL = "admin@test.com"
MAIL_TRANSPORT_SMTP_HOST = "smtp.example.org"
MAIL_TRANSPORT_SMTP_AUTH_USER = "MYUSERNAME"
MAIL_TRANSPORT_SMTP_AUTH_PWD = "MYPASSWORD"
MAIL_FROM_ADDRESS = "example@example.org"
```

## Run the Application

The survey is made of 2 distinct applications: one for web participants only, and another for administrators, interviewers, etc. Each application can run independently, and each is composed of 2 parts: client code and server.

### Participant Application

To run the *participant application*, build the client deployment package and start the server.

* `yarn build:dev` or `yarn build:prod` builds the client application in development mode (easier debugging) or production mode (minified and more performant), respectively.
* `yarn start` starts the server on port 8080.

Access the participant application at `http://localhost:8080`.

### Administrative Application

To run the *administrative application*, build the admin client and then start the server (example below on port 8082).

* `yarn build:admin:dev` or `yarn build:admin:prod` builds the client application in development or production mode, respectively.
* `HOST=http://localhost:8082 yarn start:admin --port 8082` starts the server on port 8082.

Access the administrative application at `http://localhost:8082`.

## Update the Application

To update the application, pull the latest version from the branch. There may also be changes to the Evolution version in use, so make sure to update it as well.

```bash
git checkout main
git pull origin main
yarn reset-submodules
yarn
yarn compile
yarn migrate
```

## Contributing to the Survey

To contribute to the survey and submit changes, use pull requests.

* Make sure you have the latest survey version by following the previous section.
* Create a working branch: `git checkout -b <branch-name>`.
* Make your changes on that branch.
* Once the branch is ready for review, push it upstream: `git push origin <branch-name>`.
* Then create a pull request on GitHub from the project's *Pull requests* tab.

A pull request can be a single commit or an entire branch. In the latter case, each commit must be independent and complete, with an explicit title. Titles such as `Fix typo` should be avoided.

It is possible to edit an existing commit to complete it or fix a typo. If the commit to edit is the latest one, simply use `git commit --amend` to include current changes in that commit.

To rewrite branch history, for example to modify an earlier commit (not the latest one) or combine commits, use interactive rebase: `git rebase -i HEAD~x` where `x` is the number of commits to review.

For more information about Git history rewriting tools, see the [Git manual](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History).

## Using Generator

This project uses Evolution-Generator to create surveys. To generate a survey, carefully follow the instructions in the `How to Run` section of the [Evolution-Generator README](https://github.com/chairemobilite/evolution/tree/main/packages/evolution-generator#how-to-run).

The source of truth for the questionnaire is the Excel workbook [OD_demo.xlsx](survey/references/OD_demo.xlsx). After editing it, regenerate the survey and locale files with:

```bash
yarn generateSurvey
```
