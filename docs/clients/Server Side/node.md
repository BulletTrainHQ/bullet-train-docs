---
title: Flagsmith NodeJS SDK
sidebar_label: NodeJS
description: Manage your Feature Flags and Remote Config in your NodeJS applications.
slug: /clients/node
---

This library can be used with server-side NodeJS projects. The source code for the client is available on
[Github](https://github.com/flagsmith/flagsmith-nodejs-client).

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing
purposes. See running in production for notes on how to deploy the project on a live system.

## Installing

VIA npm:

`npm i flagsmith-nodejs --save`

## Usage

### Retrieving feature flags for your project

```javascript
const flagsmith = require('flagsmith-nodejs');

flagsmith.init({
 environmentID: '<YOUR_ENVIRONMENT_KEY>',
});

flagsmith.hasFeature('header', '<My User Id>').then((featureEnabled) => {
 if (featureEnabled) {
  //Show my awesome cool new feature to this one user
 }
});

flagsmith.hasFeature('header').then((featureEnabled) => {
 if (featureEnabled) {
  //Show my awesome cool new feature to the world
 }
});

flagsmith.getValue('header', '<My User Id').then((value) => {
 //Show some unique value to this user
});

flagsmith.getValue('header').then((value) => {
 //Show a value to the world
});
```

## Available Options

| Property        |                                            Description                                            | Required |                    Default Value |
| --------------- | :-----------------------------------------------------------------------------------------------: | -------: | -------------------------------: |
| `environmentID` |  Defines which project environment you wish to get flags for. _example ACME Project - Staging._   |  **YES** |                             null |
| `onError`       |                 Callback function on failure to retrieve flags. `(error)=>{...}`                  |   **NO** |                             null |
| `api`           | Use this property to define where you're getting feature flags from, e.g. if you're self hosting. |   **NO** | https://api.flagsmith.com/api/v1 |
| `cache`         |  Defines an object containing 3 functions (`has(k)`, `get(k)`, `set(k,v)`) to cache API results   |   **NO** |                             null |

## Available Functions

| Property                       |                                                  Description                                                   |
| ------------------------------ | :------------------------------------------------------------------------------------------------------------: |
| `init`                         |                              Initialise the sdk against a particular environment                               |
| `hasFeature(key)`              |         Get the value of a particular feature e.g. `flagsmith.hasFeature("powerUserFeature") // true`          |
| `hasFeature(key, userId)`      | Get the value of a particular feature for a user e.g. `flagsmith.hasFeature("powerUserFeature", 1234) // true` |
| `getValue(key)`                |               Get the value of a particular feature e.g. `flagsmith.getValue("font_size") // 10`               |
| `getValue(key, userId)`        | Get the value of a particular feature for a specificed user e.g. `flagsmith.getValue("font_size", 1234) // 15` |
| `getFlags()`                   |                               Trigger a manual fetch of the environment features                               |
| `getFlagsForUser(userId)`      |                     Trigger a manual fetch of the environment features for a given user id                     |
| `getUserIdentity(userId)`      |         Trigger a manual fetch of both the environment features and users' traits for a given user id          |
| `getTrait(userId, key)`        |                         Trigger a manual fetch of a specific trait for a given user id                         |
| `setTrait(userId, key, value)` |                                    Set a specific trait for a given user id                                    |

## Identifying users

Identifying users allows you to target specific users from the [Flagsmith dashboard](https://www.flagsmith.com/). You
can include an optional user identifier as part of the `hasFeature` and `getValue` methods to retrieve unique user flags
and variables.
