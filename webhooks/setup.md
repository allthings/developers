# Setup & configure webhooks

## Contents

1. [Permission requirements](#permission-requirements)
1. [Adding a webhook to an App](#other-configuration-options)
1. [Other configuration options](#other-configuration-options)
   1. [Registering a webhook across multiple Apps](#registering-a-webhook-across-multiple-apps)
   1. [Receiving events only on a sub-segment of a resource](#receiving-events-only-on-a-sub-segment-of-a-resource)


## Permission requirements

Users and developers will need the `edit-app` permission in order to manage an App's webhooks.

The `edit-app` permission is granted to users by the `App Admin` role. See our user documentation for more details on how to assign user rights [here](https://support.allthings.me/hc/en-us/articles/360030404511-How-can-you-assign-rights-).

The `edit-app` permission can also be granted to a developer's OAuth Client. To request this permission, please [contact our support](https://support.allthings.me/hc/en-us/requests/new) and we'll get it set up for you.


## Adding a webhook to an App using the Cockpit

App administrators can add and manage webhooks on their Apps from within the Cockpit's [App Settings section](https://cockpit.allthings.me/apps).




## Adding a webhook programatically



## Other configuration options

### Registering a webhook across multiple Apps

It is possible for developers to subscribe to events across multiple Apps. This may be useful when your integration is offered to multiple Allthings customers or across multiple Apps.

We don't currently have a UI which exposes this functionality. If you are interested in registering a single webhook for events across more than one App, please [contact our support](https://support.allthings.me/hc/en-us/requests/new) and we'll get it set up for you.


### Receiving events only on a sub-segment of a resource

It is possible to receive events for only a sub-segment of some resources. For example, it is possible to configure a webhook to receive only those events occurring on tickets within a selection of ticket categories.

We don't currently have a UI which exposes this functionality. If you are interested in receiving only a sub-segment of events, please [contact our support](https://support.allthings.me/hc/en-us/requests/new) and we'll get it set up for you.

