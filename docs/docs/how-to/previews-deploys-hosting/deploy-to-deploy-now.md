---
title: Deploying to Ionos Deploy Now
---

This guide shows you how to deploy your next [framework-name] project on [IONOS Deploy Now](https://www.ionos.com/hosting/deploy-now). 

[Read more in the Deploy Now docs](https://docs.ionos.space/)

## Introduction
****
Deploy Now is a hosting platform for deploying static sites and PHP apps via GitHub. They set up a GitHub Actions workflow that builds and deploys your project and run your site on highly available, green [IONOS](https://www.ionos.com/) infrastructure in Europe and North America. 

Deploy Now provides SSL encryption, staging environments, visitor stats, DDoS-protection and more.

## Assumptions
You have an account with [Deploy Now](https://www.ionos.com/hosting/deploy-now). 
You have a [framework-name] project ready for deployment

## Setup 
When entering Deploy Now for the first time, you will be asked to grant read and write permissions to your repository. Afterwards, you can directly set up your first project. [framework-name] projects are supported in the 3 Starter Projects included in your membership. 

**Select a Repository**
You can either select a repository from your account or paste in the URL of a third party repository. Or simply deploy a [framework-name] starter project by hitting the button below.

[![Deploy to IONOS](https://images.ionos.space/deploy-now-icons/deploy-to-ionos-btn.svg)](https://ionos.space/setup?repo=https://github.com/your-account/your-repo)

**Configure your build**
Deploy Now will analyze your repository to provide matching build commands and a publish directory. You can add additional environment variables. Secret environment variables will be stored in [GitHub secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets).


**Review your configuration and create your project**
After finishing the setup, GitHub Actions will instantly start the build process. You can navigate to GitHub to check the build logs.

## Continuous deployment with GitHub Actions
After the setup, you will find a workflow description in your repository under `.github/workflows/deploy-now.yaml`. Each workflow will be triggered on Git Push, start with retrieving metadata from IONOS and checking out your repository and end with deploying assets to the server.

Between these steps, Deploy Now will add the following step to the workflow. It will install Node.js, which is required to execute your build steps. This is where you can update the node version if required.

```
     - name: Setup Node
        if: ${{ steps.project.outputs.deployment-enabled == 'true' }}
        uses: actions/setup-node@v1
        with:
          node-version: v16.x
```
Below, you will find the actual build step, where you can adapt build commands or add additional environment variables later on.

```
     - name: Build Node assets
        env:
          CI: true
          SITE_URL: ${{ steps.project.outputs.site-url }}
        run: |
          npm ci
          npm run build
```

## Server configuration

Websites get deployed to a shared hosting infrastructure using [Apache HTTD servers](https://httpd.apache.org/). You can define redirects and rewrites our optimize performance and security via [.htaccess](https://httpd.apache.org/docs/current/howto/htaccess.html). 

[Learn more](https://docs.ionos.space/docs/apache-configuration-htaccess/)

## Staging environments

In addition to your productive branch, you can deploy additional branches to staging environments. Simply open a new feature branch, and Deploy now will deploy it under a preview URL.

## Connect a domain

By default, Deploy Now publishes your projects under generated preview URLs. You can connect custom domains to your production deployment directly by clicking on the settings wheel on the project detail page. If you want to connect a domain from another provider, you need to set it up as an [external domain](https://www.ionos.com/help/domains/set-up-and-manage-an-external-domain-at-11-ionos/setting-up-an-external-domain-at-11-ionos/?source=helpandlearn) first.

Both, preview URLs and custom URLs are automatically SSL-encrypted.  






