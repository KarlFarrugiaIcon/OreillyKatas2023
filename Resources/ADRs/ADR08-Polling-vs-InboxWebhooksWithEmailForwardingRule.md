# ADR 8: Polling vs Inbox Webhooks with Email Forwarding Rules

## Status  
Proposed

## Rationale
There is the need for the system to be able to know whether there is a new email of interest which has hit the users mailbox. Two approaches have been identified:

1. Polling the users' mailbox every arbitrary amount of time and looking for the latest emails to see if there are any of interest.

2. Creating a webhook that notifies the system of any new email that has hit the user's mailbox.

Option 2 was further expanded that a mail forwarding rule was to be set on the user's side to forward all email to a mailbox which is managed by Road Warriors. This allows the technical team to create a webhook trigger with an RPA tool to publish an event on the service bus.

## Decision
Inbox Webhooks combined with Email Forwarding Rules are being proposed as the suggested approach due to their more efficient way of handling the email discovery, all the meanwhile delegating processing resources from the microservices.

## Consequences
Positive:
* Significantly less intensive on core services;
* The discovery of new emails is much more efficient.

Negative:
* The addition of more products to the solution.
