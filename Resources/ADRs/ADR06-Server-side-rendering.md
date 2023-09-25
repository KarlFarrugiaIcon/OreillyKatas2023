# ADR 6: Server Side Rendering (SSR) vs Client Side Rendering (CSR)

## Status  
Proposed

## Rationale 
Server side rendering (SSR) and client side rendering (CSR) are two common techniques for web development. SSR means that the web page is rendered on the server and sent to the browser as HTML, while CSR means that the web page is rendered on the browser using JavaScript. Intergrated into the PWA design, SSR will help provide faster initial loading times, as the browser does not have to wait for javascript to execute and render the page. 

## Decision   
After evaluating both approaches, Server-Side Rendering (SSR) is the proposed rendering approach for the Progressive Web App.

## Consequences  
Positive:
* Faster initial loading time;
* Better SEO, as search engines can crawl and index the HTML content more easily;
* Better accessibility, as users with slow or unreliable internet connections or devices with limited capabilities can still view the page.

Negative:
* It may require additional server resources, especially during periods of high traffic. Additional efforts need to be taken to ensure that the infrastructure can handle the increased load.
