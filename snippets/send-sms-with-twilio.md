Allows you to send SMS text messages from your Wix Corvid account via Twilio's Programmable SMS.

> You'll need an active Twilio account for this to work.

## Preparing funcionality

- First of all, go to your node_modules folder inside the Corvid editor and install `twilio`. Wix already has the library available, so why not take advantage of it.
- To keep your code organized, create a `utils` folder inside your `Backend` folder to store our helper file.
- Create a new file called `twilio.js` (the name is up to you), I just kept it simple.
- You'll notice that Wix shows an error on line `5` due to the `require` statement. That is a bug in their system, which you can ignore. The `require` statement works without issues.

> twilio.js

```js
// Function used to send text messages via Twilio
export function sendText(phone, body) {
  const accountSid = "ACXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"; // Your Account SID from www.twilio.com/console
  const authToken = "your_auth_token"; // Your Auth Token from www.twilio.com/console
  const client = require("twilio")(accountSid, authToken); // ignore Wix error

  client.messages
    .create({
      body: body, // Dynamic value from incoming parameters
      from: "+12345678901", // Your Twilio number from www.twilio.com/console
      to: phone, // Dynamic value from incoming parameters
    })
    .then((message) => console.log(message.sid));
}
```

_Note: We created the helper funcion `sendText` so that you could send SMS messages from any part of your code, without exposing account credentials. (Of course, in a regular Nodejs environment, you would load those credentials from environment variables anyway, instead of including them in the code.)_

## Sending messages

- To send messages, you will import the helper function wherever you want to use it.
- Once imported in a file _(backend files only)_, you can send a message by calling the `sendText` function.

```js
import { sendText } from "backend/utils/twilio";
```

```js
const textDestination = "+19876543210"; // Phone number you wish to send to
const message = "Thank you for joining us!"; // Message text you want to send

// Send text message using Twilio helper function
sendText(textDestination, message)
  .then((response) => {
    console.log("Sent text: " + message);
  })
  .catch((error) => {
    console.log("Error sending text: ", error);
  });
```
