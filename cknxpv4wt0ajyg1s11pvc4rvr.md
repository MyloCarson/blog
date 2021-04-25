## Don't have CSR and SSR mixed up!

Server-side rendering vs Client-side rendering. 

As a self-taught developer who has worked with Javascript frameworks such as Angular, Nextjs, and Nuxt, I've struggled to decide whether to use SSR or CSR because they both have advantages and disadvantages.

In the early days of web development, SSR was the de-facto method of creating applications. Simply put, you type a URL into your browser, the browser sends the request to the server, and the server returns an HTML page that has been compiled and is ready to open. An SSR application becomes interactive after the browser has been painted.

> A simple arrow Diagram

Browser → Server → Server returns HTML → Page is Viewable and Interactable


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1616348152587/rFnqfABGE.png)
###### (Disclaimer:  this picture is a property of [https://www.growth-rocket.com/])


However, the times have changed. We can't keep going back and forth to the server because web apps are more complicated. We can do this now that Vue, React, and other JS frameworks have emerged, by creating SPAs with CSR. In CSR, the browser sends a request to the given URL, the server responds with an empty HTML document with links to JS files, the browser downloads these JS files, builds the virtual DOM, and then adds events to make the page interactive. This is why CRS's initial page load time is so long (it also affects the Time to First Meaningful Paint).

> A simple arrow Diagram

Browser → Server → Server returns HTML → Browser fetches JS → Browser executes Vuejs → Page is Viewable and Interactable

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1616348130513/b8kSxfMXx.png)

###### (Disclaimer:  this picture is a property of [https://www.growth-rocket.com/])

## Differences between SSR and CSR

* SEO - Search Engine Optimization 

Web crawlers also known as bots, use the website metadata to rate it in search engines.

SSR pages are pre-rendered on the server and the client receives a webpage as a result. SSR is the best option for SEO. This is done without the need for any additional server plugins or libraries.

Since CSR websites and web content are dynamically rendered, metadata can be applied using JS. The majority of web crawlers don't recognize or accept JS.

* Web performance

With high-end computers (high processing capacity and high-speed networking) on the server, SSR also outperforms CSR because the majority of work and processes are completed on the server, providing the client with just a page ready to be painted in the browser.

CSR depends on the client’s machine, which could have a limited amount of computing power, or networking speed, thus more time is required to carry out an action

* Communication with Server

SSR goes back and forth to the server for any requests. Thus more internet bandwidths are consumed

CSR, however only goes to the server for the initial page request. And the subsequent request is only API calls, not page calls. Except if lazy loading modules/ components are implemented, then network calls will be made to the server to request for such components

They all have one thing in common: for resources like scripts, fonts and photos, the web browsers and server cache them first and then return them when appropriate, avoiding network calls.

* Load time in page transition

This basically discusses the time it takes to transition from Page A to Page B.

Since SSR must always make a request to the server, load time is influenced by network speed. However, this loop will certainly repeat itself with every call to the SSR page.

When comparing CSR to SSR, CSR would take less time to load. It just has to get data from the server if all of the initial scripts have been stored before the first page load.

A long load time is bad for the user experience.

* First Load time

At first load, SSR is faster than CSR since the server just returns a ready page.

Until compiling and rendering a CSR application, the browser must first retrieve all scripts, stylesheets, and files.

* Loading state

SSR does not require a loading state so the server can either have data or not.

To accommodate blank screens, CSR employs loading states. A blank screen occurs when there is a period of time between the network request and the response.

### Use Cases

##### CSR 

* Web applications

* SEO isn’t a priority

* Application is has a lot of interactions

* Dashboard applications where heavy data are requested/needed

##### SSR 

* SEO is a priority

* The first impression is important thus fast first load time

* Interactions are not needed

* Social media sharing

* E-commerce, Blog applications etc

#### Ways to make CSR faster

* Code-splitting

* Lazy loading components, images

* Reducing media sizes or use CDNs for media

##### References:

* https://medium.com/walmartglobaltech/the-benefits-of-server-side-rendering-over-client-side-rendering-5d07ff2cefe8

