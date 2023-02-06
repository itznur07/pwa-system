Create a new React project using npx create-react-app my-pwa.

Install the workbox-webpack-plugin to configure the service worker by running npm install --save-dev workbox-webpack-plugin.

Open the webpack.config.js file in the my-pwa/config directory and add the following code:


const WorkboxWebpackPlugin = require('workbox-webpack-plugin');

module.exports = {
  // ... other configuration
  plugins: [
    // ... other plugins
    new WorkboxWebpackPlugin.GenerateSW({
      clientsClaim: true,
      skipWaiting: true
    })
  ],
};



Create a new file src/registerServiceWorker.js and add the following code:

export default function register() {
  if (process.env.NODE_ENV === "production" && "serviceWorker" in navigator) {
    window.addEventListener("load", () => {
      const swUrl = `${process.env.PUBLIC_URL}/service-worker.js`;
      navigator.serviceWorker
        .register(swUrl)
        .then(registration => {
          console.log("Service worker registered: ", registration);
        })
        .catch(error => {
          console.error("Service worker registration failed: ", error);
        });
    });
  }
}


Import the registerServiceWorker in the src/index.js file and call it at the end of the file:


import React from "react";
import ReactDOM from "react-dom";
import App from "./App";
import registerServiceWorker from "./registerServiceWorker";

ReactDOM.render(<App />, document.getElementById("root"));
registerServiceWorker();


Add a basic manifest.json file to the public directory with the following content:

{
  "short_name": "My PWA",
  "name": "My Progressive Web App",
  "start_url": ".",
  "display": "standalone",
  "theme_color": "#000000",
  "background_color": "#ffffff"
}


Link the manifest.json file in the public/index.html file:

<link rel="manifest" href="%PUBLIC_URL%/manifest.json">
