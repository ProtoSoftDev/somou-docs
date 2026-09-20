# Tracking with the Meta Pixel



The [Meta Pixel](https://developers.facebook.com/documentation/meta-pixel) is a snippet of JavaScript code that allows you to track visitor activity on your website. It works by loading a small library of functions that you can use whenever a site visitor takes an action (that is, an event) that you want to track; this is called a conversion.

Embedding the Meta Pixel is a feature that lets you know how many visitors to a given page have clicked on the embedded signup button. This can help you understand how many people considered WhatsApp and how many successfully converted.

Make sure the [initial code setup](https://developers.facebook.com/documentation/meta-pixel/get-started#base-code) triggers a `Pageview` event with your Facebook app ID and the `feature` parameter.

## Example

```js
&lt;!-- Meta Pixel Code --&gt;
&lt;script&gt;
  !function(f,b,e,v,n,t,s)
  &#123;if(f.fbq)return;n=f.fbq=function()&#123;n.callMethod?
  n.callMethod.apply(n,arguments):n.queue.push(arguments)&#125;;
  if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version=&#039;2.0&#039;;
  n.queue=[];t=b.createElement(e);t.async=!0;
  t.src=v;s=b.getElementsByTagName(e)[0];
  s.parentNode.insertBefore(t,s)&#125;(window, document,&#039;script&#039;,
  &#039;https://connect.facebook.net/en_US/fbevents.js&#039;);
  fbq(&#039;init&#039;, &#039;&lt;i&gt;your-pixel-id&lt;/i&gt;&#039;);
  fbq(&#039;track&#039;, &#039;PageView&#039;, &#123;appId: &#039;&lt;i&gt;your-facebook-app-id&lt;/i&gt;&#039;, feature: &#039;whatsapp_embedded_signup&#039;&#125;);
&lt;/script&gt;
&lt;noscript&gt;
  &lt;img height=&quot;1&quot; width=&quot;1&quot; style=&quot;display:none&quot; src=&quot;https://www.facebook.com/tr?id=&lt;i&gt;your-pixel-id&lt;/i&gt;&amp;ev=PageView&amp;noscript=1&quot;/&gt;
&lt;/noscript&gt;
&lt;!-- End Meta Code --&gt;
```
