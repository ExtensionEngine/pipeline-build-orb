# Orb Template


[![CircleCI Build Status](https://circleci.com/gh/ExtensionEngine/pipeline-build-orb.svg?style=shield "CircleCI Build Status")](https://circleci.com/gh/ExtensionEngine/pipeline-build-orb) [![CircleCI Orb Version](https://badges.circleci.com/orbs/pipeline-build-orb/pipeline-build-orb.svg)](https://circleci.com/developer/orbs/orb/pipeline-build-orb/pipeline-build-orb) [![GitHub License](https://img.shields.io/badge/license-MIT-lightgrey.svg)](https://raw.githubusercontent.com/ExtensionEngine/pipeline-build-orb/master/LICENSE) [![CircleCI Community](https://img.shields.io/badge/community-CircleCI%20Discuss-343434.svg)](https://discuss.circleci.com/c/ecosystem/orbs)


A project template for Orbs.

This repository is designed to be automatically ingested and modified by the CircleCI CLI's `orb init` command.

_**Use this orb to build NodeJS projects and publish to AWS ECR.**_

---

## Resources

[CircleCI Orb Registry Page](https://circleci.com/developer/orbs/orb/pipeline-build-orb/pipeline-build-orb) - The official registry page of this orb for all versions, executors, commands, and jobs described.

[CircleCI Orb Docs](https://circleci.com/docs/orb-intro/#section=configuration) - Docs for using, creating, and publishing CircleCI Orbs.

# Setting up OpenID for AWS

## Intro

Instead of using AWS access key, we use OpenID for AWS auth. We want to use OpenID
credentials because they expire soon after the job is completed and
we don't need to set up AWS keys and keep them secret.

## Setup

CircleCI Guide is available here https://circleci.com/docs/openid-connect-tokens/

1. Go to https://app.circleci.com and copy your `Organization ID`
    which is listed on the `Organization Settings` page in CircleCI app.
2. Log into AWS, go to IAM, then Identity providers page and click `Add provider`.
3. In the "Add an Identity Provider" form, select `OpenID Connect` type.
    Under "Provider URL", write `https://oidc.circleci.com/org/<organization-id>`
    with org ID obtained in step 1. Click `Get thumbprint` button.
    Under Audience input, paste the same org ID (but just the ID, without URL
    or anything else). Click `Add provider` button to save this provider.
4. Go back to IAM, to Roles page and click `Create role` button.
5. In the create role form, select `Web identity`, then in "Identity provider" select
    URL of the provider from step 3, and for "Audience", select the only available
    option which is your CircleCI organization ID. Click Next.
6. On the Add permissions page, select permission you want to add. Click Next.
    TODO - create permissions only for the scope of this orb.
7. Finally, add name and description to the role, review permissions and save.
8. Back in IAM, Roles page, click on the newly created role and copy its ARN.
    Use that ARN when calling `build_server` job from this orb.
