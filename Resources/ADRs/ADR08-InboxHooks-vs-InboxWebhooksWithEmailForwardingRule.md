# ADR 8: Polling vs Inbox Webhooks with Email Forwarding Rules

## Status  
Proposed

## Rationale
There is the need for the system to be able to know whether there is a new email of interest which has hit the users mailbox. There are two approaches we have identifies. 

1. Polling the users' mailbox every arbitrary amount of time and looking for the latest emails to see if there are any of interest

2. Creating a webhook that notifies our system of any new email that has hit the user's mailbox.

Point 2 was further expanded that a mail forwarding rule was to be set on the user's side to forward all email to a mailbox which is managed by Road Warriors. This allows the technical team to create a webhook trigger with an RPA tool to publish an event on the service bus.

## Decision
We are going to use Inbox Webhooks with Email Forwarding Rules due to their more efficient way of handling the email discovery and less core service intensive way.

## Consequences
Positive:
* It is less intensive on our core services
* The discovery of new emails is much more efficient

Negative:
* The addition of more products to the solution