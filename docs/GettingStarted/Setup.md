# Setup Environment

The development environment is easy as your original Node.js projects. Let's move on.

## Node.js
Node.js v8.0+ is the required runtime to install. It is the basis to support to develop/run your mapping software we are going to build. Recommend to install the latest LTS to better stability purpose. Visit the [official website](https://nodejs.org/) for detail.

## Yarn (Optional)
This is only my personal favorite. Even though, Node.js has its build-in package management tool (NPM), but using `yarn` will be faster, securer and more reliable. Our later demos are all working with `yarn`. If you don't like it, `npm` is also good enough. Refer [this guide](https://classic.yarnpkg.com/en/docs/install/) for detail.

## Ginkgoch Map Library
The Ginkgoch Map Library is hosted in [npmjs](https://npmjs.com) names `ginkgoch-map`. Install with the command below.

```bash
yarn add ginkgoch-map
```

This is almost everything for setup. But for the multiple platforms, we have **slightly more** components to install.

### Desktop App Specific
To build a desktop mapping software, we need another framework for creating native applications with web technologies using JavaScript. `Electron`, `NW.js`, `Carlo` are recently popular frameworks to do such things. In our later demos, we will use `Electron` to introduce how to build a mapping software for desktop.

### Web Service and Web App Specific
Web service and web app is a little different from desktop app development. The key is that, Node.js is lack of graphics API support. Thus we need to install another 3rd party graphic module for rendering graphics on the server side. [canvas](https://www.npmjs.com/package/canvas) is my chosen one in current version. Refer to the [official document](https://github.com/Automattic/node-canvas) to install on a specific operation system.

Here is a quick reference for the dependencies.

| OS      | Command                                                      |
| ------- | ------------------------------------------------------------ |
| macOS   | brew install pkg-config cairo libpng jpeg giflib             |
| Ubuntu  | sudo apt-get install libcairo2-dev libjpeg-dev libpango1.0-dev libgif-dev build-essential g++ |
| Fedora  | sudo yum install cairo cairo-devel cairomm-devel libjpeg-turbo-devel pango pango-devel pangomm pangomm-devel giflib-devel |
| Solaris | pkgin install cairo pkg-config xproto renderproto kbproto xextproto |
| Windows | [refer the wiki for instruction](https://github.com/Automattic/node-canvas/wiki/Installation---Windows) |

> Why Desktop App doesn't Require `canvas` module? Good question! Desktop is running on `electron`, which combines the Chromium's rendering library with Node.js. We don't need another graphic module to rendering anymore.

### Command Line Tool Specific
For normal mapping related command line tool, Ginkgoch Map Library is enough to build functionalities for reading/querying features and doing spatial analysis. For special requirement that requires rendering, a 3rd party graphic module like `canvas` is also required.

This is pretty much everything for setup. Thanks for reading. 
