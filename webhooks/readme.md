# Receive event notifications with webhooks

Listen for events occurring within Apps and automatically trigger reactions.

## Contents

1.  [Introduction](#introduction)
1.  [Common use cases](#common-use-cases)
    1. [Ticketing](#ticketing)
    1. [Booking](#booking)
    1. [Registration Codes](#registration-codes)
    1. [Updating ERP systems](#updating-erp-systems)


 ## Introduction

Allthings uses webhooks to notify you when an event happens in an App. Webhooks are useful when you want to react to certain events occurring within an App like synchronizing ticketing data between Allthings and a 3rd-party ticketing tool or keeping ERP systems up to date with changes to tenant data. Webhooks are particularly useful for extending the Allthings platform with 3rd-party integrations and partners.


### What are webhooks

_Webhooks_ refers to a combination of technologies tied together so that collectively they create a notification and reaction system. Webhooks allow App administrators and developers to augment or enhance the Allthings Platform with additional custom behaviors and functionality.

You can think of a webhook a little bit like a phone call. You share with us your phone number, and whenever there is activity in your App, we call your phone number to share that information with you.


### Common use cases

#### Ticketing

Ticket events are a common webhook use case. Ticket events can be used to synchronize tickets between Allthings and another ticketing or ERP system. More common is for ticket events to be used by partners who offer services or platforms which revolve around service delivery. For example, an integration might seek to automate the damage-case and crafts-person work order creation automatically from Allthings tickets.

![Ticket use case flow chart](https://raw.githubusercontent.com/allthings/developers/master/webhooks/assets/webhooks.introduction.common-use-cases.ticketing.1.svg)


#### Booking

Webhooks are a useful way to integrate Bookings with partner services like door access systems. A tenant can make a booking for an asset like a guest room or common room. When the booking is paid for and/or has been accepted by an Agent, a webhook can deliver the booking to the access-control system used to restrict or control access to the asset. The access-control system can then update the booking with a PIN or access code which is valid only during the period of the booking. Allthings will then deliver this access code to the tenant who made the original booking.

![Booking use case flow chart](https://raw.githubusercontent.com/allthings/developers/master/webhooks/assets/webhooks.introduction.common-use-cases.booking.1.svg)


#### Registration Codes

You can use webhooks to export newly created registration codes. Perhaps you send out registration codes by physical mail, but only want to send new ones. A simple way to implement this is by using Zapier's [Incoming Webhook integration](https://zapier.com/apps/webhook/integrations) feature to automatically have new registration codes [exported to a Google Spreadsheet](https://zapier.com/app/editor/template/1035?referrer=%2Fapps%2Fwebhook%2Fintegrations%2Fgoogle-sheets).


#### Updating ERP systems

Webhooks are a convenient way to keep your ERP system up to date with changes to tenant profile data. For example, when a tenant changes their data like phone number or email address, your ERP system can be automatically updated. Please note that, given the privacy implications of such data synchronization, such integrations must be validated by Allthings before being enabled on production data.



<br /><br /><br />
➡️ **Next Step:** [Set up and configure a webhook](./setup.md)
