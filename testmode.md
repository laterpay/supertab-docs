---
title: 'Test Mode'
description: 'Learn how to use Test Mode'
---



## Overview
Test Mode allows developers to test Experiences without processing real transactions. Using Stripe test cards, developers can simulate payment scenarios and toggle between Test and Live modes through the Experience Editor. This guide explains how to create Experiences, switch between modes, and test transactions.

## Key Features of Test Mode
- Test mode replicates Live Mode behavior, except no real transactions occur. This allows comprehensive testing of all APIs and integration flows.
- Each Site generates a Test Client ID and a Live Client ID. The Experience Editor automatically uses the correct ID based on the selected mode.
- The Installation tab in the Experience Editor allows to toggle between modes and provides ready-to-use code snippets, eliminating the need for manual ID swapping. 
- KYC is deferred until merchants are ready to go live. Merchants can create Sites, Offerings, and Experiences without completing KYC, but KYC must be completed before enabling Live Mode and starting to accept payments.

## Creating and Editing Experiences
- To create or edit an Experience, go to the Revenue Generator section of the Business Portal. Here, you'll find a list of existing Experiences and the option to add a new Experience.
- When adding a new Experience or editing an existing one, you will be taken to the Experience Editor.
- In the Installation tab of the Experience Editor, there is an option to toggle between Test Mode and Live Mode.
    - Switching modes automatically updates the installation code provided, so developers can simply copy the correct snippet without needing to manually change client IDs.
    - The code must be installed on all pages where the Experience is intended to be displayed.


## Using Stripe Test Cards
Use Stripe test cards to simulate various scenarios. Common test cards include:
- Successful Payment: `4242 4242 4242 4242` (Visa)
- Card Declined: `4000 0000 0000 0002`
- Insufficient Funds: `4000 0000 0000 9995`
- Incorrect CVC: `4000 0000 0000 0127`
- Expired Card: `4000 0000 0000 0069`
- 3D Secure Required: `4000 0025 0000 3155`

For more, refer to [Stripe's Testing Guide](https://docs.stripe.com/testing).

## Steps to Simulate a Test Transaction:
1. Install the Test Mode code snippet on all relevant pages.
2. Visit the page displaying the Test Mode Experience.
3. Keep adding Offerings to your Tab until you fill it up.
4.Proceed to payment, and enter a Stripe test card.
5. Complete the form and confirm the transaction. The system will simulate the payment and return the result.

## Post-Transaction Testing and Monitoring
- Use direct API calls to see test tabs, purchases, and payments.
- To see test transactions as an end user, log in to [my.supertab.co](https://my.supertab.co/) with appropriate login credentials and add `?testmode=true` at the end of the URL.


## Moving to Production
1. Once testing is complete, follow these steps to move your Experiences and integrations to production:
2. Switch to Live Mode in the Experience Editor and complete KYC if required.
3. Install the Live Mode code snippet on relevant pages.
4. Use real cards for live transactions.
5. Monitor live transactions in the Dashboard to ensure they process correctly.

