
# ADR 1: Progressive Web APP
One of the first architectural decisions taken concerned choosing the right public-facing component which users will interact with. 

Two distinction options were considered:
* Option 1: Developing a distinct Web and Mobile Application having different web and mobile native components; or
* Option 2: Developing Progressive Web App (PWA) following Web native designs supported by native mobile features and platforms.

This decision has a significant impact on the development workload and general user experience of Road Warrior.

## Rationale
The only significant advantages that Option 1 had to offer, were not very applicable to this business case, mainly because:
* Its key advantage would have been to have a dedicated Mobile Application, which makes it easier to consume mobile-specific resources - but, the required features on the mobile application do not have any complexity or access that cannot be covered with PWA mobile support.
* Although cross-platform options are available, developing native mobile applications would have potentially resulted in different workstreams concerning Apple and Android platforms - in addition to the need to build a distinct website.

With this in mind, due to the relatively complexity-free nature of the features specified in the product (from an end-user perspective), a PWA front-end architecture is clearly the superior option. In addition to meeting the baseline requirements adequately, the PWA will still allow usage of mobile features needed (or forecasted to be needed), such as Location Tracking and Push Notifications. Ultimately, development & maintenance efforts and costs are reduced due to the need to build & support one single component.

## Decision
The Progressive Web App (PWA) option is proposed.

## Consequences
Positive:
* Development overhead is reduced and the same code base is used for all platforms.
* Incurs lower costs as compared to maintaining separate codebases for different platforms, which is often required with native mobile applications.
* Allows for rollouts of new features and bug fixes, as changes can be applied on the web server without requiring users to manually update their application installations.

Negative:
* PWA may not fully leverage platform-specific native features, which has more direct access to native APIs.
* While PWA performance has significantly improved in the case of most contemporary technologies, it may not match the performance of a fully optimized native app in all scenarios.
