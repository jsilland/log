---
title: Chrome Dev Summit November 2015
date: 2015-11-18
layout: page
---

## TL;DR

- Progressive Web Apps take advantage of a broad set of new Web APIs, the most important of which is ServiceWorker. The target is mobile devices but evertone benefits from optimizing for them.
- RAIL is a model for building fast websites where performance guidelines are rooted into the user experience.
- HTTPS and HTTP/2 are unavoidable because they provide clear benefits to the user.

## Links and things of note

- [PerfJankie](https://github.com/axemclion/perfjankie) is a tool to monitor smoothness and responsiveness of websites and Cordova/Hybrid apps over time
- [HTTPS testing](https://badssl.com/) - 
- [Report URI](http://report-uri.io), can be used as a target for mixed content and PKP warnings
- [sw-precache](https://github.com/GoogleChrome/sw-precache) is a code generator for SWs dedicated to pre-caching, e.g., assets
- [sw-toolbox](https://github.com/GoogleChrome/sw-toolbox) is a generic toolbox for SWs
- [Web Starter Kit](https://developers.google.com/web/tools/starter-kit/?hl=en)
- [Vannila JS Application Shell](https://github.com/GoogleChrome/application-shell)
- [Eddystones](https://developers.google.com/beacons/?hl=en)
- Audit tab in the Dev Tools – Holy Strava!
- [FLIP animations](http://bit.ly/fli-anims) – guidelines for creating smooth animations on web pages
- [BigRig](https://github.com/GoogleChrome/big-rig) – Frontend and backend for storing and analyzing recorded Chrome traces / timelines over time
- [Polyfill CDN](https://cdn.polyfill.io/v2/docs/) – On demand polyfills
- [d8](http://www.sandeepdatta.com/2011/10/using-v8-javascript-shell-d8.html) – Implements a JS Console on the CLI and more
- [Perf Timing Primer](http://siusin.github.io/perf-timing-primer/) – guidelines for determining how to measure performance
- [List of HTTP/2 implementations](https://github.com/http2/http2-spec/wiki/Implementations)
- One request to CORS-ify the Strava API (!)
- [SVGOMG](https://github.com/jakearchibald/svgomg)

## Darin Fisher – Keynote

- Chrome for Android - shipping sine 2010
- Lot of work into the WebView - both Chrome and the WebView auto-update
- 800M users of Chrome on mobile, probably including iOS?
- The Web platform is meant to be no-friction - no install needed and link-based
- Chrome on mobile: users have 100s of tabs. 80% of the time is spent in 3 (native?) apps.
- The reach is higher for mobile web apps, growth is twice faster for mobile web than it is for native apps
- "The Web is critical to your mobile strategy" - but the Web is not just Chrome
- Working with standard bodies, developers, features that are built to last
- Reliability
  - Can users count on the web working, being good? Don't assume a consistent network connection
  - ServiceWorker API - deployed in Chrome since several releases ago
  - 2.2B page loads a day using SWs, split about even between mobile and desktop
- Performance
  - Performance is what a user perceives it is - responsiveness, smooth animations - RAIL
  - Reaction Time < 100ms
  - Animation Time < 16.67ms
  - Idle Time < 50ms 
  - Load Time < 1s - enough of your application should have loaded
  - 10% reduction in memory ussage, 25% reduction in startup time, 25+% reduction in power on Mac
  - ampproject.org
  - Polymer: 4x faster, 36% smaller
- Engagement
  - Help users gt back to your site
  - 79% growth in launches from home screen icons, May to November 2015
  - Push notifications
  - FB's @Scale : Mobile first cannot mean native only
  - 72% more time spent, +26% in revenue when users engage through push
- Flipkart.com
- 25% increase in forms submissions when autofill enabled

## Tal Oppenheimer – Developing for billions

- 3.2B internet users, 640M in China, 243M in India
- Growth is stronger in India, China, potential is also higher there
- 35% growth in ecommerce in India
- Speed is a challenge, computing power is a challenge, storage is a challenge, eg. Nexus 6P vs Samsung Galaxy J1
- 2G is prevalent
- 1 second delay in page load time: 11% fewer page views, 16% decrease in customer satisfaction
- In India, it takes 17 hours of minimum wage work to get a 500MB plan
- People use their phones differently - lots of tradeoffs
- Turn data on/off daily
- Chrome Data Saver - compresses web pages by 60%, removes images with placeholders
- PageSpeed module OptimizeForBandwidth
- DPR, Width, Viewport-Width: match the resolution of the device
- Chrome network limiter

## Alex Russell – Progressive Web Apps

- Lots of attempts to make Web content work offline
- The difference now is the power of friction
- Service Workers are built on URLs
- 25 native apps per month vs. 100 sites per month (Android)
- Cost per install == $1~2, cost per loyal user is even greater
- Each step will cost you 20% of users: load store, find app, click install, accept permission, download
- Web: Click install, download, accept permissions
- Flibpboard: +75% since giving up app only strategy
- App upsell interstitials - demoted in Google search
- Add to Home Screen vs. Bookmarking – what's the difference?
- How to build and cache application shells?
- Apps should start up and be unique
- Requirements
  - Service Worker
  - TLS
  - 2 visits to scoped documents over 5+ minutes
  - Manifest
- Progressive web apps are live and part of the real open web, ongoing standardization effort
- Great apps for budget phones
- Web Manifests store the metadata for a progressive web app
  - Icon
  - Splash screen
  - Background color
  - Orientation
  - Name / short name
- Considerations:
  - App needs to provide nav since there is no URL bar
  - Share UI is also not built in
  - Deep linking configuration

## Emily Stark – Deploying HTTPS: The Green Lock and Beyond

- Properties of HTTPS
  - Confidentiality
  - No tampering with content
  - Identity assertion
- Chrome will progressively enable powerful features over HTTPS only
- Step 1: buy a certificate – SSLMate / Let's Encrypt
- HTTPS 102 - lots of things can go wrong after that
- Understanding the lock icon
	- Why is unsafe the neutral page icon?
	- Chrome will introduce a negative indicator, making HTTPS the norm
	- Intermediate state, when the connection is not fully secure, e.g. yellow badge (which was recently replaced with the neutral page)
	- Lock icon of doom - expired cert, wrong cert
	- New panel in Dev Tools: Security
	  - Subresources affecting the lock icon
	  - Notices about upcoming deprecations
	  - Available on canary
- Getting to green
  - Mixed content warning
  - Content-Security-Policy header points to a report URI which will be poinged when users get mixed content [Report URI](report-uri.io)
  - Content-Security-Policy: upgrade insecure requests (for subresources)
- Beyond green
  - Mark session cookies as secure
  - Strict Transport Security, mandates HTTPS at the domain level 
  - Certificate chain is susceptible to compromission of any single CA
    - Certificate transparency: log all certificates publicly
    - Public Key Pinning: asserts the presence of a known public key in the chain. Fairly dangerous and heavy-handed, requires experience from the ops side
    - Practice mode for PKP: Report URI for violations

## Jeff Posnick – Instant Loading with Service Workers

- Application shell + Service Worker
- SW vs. real network: 5ms vs 400+ms load time
- Instant loading isn't just nice to have, direct impact ont he bottom line
- What's an App Shell? HTML + CSS + JS that provide the infrastructure for the content
- App Shell + Dynamic content = application
- SW lifecycle:
  - install
  - activate: cache cleanup
  - idle
  - fetch event
- Libraries: `sw-precache` for the shell, `sw-toolbox` for the content
- `sw-precache` generates the SW code for assets and maintains an up-to-date cache
- `sw-toolbox` implements strategies for loading resources through the SW
  - `.cacheFirst` handler – goes to the cache first, then fetch 
  - `.networkFirst` handler – goes to the network first, then cache
  - `.fastest` handler – goes to both in parallel

```
toolbox.router.get(
  '/path/to/images/*.png',
  toolbox.cacheFirst,
  {cache { name: 'images', maxEntries: 6 }}
)
```

- HTTP caching best practices still apply: distant expirations and hashed filenames

## Owen Campbell-Moore – Increase Engagement with Web Push Notifications

- Web Push = Reach + Engagement
- Facebook
  - The number of mobile web users is growing as fast as native apps, especially for emerging markets
  - The Web is at a disadvantage
  - Developed proprietary push notifications integration, which drove engagement, increase in MAUs
- +50% repeat visits within 3 months (Beyond the Rack) after push implementation
- Service Worker also handles push notifications from a 'Push Service' (GCM):
  - SW subscribes through the `pushManager` of the SW registration
  - Push messages do not contain payloads, just a signal
  - SWs have a `push` event for which a handler may be install
  - Upon push: fetch notification content and display notification
- Consider notifications along the following: importance, urgency
- Don't request permissions on page load, provide contexts, e.g. Twitter does it in Settings
- Make permissions controllable.
- Deduplicating with native - no ideal solution here
  - Orient user to the native app in the web settings if installed
  - Deduplicate on the server side (fingerprinting!)
- Next: custom controls / actions 
- Next: payloads in push messages

## Scott Jenson & Vincent Scheib – Engaging with the Real World: Web Bluetooth and Physical Web

- The Web MIDI API shipped - used by Yamaha to bridge the Web and the real-world
- Web Bluetooth enabled today in Dev
- Can be used to control Bluetooth lightbulbs
- Bluetooth generic attributes, for reading from HRMs, battery level
- Bluetooth devices expose services
- Pairing resolves a `Promise`
- `device.connectGATT()`, `getCharacteristic()`, `readValue()`
- Event listeners for when values change
- Polymer Platinum element, `platinum-bluetooth`
- Experimental, behind a flag + BLE peripheral simulator
- WebNFC, WebUSB are both in the works
- The browser viewport is magic
- The URL bar is the new DOS prompt, entirely unfriendly
- The Web needs a discovery service
- BLE is a connection-free, URL broadcast mechanism
- Better, working QR codes
- Physical resources addressable with URLs
- Experimental Physical Web support coming in Chrome today

## Elisabeth Morant – Asking for Permission: respectful, opinionated UI

- Beyond the Rack, +20% CTO, +26% in revenue after implementing push notifications
- Requires the consent of the user
- Opt-in rate for geolocation API = 6%, notifications = 17%
- Certain apps on iOS are very respectful
  - Double dialog, one in app, the other by the system
  - Lets the app ask again later
- Certain APIs are sensitive
- M47 will disable camera or mic for non HTTPS websites
- New powerful features will only be available over HTTPS
- Guidelines
  1. Provide clear value by communicating intent: 'Notify me when…'
  - Ask at the right time – never on load. Do it on user gesture
  - Only ask for what you need, even though `navigator.permissions.requestAll()` allows to bundle permissions together
  - Give users control - `navigator.permissions.revoke()`
  - Handle failure gracefully
- Chrome is working on a way to monitor the abuse of APIs
  - e.g. Facebook only allows Facebook to exceed notification quotas after they have proven to lift engagement
- New permission API will allow to query the state of  permission 

## Taylor Savage – Polymer - State of the Union

- Lightweight wrapper around the Web Components API
- 1 year ago: Polymer 0.8, shifting away from the experiment realm
- May 2015: Polymer 1.0
  - Redesigned data binding
  - Styling system using custom properties
  - Lightweight Shadow DOM shim
  - 3x faster on Chrome, 36% smaller
- Elements: Iron, Paper, GWC, Platinum, Gold
- Web Components are happening: Edge, Firefox, WebKit
- Over 1M web pages use Polymer, 320 projects inside Google, e.g. YouTube gaming, Google Play Music, Patents, I/O
- Polymer developer summit
- Introducing Polymer 1.2
- Ergonomics vs. performance
  - 20% faster, conservative estimate
- Progressive Web Apps are the ultimate expression of the modern Web
- The Web is a first-class application platform
- Polymer can be a very powerful tool, which might be hard to use
- Polylint for validating Polymer code
- Polydev: Chrome plugin that adds itself to the Chrome Dev Tools
- Application structure; routing; i18n?
- Carbon elements
  - As close to the platform as possible - no frameworks
  - App Layout elements

## Rob Dodson – Building Progressive Web Apps with Polymer

- Progressive Web Apps are just web pages that took the right vitamins. They are:
  - Responsive
  - Load fast
  - Offline
  - Installable
  - Engaging
- Script and HTML imports in the head prevent FOUC but block the rendering
- Keep them in the head, add `async` attribute
- Chrome and Opera already support components – conditionally load the polyfills
- Create a skeleton that mimics the look of the app shell, with inline styles
- `platinum-sw-register` to register a Service Worker, uses `sw-toolbox`
- Polymer Starter Kit takes care of manifest, icon management, etc…
- Bypass User Engagement Checks in [`chrome://flags`](chrome://flags) to validate the app is installable
- Make sure that the `start_url` is cached in the SW, including the query parameters
- Because we have SW + manifest, we can implement push
- `paper-toggle-button` + `platinum-push-messaging`

## Alice Boxhall & Laura Palmaro – Accessibility

- Design of products devices services for people with disabilities
- Over 1B people in the world have some form of disability
- Accessibility is built-into the standard – use elements with the right semantics
- Polymer helps, the elements were thoroughly tested / QA'd on the path to the 1.0 launch
- Focusable and actionnable with the keyboard – `tabIndex`
- `iron-a11y-keys` abstract cross-browser differences for key codes
- Test with a screen reader - built-in most OSes
- ARIA let specify role, state, values
- Accessibility pane in the Dev Tools

## Paul Baukaus – DevTools in 2015: Authoring to the max

- People use Dev Tools to build websites — designers in particular
- It makes a lot of sense
- Make Dev Tools a joy to use
- Make authoring fast intuituve and fun
- More power to JS developers
- User Interface to User Hinderface
- Full-time designer working on Dev Tools
- Color palettes directly in the Dev Tools (Material Design colors)
- New device mode to simulate mobile phones, with keyboard
- Enable throttling for specific speeds
- Easier to access remote debugging
- Replay and debug composite animations
- Framework listeners – actually debug JQuery event listeners
- `serviceworker-internals` is going away
- The 'Resources' tab will list the SWs in scope
- Layout mode: WYSIWYG editing for Dev Tools – visual edition of CSS styles

## Paul Irish & Paul Lewis – Introduction to RAIL

- "Fast is good, fast is better", every bit of perf research
- But it's not linear relation between perf and success
- Sometimes it's too slow, sometimes the ROI is too small
- "is 50ms too slow?" - it depends: loading is ok but scrolling isn't
- The user context matters
- "What does the user feel?" - how do we make it user-centric?
- 60fps / 16ms
- Response: 100ms after user input to feel instantaneous
- Animation: 16ms to avoid a janky feeling
- Idle 
- Load: 1 second to maintain the user's flow

## Paul Lewis – RAIL in the real world

- RAIL works but it needs a few tweaks
- Everything is not equally important
- For a content site, the load is super important
- An app should focus on response and animation, load is less important
- Response and Animation can be grouped together in a funny way
- Response:
  - `setTimeout` to show spinner after 100ms
  - discard timer if the response happened
  - `console.time('something-to-track')`
- Animation
  - The target: 8ms, because the 16ms window is shared by everyone
  - Work with the compositor
  - CSS triggers, CSS transforms
  - FLIP
  - Avoid work during scrolls
- Idle
  - `requestIdleCallback`
  - Used for non-essential work, e.g. analytics
- Load
  - 1000ms for the 75th percentile
  - Inlining the styles for the initial view (skeleton)
  - Lazy load (async) the rest
  - You can't control the initial load, a good chunk of time will be spent on DNS / SSL handshake, etc…
  - Repeat loads can be controlled by Service Workers
  - `webpagetest.org` can capture traces/timelines for a given device / network speed
  - Big Rig - for capturing and tracking Chrome traces over time

## Paul Irish – Owning your performance: RAIL

- Turn on screenshot capturing in the Dev Tools (Timeline tab)

## Seth Thompson – V8 Performance from the Driver's Seat

- A new optimizing compiler
  - TurboFan, optimized for ES2015
  - Static type information to optimize `asm.js` (and WebAssembly)
- Smarter garbage collection
  - Idle time GC
  - V8 can shrink the heap by ~40% when the page is not in use
- ES2015, ES6 support
  - Classes, arrow functions, spread/rest operators
  - Coming: default parameters, destructuring assigment
- Svelte interpreter
  - For svelte devices, with little RAM
  - Ignition bytecode is 3~4 smaller than unoptimized code
- Next: adapting to frameworks such as JQuery, Ember, React
- Tip: do not add dynamic properties to objects, declare and initialize them in the constructor
- Use d8, a headless V8 to do inspect from the command line
- `node --trace-deopt` to get a sense for where the compiler is struggling

## Ilya Grigorik – Quantify and improve real-world RAIL

- Variance in performance is very high in the real world
- Consult the [Perf Timing Primer](http://siusin.github.io/perf-timing-primer/)
- New `PerformanceObserver` API in Chrome, observer-based
- Response in < 100ms – some amount of that is taken by the hardware and the Kernel
  - `event.timeStamp` should be that of the Kernel event, `DOMHighResTimestamp`
- Passive event listeners – where `preventDefault()` becomes a noop, still in the works
- Visbility is a common use-case, i.e. understanding what is in the viewport. Developers want to know when an element enters the viewport
- `IntersectionObserver`, also works to detect the viewport approaching, still in the works
- 16ms is a nice number but there's no way to know how much work *can* be scheduled upfront, there are too many unknown factors
- Frame API timing should help, it accounts for most external factors
- Subscribe to slow frames notifications — part of `PerformanceObserver`
- Make your critical path local with Service Worker (app shell)

## Surma – HTTP/2 101

- TLDR: Switch away from HTTP1. HTTP/2 is better in every way
- Performance
- We've been abusing H1 for things it wasn't designed for
- Best practices, workarounds, hacks
- Concatenation, minification, inline, sprites, sharding, vulcanize, gzip
- Flaw #1: Head Of Line blocking means one resource at a time
- Browsers originally only had 2 connections to the same server, adding more connections is stupid
- Flaw #2: metadata - waste of bandwitdh by encoding state in cookies
- SPDY/2, then HTTP/2
- H2 has the same semantics as H1, request + responses
- Every connection starts as H1, then gets upgraded to H2
- TLS encrypted – h2c (clear text) exists but is not implemented
- 1 multiplexed connection instead of n blocking ones
- H2 defines different frames for headers vs. data
- [http://http2.golang.org/gophertiles](http://http2.golang.org/gophertiles) as a toy example – demonstrates we do not need sprites anymore
- Separating data and headers means we can compress headers, using HPACK
- The compressor is maintained at the connection level, meaning it can become better over time. Session cookies become highly-compressible
- Push: the server can return styles and scripts as a response to the document request – the UA will not request them.
- Still use compression, but only on content types where it matters
- Most browsers support H2 today (Opera Mini and Android browser)
- Server-side: Nginx, GCE, Apache, Python, Ruby, Go, Node
- [https://github.com/http2/http2-spec/wiki/Implementations](https://github.com/http2/http2-spec/wiki/Implementations)
- Putting assets on an H2-enabled CDN
- H2 reverse proxy is a viable option

## Peeyush Ranjan – Building and deploying a Progressive Web App at scale with FlipKart

- [Webpack](http://webpack.github.io/docs/what-is-webpack.html)

## Leadership Panel

- Is SW ready?
  - 2.2B page loads a day though SWs, 350M push messages a day
  - Chrome only, FF hopefully will land support early 2016
  - Pages aren't going to break in browsers that do not have SWs
- Chrome Extensions long-term support
  - Part of the platform, not going away
  - Wanted to focus on progressive web apps and the mobile web
- ChromeOS + Android ?
  - The core user experience of ChromeOS is not something that is going away
- Forcing HTTPS is causing developers pain
  - HTTPS is really important
  - No expectation of security in its absence
- When will Google properties start using SWs?
  - No one knows
- Deprecation of SMIL
  - No path to interoperability
  - Want to make SVG work well
- Pointer events
  - Happening right now, behind a flag
- PPK's call to slowdown (moratorium)
  - What is the developer adoption for the new things?
  - Working with other browser vendors
  - Why slowing, to what end? It's about value, exchange / communication
- How are ideas prioritized?
  - Performance is a big issue on the mobile web
  - Emerging markets
  - `blink-dev` is open, talking to developers
  - It's a collaborative process
- Polymer & porgressive web apps 
  - It does - watch the talk from yesterday
  - `async` for components is coming
- Mixed content, downloading from any source for, e.g. a podcast app
  - No good answer
  - Want users to feel safe – tradeoff in terms of reach
- Push notification token bound to a single device
  - Looking into ways to make it happen
- Is V8 actually paying attention to the asm flag
  - Turbofan replacing Crankshaft
  - Fully type-aware
- When you launch a window from a push event, it doesn't open in full-screen
  - Probably a good suggestion
- What about Let's Encrypt?
  - It's an exciting project - can't say more at this time
- ES2015 modules vs. Web Components
  - Web Components = loose combination of specs
  - Not entirely sure how things will fit together
  - Loading mechanism for ES6 not fully spec'd out
- TC39 talking about adding types
  - Turbofan is fully type aware
- Platform feature requests
  - No easy way
  - W3C, but not the easiest group to work with
  - `blink-dev`, just put the idea out
  - Web Platform Incubation Group (?)
  - Platform developers sometime have arguments in a complete vacuum
- URL bar status for when apps are launched from home screen
  - Reasoning about navigation is important for apps that want to be native
  - Same goes for triggering native share intents
- What about Angular?
  - React is more of the new hot thing than Angular
  - Polymer is a little different - it uses the platform
  - DOM is the framework, no big JS payload
  - Polymer is part of the Chrome team