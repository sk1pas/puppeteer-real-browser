> This project is a fork of the original work by @zfcsoftware,
licensed under the MIT License.

<h3 align="center">Puppeteer Real Browser</h3>

## Installation

If you are using a Linux operating system, xvfb must be installed for the library to work correctly.

```bash
npm i puppeteer-real-browser
```

if you are using linux:

```bash
sudo apt-get install xvfb
```

## Include

### CommonJS

```js
const { connect } = require("puppeteer-real-browser");

const start = async () => {
  const { page, browser } = await connect();
};
```

### Module

```js
import { connect } from "puppeteer-real-browser";

const { page, browser } = await connect();
```

## Usage

```js
const { connect } = require("puppeteer-real-browser");

async function test() {
  const { browser, page } = await connect({
    headless: false,

    args: [],

    customConfig: {},

    turnstile: true,

    connectOption: {},

    disableXvfb: false,
    ignoreAllFlags: false,
    // proxy:{
    //     host:'<proxy-host>',
    //     port:'<proxy-port>',
    //     username:'<proxy-username>',
    //     password:'<proxy-password>'
    // }
  });
  await page.goto("<url>");
}

test();
```

**headless**: The default value is false. Values such as “new”, true, “shell” can also be sent, but it works most stable when false is used.

**args:** If there is an additional flag you want to add when starting Chromium, you can send it with this string.
Supported flags: https://github.com/GoogleChrome/chrome-launcher/blob/main/docs/chrome-flags-for-tools.md

**customConfig:** https://github.com/GoogleChrome/chrome-launcher The browser is initialized with this library. What you send with this object is added as a direct initialization argument. You should use the initialization values in this repo. You should set the userDataDir option here and if you want to specify a custom chrome path, you should set it with the chromePath value.

**turnstile:** Cloudflare Turnstile automatically clicks on Captchas if set to true

**connectOption:** The variables you send when connecting to chromium created with puppeteer.connect are added

**disableXvfb:** In Linux, when headless is false, a virtual screen is created and the browser is run there. You can set this value to true if you want to see the browser.

**ignoreAllFlags** If true, all initialization arguments are overridden. This includes the let's get started page that appears on the first load.

## How to Install Puppeteer-extra Plugins?

Some plugins, such as puppeteer-extra-plugin-anonymize-ua, may cause you to be detected. You can use the plugin installation test in the library's test file to see if it will cause you to be detected.

The following is an example of installing a plugin. You can install other plugins in the same way as this example.

```bash
npm i puppeteer-extra-plugin-click-and-wait
```

```js
const test = require("node:test");
const assert = require("node:assert");
const { connect } = require("puppeteer-real-browser");

test("Puppeteer Extra Plugin", async () => {
  const { page, browser } = await connect({
    args: ["--start-maximized"],
    turnstile: true,
    headless: false,
    // disableXvfb: true,
    customConfig: {},
    connectOption: {
      defaultViewport: null,
    },
    plugins: [require("puppeteer-extra-plugin-click-and-wait")()],
  });
  await page.goto("https://google.com", { waitUntil: "domcontentloaded" });
  await page.clickAndWaitForNavigation("body");
  await browser.close();
});
```

## Docker

You can use the Dockerfile file in the main directory to use this library with docker. It has been tested with docker on Ubuntu server operating systems.

To run a test, you can follow these steps

```bash
git clone https://github.com/sk1pas/puppeteer-real-browser
```

```bash
cd puppeteer-real-browser
```

```bash
docker build -t puppeteer-real-browser-project .
```

```bash
docker run puppeteer-real-browser-project
```

## License

Distributed under the MIT License. See [LICENSE](https://github.com/sk1pas/puppeteer-real-browser/blob/main/LICENSE.md) for more information.

## Disclaimer of Liability

No responsibility is accepted for the use of this software. This software is intended for educational and informational purposes only. Users should use this software at their own risk. The developer cannot be held liable for any damages that may result from the use of this software.

This software is not intended to bypass Cloudflare Captcha or any other security measure. It must not be used for malicious purposes. Malicious use may result in legal consequences.

This software is not officially endorsed or guaranteed. Users can visit the GitHub page to report bugs or contribute to the software, but they are not entitled to make any claims or request service fixes.

By using this software, you agree to this disclaimer.
