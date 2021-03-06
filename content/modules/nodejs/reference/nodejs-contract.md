---
title: NodeJS Contract
description: Requirements to run the Section NodeJS module.
keywords: reverse proxy, CDN, NodeJS
aliases:
  - /reference/nodejs-contract/

---

# NodeJS Requirements

The following defines the requirements to run the NodeJS module in the Section platform:

## Essential

* HTTP server listening a port defined by `process.env.PORT` for HTTP/1.1 — never using HTTPS or any other port.
* In the event communicating upstream or to another service you must connect to `next-hop:80` as HTTP, never HTTPS.
   * The platform will ensure `next-hop` resolves to the next proxy in the chain or the origin — whichever is next upstream.
   * Specify a `X-Forwarded-Proto: https` HTTP request header to perform HTTPS requests to the origin.
* Proxy must handle logs according to these requirements:  
   * All log files must be written under the `/var/log/nodejs` directory.
* Your config must contain a `.gitignore` file which ignores the `node_modules` directory.
* Your NodeJS config must contain a `package-lock.json`file which is generated by `npm`. 
* Your NodeJS application must contain a npm start script which must start your HTTP server.

## Optional

* Your application should handle run time errors which does not crash the application.

## Things you can rely on

* Module can expect a `X-Forwarded-Proto: https` request header for HTTPS connections from the browser. Assume HTTP otherwise.
* Module can expect a `True-Client-IP` request header containing the IP address of the client connecting to the Section servers. The `X-Forwarded-For` request header will be also be present but may contain multiple IP addresses from other HTTP proxies between the client and the NodeJS module.
