---
title: 'Supertab SDK'
description: 'Learn how to use Supertab SDK'
---

## Introduction

The Supertab Browser SDK allows developers to integrate Supertab functionality into their web applications. It provides a set of tools for managing user authentication, handling purchases, and interacting with the Supertab API.

## Installation

To install the Supertab Browser SDK, use npm:

```javascript
npm install @getsupertab/supertab-browser
```

Make sure you have Node.js version 21.0.0 or higher and npm version 10.0.0 or higher installed.

## Initialization

The Supertab SDK can be initialized in two ways:

### 1. Using `window` global variables

```javascript
window.SupertabInit({ clientId: 'your-client-id' });
const supertab = window.Supertab;
```

### 2. Using `Supertab` class

```javascript
import Supertab from '@getsupertab/supertab-browser';

const supertab = new Supertab({ clientId: 'your-client-id' });
```

## SDK Functions

### Initialize

```javascript
window.SupertabInit({ clientId: 'your-client-id' });
```

or

```javascript
const supertab = new Supertab({ clientId: 'your-client-id' });
```

Initializes the Supertab SDK. This should be the first function called before using any other SDK features.
- `clientId` (string, required): Your unique client identifier provided by Supertab.

### auth

```javascript
const authentication = await supertab.auth(options);
```

Authenticates the user with Supertab.

#### options

An optional `options` object supports following properties.

<ParamField path="silently" type="boolean">
  Attempt silent authentication if true
</ParamField>
<ParamField path="screenHint" type="string">
  `login` or `register` to specify the desired authentication screen
</ParamField>
<ParamField path="state" type="object">
  Optional state object to pass through the authentication process
</ParamField>
<ParamField path="redirectUri" type="string">
  The URI to redirect to after authentication
</ParamField>

Returns an Authentication object if successful, or undefined if silent auth fails.

### getCurrentUser

```javascript
const user = await supertab.getCurrentUser();
```

Retrieves information about the currently authenticated user.

Returns an object with the user's ID.

### getOfferings

```javascript
const offerings = await supertab.getOfferings(options);
```

Retrieves available offerings for the user.
- `options` (object):
  - `language` (string, optional): Preferred language for offering descriptions.
  - `preferredCurrencyCode` (string, optional): Preferred currency for pricing.

Returns an array of offering objects.

### checkAccess

```javascript
const access = await supertab.checkAccess();
```

Checks the user's current access status.

Returns an object with a `validTo` property indicating when the access expires (or undefined if no valid access).

### getUserTab

```javascript
const tab = await supertab.getUserTab();
```

Retrieves the user's current tab information.

Returns an object with tab details including ID, currency, limit, purchases, status, and total.

### pay

```javascript
await supertab.pay(tabId);
```

Initiates the payment process for a specific tab.
- `tabId` (string): The ID of the tab to be paid.

Returns a Promise that resolves when the payment process is complete.

### purchase

```javascript
const purchase = await supertab.purchase(options);
```

Initiates a purchase for a specific offering.
- `options` (object):
  - `offeringId` (string): The ID of the offering to purchase.
  - `preferredCurrency` (string): The preferred currency for the purchase.

Returns an object with purchase details.

### getApiVersion

```javascript
const version = await supertab.getApiVersion();
```

Retrieves the current version of the Supertab API.

Returns a string representing the API version.

### authStatus

```javascript
const status = supertab.authStatus;
```

A getter that returns the current authentication status.

Returns an AuthStatus enum value.

## Authentication

To authenticate a user:

```javascript
try {
  const authentication = await supertab.auth({
    silently: true, // Attempt silent authentication
    screenHint: 'login', // or 'register'
    state: {}, // Optional state object
    redirectUri: 'https://your-redirect-uri.com'
  });

  if (authentication) {
    console.log('User authenticated');
  } else {
    console.log('Silent authentication failed, user interaction required');
  }
} catch (error) {
  console.error('Authentication error:', error);
}
```

## User Information

To get information about the current user:

```javascript
try {
  const user = await supertab.getCurrentUser();
  console.log('User ID:', user.id);
} catch (error) {
  console.error('Error getting user information:', error);
}
```

## Offerings

To retrieve available offerings:

```javascript
try {
  const offerings = await supertab.getOfferings({ language: 'en-US' });
  console.log('Available offerings:', offerings);
} catch (error) {
  console.error('Error getting offerings:', error);
}
```

## Access Control

To check user access:

```javascript
try {
  const access = await supertab.checkAccess();
  if (access.validTo) {
    console.log('Access valid until:', access.validTo);
  } else {
    console.log('No valid access');
  }
} catch (error) {
  console.error('Error checking access:', error);
}
```

## Tab Management

To get the user's current tab:

```javascript
try {
  const tab = await supertab.getUserTab();
  console.log('User tab:', tab);
} catch (error) {
  console.error('Error getting user tab:', error);
}
```

To pay a tab:

```javascript
try {
  await supertab.pay(tabId);
  console.log('Tab paid successfully');
} catch (error) {
  console.error('Error paying tab:', error);
}
```

## Purchases

To make a purchase:

```javascript
try {
  const purchase = await supertab.purchase({
    offeringId: 'offering-id',
    preferredCurrency: 'USD'
  });
  console.log('Purchase successful:', purchase);
} catch (error) {
  console.error('Error making purchase:', error);
}
```

## Utility Functions

To get the API version:

```javascript
try {
  const version = await supertab.getApiVersion();
  console.log('API Version:', version);
} catch (error) {
  console.error('Error getting API version:', error);
}
```

## Error Handling

The SDK uses promises, so you can use try/catch blocks or .catch() methods to handle errors. Always wrap SDK calls in try/catch blocks to handle potential errors gracefully.

## Example Implementation

Below is a basic example of how to implement the Supertab SDK in a web application:

```html
<html>
  <head>
    <script type="module" src="path/to/supertab-sdk.js"></script>
    <script type="text/javascript">
      let authentication;
      let user;

      window.addEventListener("load", async () => {
        window.SupertabInit({
          clientId: "your-client-id-here",
        });
      });

      async function getCurrentUser() {
        try {
          user = await Supertab.getCurrentUser(authentication);
          console.log('User:', user);
        } catch (e) {
          console.error('Error getting current user:', e);
        }
      }

      async function startAuthFlow(options) {
        try {
          authentication = await Supertab.authorize(options);
          if (authentication) {
            console.log('Authentication successful:', authentication);
          } else {
            console.log("No authentication object returned");
          }
        } catch (e) {
          console.error('Authentication error:', e);
        }
      }

      async function checkAccess() {
        try {
          const access = await Supertab.checkAccess();
          console.log('Access:', access);
        } catch (e) {
          console.error('Error checking access:', e);
        }
      }

      async function getOfferings() {
        try {
          const offerings = await Supertab.getOfferings();
          console.log('Offerings:', offerings);
        } catch (e) {
          console.error('Error getting offerings:', e);
        }
      }

      async function purchase() {
        try {
          const offerings = await Supertab.getOfferings();
          const offeringId = offerings[0].id;
          const preferredCurrency = "USD";
          const purchase = await Supertab.purchase({
            offeringId,
            preferredCurrency,
          });
          console.log('Purchase:', purchase);
        } catch (e) {
          console.error('Error making purchase:', e);
        }
      }
    </script>
  </head>

  <body>
    <h1>Supertab SDK Demo</h1>
    <button onclick="startAuthFlow({ silently: true })">Auth (silently)</button>
    <button onclick="startAuthFlow()">Auth</button>
    <button onclick="getCurrentUser()">Get User</button>
    <button onclick="checkAccess()">Check Access</button>
    <button onclick="getOfferings()">Get Offerings</button>
    <button onclick="purchase()">Purchase</button>
  </body>
</html>
```

This example demonstrates how to initialize the SDK, handle authentication, retrieve user information, check access, get offerings, and make a purchase. It provides a simple UI with buttons to trigger these actions, making it easy to test and understand the SDK's functionality.

## Advanced Usage

### Custom System URLs

You can provide custom system URLs when initializing the Supertab instance:

```javascript
const supertab = new Supertab({
  clientId: 'your-client-id',
  systemUrls: {
    authUrl: 'https://custom-auth-url.com',
    tokenUrl: 'https://custom-token-url.com',
    ssoBaseUrl: 'https://custom-sso-base-url.com',
    tapiBaseUrl: 'https://custom-tapi-base-url.com',
    checkoutBaseUrl: 'https://custom-checkout-base-url.com',
  }
});
```

### Handling Currency Preferences

When calling methods that involve currency, you can specify a preferred currency code:

```javascript
const offerings = await supertab.getOfferings({
  preferredCurrencyCode: 'EUR'
});
```

## API Reference

This documentation provides an overview of the Supertab Browser SDK's main features and usage. For more detailed information about specific methods and their parameters, please consult the API reference.
