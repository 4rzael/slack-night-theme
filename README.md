# Slack Night Theme
## Motivation
I could not stand once again the white background of the Slack desktop application.

## Install MacOS
### index.js
Open the file `/Applications/Slack.app/Contents/Resources/app.asar.unpacked/src/static/index.js` and copy/paste the following code at the bottom of the file.

```
// DARK MODE
// First make sure the wrapper app is loaded
document.addEventListener("DOMContentLoaded", function() {

   // Then get its webviews
   let webviews = document.querySelectorAll(".TeamView webview");

   // Fetch our CSS in parallel ahead of time
   const cssPath = 'https://cdn.rawgit.com/widget-/slack-black-theme/master/custom.css';
   let cssPromise = fetch(cssPath).then(response => response.text());

   let customCustomCSS = `
   :root {
      /* Modify these to change your theme colors: */
      --primary: #09F;
      --text: #CCC;
      --background: #080808;
      --background-elevated: #222;
   }
   `

   // Insert a style tag into the wrapper view
   cssPromise.then(css => {
      let s = document.createElement('style');
      s.type = 'text/css';
      s.innerHTML = css + customCustomCSS;
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
                     s.innerHTML = \`${css + customCustomCSS}\`;
                     document.head.appendChild(s);
                     `
               webview.executeJavaScript(script);
            })
      });
   });
});
```

### ssb-interop.js
Open the file `/Applications/Slack.app/Contents/Resources/app.asar.unpacked/src/static/ssb-interop.js` and copy/paste the following code at the bottom of the file.

```
// DARK MODE
// First make sure the wrapper app is loaded
document.addEventListener("DOMContentLoaded", function() {

   // Then get its webviews
   let webviews = document.querySelectorAll(".TeamView webview");

   // Fetch our CSS in parallel ahead of time
   const cssPath = 'https://cdn.rawgit.com/widget-/slack-black-theme/master/custom.css';
   let cssPromise = fetch(cssPath).then(response => response.text());

   let customCustomCSS = `
   :root {
      /* Modify these to change your theme colors: */
      --primary: #09F;
      --text: #CCC;
      --background: #080808;
      --background-elevated: #222;
   }

   .c-message:hover:not(.c-message--highlight):not(.c-message--standalone):not(.c-message--pinned):not(.c-message--ephemeral):not(.c-message--custom_response):not(.c-message--starred):not(.c-message--sli_highlight), .c-message--hover:not(.c-message--highlight):not(.c-message--standalone):not(.c-message--pinned):not(.c-message--ephemeral):not(.c-message--custom_response):not(.c-message--starred):not(.c-message--sli_highlight), .c-message--focus:not(.c-message--highlight):not(.c-message--standalone):not(.c-message--pinned):not(.c-message--ephemeral):not(.c-message--custom_response):not(.c-message--starred):not(.c-message--sli_highlight) {
    background-color: #2c2d30 !important;
   }

.c-message__body
{
color: #fff !important;
}

.c-message__sender_link {
color: #fff !important;


.c-message_list__day_divider__label__pill {
    color: #fff !important;
    background-color: #2c2d30 !important;
}

.p-message_pane .c-message_list:not(.c-virtual_list--scrollbar):before, .p-message_pane .c-message_list.c-virtual_list--scrollbar > .c-scrollbar__hider:before {
background-color: black !important;
border-bottom: 1px solid #09F;
}

.c-message_list__day_divider__line {
border-color: #09F;
}
   `

   // Insert a style tag into the wrapper view
   cssPromise.then(css => {
      let s = document.createElement('style');
      s.type = 'text/css';
      s.innerHTML = css + customCustomCSS;
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
                     s.innerHTML = \`${css + customCustomCSS}\`;
                     document.head.appendChild(s);
                     `
               webview.executeJavaScript(script);
            })
      });
   });
});
```

### Restart
Now restart your application and **Enjoy!**

# References
[Slack Black Theme](https://github.com/widget-/slack-black-theme)
