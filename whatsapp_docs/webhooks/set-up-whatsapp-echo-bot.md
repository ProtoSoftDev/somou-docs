# Create a test webhook endpoint



If you aren&#039;t ready to create your own webhook endpoint yet, you can deploy a test webhook app on [Render.com](https://www.render.com/) that accepts webhook requests and dumps their contents to Render&#039;s console.

_Only use this app for testing purposes._

## Requirements

- A [Render](https://www.render.com/) account.
- A [GitHub](https://www.github.com/) account.

## Step 1: Create a GitHub repository

Sign into your GitHub account and create a new repo (public or private) with a name of your choice. Within the repo, create an `app.js` file and paste this code into it:

```js
// Import Express.js
const express = require(&#039;express&#039;);

// Create an Express app
const app = express();

// Middleware to parse JSON bodies
app.use(express.json());

// Set port and verify_token
const port = process.env.PORT || 3000;
const verifyToken = process.env.VERIFY_TOKEN;

// Route for GET requests
app.get(&#039;/&#039;, (req, res) =&gt; &#123;
  const &#123; &#039;hub.mode&#039;: mode, &#039;hub.challenge&#039;: challenge, &#039;hub.verify_token&#039;: token &#125; = req.query;

  if (mode === &#039;subscribe&#039; &amp;&amp; token === verifyToken) &#123;
    console.log(&#039;WEBHOOK VERIFIED&#039;);
    res.status(200).send(challenge);
  &#125; else &#123;
    res.status(403).end();
  &#125;
&#125;);

// Route for POST requests
app.post(&#039;/&#039;, (req, res) =&gt; &#123;
  const timestamp = new Date().toISOString().replace(&#039;T&#039;, &#039; &#039;).slice(0, 19);
  console.log(`\n\nWebhook received $&#123;timestamp&#125;\n`);
  console.log(JSON.stringify(req.body, null, 2));
  res.status(200).end();
&#125;);

// Start the server
app.listen(port, () =&gt; &#123;
  console.log(`\nListening on port $&#123;port&#125;\n`);
&#125;);
```

## Step 2: Deploy a Node Express app on Render

Follow Render&#039;s instructions for [deploying a Node Express app](https://render.com/docs/deploy-node-express-app), with these differences:

- Skip step 1.
- Use these settings for step 3:
  - Build command: `npm install express`
  - Start command: `node app.js`
  - In the **Environment Variables** section, add the variable `VERIFY_TOKEN` and set it to a string of your choice (for example, `vibecode`).

When you&#039;re done, click the **Deploy your web service** button. Clicking **Deploy your web service** takes you to the app log where you will see your app being built, which can take a few minutes. You&#039;ll know it&#039;s done when you see &quot;Your service is live&quot; in the log.

Copy your deployed test webhook app URL, which appears at the top of the page under your GitHub repo name. (If you view the URL, you&#039;ll get a 403 error, which is expected).

## Step 3: Add your test webhook app URL to your Meta app

Open a new window/tab, and navigate to the (Meta) [App Dashboard](https://developers.facebook.com/apps) &gt; **WhatsApp** &gt; **Webhooks** &gt; **Configuration** panel.

Paste your test webhook app URL in the **Callback URL** field, and add the `VERIFY_TOKEN` environment variable string you set earlier to the **Verify token** field, then click **Verify and save**.

If verification is successful, the Meta app dashboard should refresh and you should see a list of webhook fields you can subscribe to.

_Subscribe to the **messages** webhook field if you haven&#039;t already._

Also, in Render&#039;s app log, if you see &quot;WEBHOOK VERIFIED&quot;, your test webhook app URL has been successfully verified.

## Step 4: Send a test message

Back in the Meta app dashboard **Configuration** panel, scroll down to the **messages** webhook field, subscribe to the field if you haven&#039;t already, then click the **Test** link.

Clicking the **Test** link sends a test message to your test webhook app. Confirm that it appears in Render app log with &quot;Webhook received&quot; followed by a test JSON payload:

## Troubleshooting

If the test **messages** webhook doesn&#039;t appear in the Render app dashboard log:

- Confirm that you successfully added your test webhook app URL to your Meta app (Step 3).
- Confirm that your app is subscribed to the **messages** webhook field.
- Make sure you are sending a **messages** test webhook; some test webhooks only work when your app is in Live mode, while others only work in Development mode (**messages** test webhooks work in both modes).
