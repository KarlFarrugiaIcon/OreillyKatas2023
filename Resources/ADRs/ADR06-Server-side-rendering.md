
# ADR 6: Server Side Rendering (SSR)

## Status  
Proposed

## Rationale 
Server side rendering (SSR) and client side rendering (CSR) are two common techniques for web development. SSR means that the web page is rendered on the server and sent to the browser as HTML, while CSR means that the web page is rendered on the browser using JavaScript. We are embarking on the development of a Progressive Web App (PWA) for our application so it will help us faster initial loading time, as the browser does not have to wait for javascript to execute and render the page. 

## Decision   
After evaluating both approaches, we have decided to adopt Server-Side Rendering (SSR) for our Progressive Web App.

## Consequences  
Positive:
* Faster initial loading time.
* Better SEO, as search engines can crawl and index the HTML content more easily.
* Better accessibility, as users with slow or unreliable internet connections or devices with limited capabilities can still view the page.

Negative:
* It may require additional server resources, especially during periods of high traffic. We need to ensure that our infrastructure can handle the increased load

