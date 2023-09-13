
# ADR 1: Progressive Web APP
We are tasked with selecting a user interface framework for our project. The two options under consideration are Xamarin and Progressive Web App (PWA). The chosen framework will significantly impact the development and user experience of Road Warrior.

## Rationale 
PWA and Xamarin are both cross-platform solutions, but for our use case, PWA has been more beneficial. It has reduced the development effort and cost, while still allowing us to access the native features of web and mobile platforms. PWA also offers better performance, security, and user experience than Xamarin.


## Decision
We will use PWA over Xamarin to implement our User Interface.


## Consequences
While choosing PWA offers numerous advantages, it's essential to consider potential trade-offs:

Positive:
* It reduces the development overhead and we will be able to use the same code base for all platforms
* It generally incurs lower costs as compared to maintaining separate codebases for different platforms, which is often required with Xamarin.
* It allows for quicker iteration and deployment of new features and bug fixes, as changes can be applied on the web server without requiring users to update their app

Negative:
* PWA may not fully leverage platform-specific native features compared to Xamarin, which has more direct access to native APIs.
* While PWA performance has significantly improved, it may not match the performance of a fully optimized native app in all scenarios.
