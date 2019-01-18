<<<<<<< HEAD
# slack-desktop-dark-theme
My Frankensteined CSS to add a dark theme to the Slack desktop app, based on [slack-black-theme](https://github.com/d-fay/slack-black-theme).

## Installation

Navigate to the Slack desktop installation directory:
* Windows: `%localappdata%\slack\`
* Linux: `/usr/lib/slack/`
* MacOS: `/Applications/Slack.app/Contents/`

Open up the most recent version (e.g. app-3.1.1) then open resources\app.asar.unpacked\src\static\index.js and resources\app.asar.unpacked\src\static\ssb-interop.js

At the very bottom of both files, add the contents of [loader.js](loader.js).
=======
# Slack Black Theme

A darker Slack theme.

# Installing into Slack

Find your Slack's application directory.

* Windows: `%homepath%\AppData\Local\slack\`
* Mac: `/Applications/Slack.app/Contents/`
* Linux: `/usr/lib/slack/` (Debian-based)


Open up the most recent version (e.g. `app-2.5.1`) then open
`resources\app.asar.unpacked\src\static\index.js`

For versions after and including `3.0.0` the same code must be added to the following file
`resources\app.asar.unpacked\src\static\ssb-interop.js`

At the very bottom, add

```js
// First make sure the wrapper app is loaded
document.addEventListener("DOMContentLoaded", function() {

  // Then get its webviews
  let webviews = document.querySelectorAll(".TeamView webview");

  // Fetch our CSS in parallel ahead of time
  //const cssPath = 'https://raw.githubusercontent.com/jpdillingham/slack-desktop-dark-theme/master/custom.css';
  const cssPath = 'https://raw.githubusercontent.com/Possemaster/slack-desktop-dark/master/dark.css';
  let cssPromise = fetch(cssPath).then(response => response.text());

  let experimentalcss = `
  `;

  // Insert a style tag into the wrapper view
  cssPromise.then(css => {
     let s = document.createElement('style');
     s.type = 'text/css';
     s.innerHTML = css + experimentalcss;
     document.head.appendChild(s);
  });

  // Wait for each webview to load
  webviews.forEach(webview => {
     webview.addEventListener('ipc-message', message => {
        if (message.channel == 'didFinishLoading')
           // Finally add the CSS into the webview
           cssPromise.then(css => {
              let script = `
                    let s = document.createElement('style');
                    s.type = 'text/css';
                    s.id = 'slack-custom-css';
                    s.innerHTML = \`${experimentalcss}\`;
                    document.head.appendChild(s);
                    `
              webview.executeJavaScript(script);
           })
     });
  });
});
```
>>>>>>> a19cee36c5d93e9426045c0e0f6120c3b0a07da1
