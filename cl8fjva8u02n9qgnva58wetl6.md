## Fix Mixed Content Errors For HTTP API Calls

You're probably here because you deployed an app on Netlify that fetched an API with HTTP and not HTTPS protocol, and things don't work now. This article will help solve that for you.

> I'm using Netlify here as I'm making use of a static routing feature that they offer. The principles of the fix should work for other platforms that offer some routing mechanism.

## The Scenario

I'm using [NumbersAPI](http://numbersapi.com/#42) for this demo, but any API served over HTTP would work. 

Head over to [this GitHub repo](https://github.com/abinjohn123/netlify-http-api) and download the index.html file. Open it, click on the different facts buttons, and view math facts in all their glory.

The app uses the `fetch()` method to make a GET request to  `http://numbersapi.com/random/type-of-fact`, where `type-of-fact` will depend on the button clicked.

All good so far.

Next, head over to [this link](https://macts-fath.netlify.app/), where I've hosted the exact same project on Netlify. Click on any of the buttons. 

Things don't work now, do they?

You'll get a `TypeError: Failed to fetch` where a beautiful math fact should be. Inspect the dev tools, and you'll find something peculiar.

```
Mixed Content: The page at 'https://macts-fath.netlify.app/' was loaded over HTTPS, but requested an insecure resource 'http://numbersapi.com/random/<type-of-fact>'. This request has been blocked; the content must be served over HTTPS.
```

## What is happening?

According to [this article](https://web.dev/what-is-mixed-content/), *Mixed content occurs when initial HTML is loaded over a secure HTTPS connection, but other resources (such as images, videos, stylesheets, scripts) are loaded over an insecure HTTP connection. This is called mixed content because both HTTP and HTTPS content are being loaded to display the same page, and the initial request was secure over HTTPS.*

Since loading content via an insecure channel is terrible for security, browsers today actively try to block such requests. 

When we load the page locally on our machine, it is loaded in localhost, which is HTTP. Therefore, no mixed content errors are thrown.

## The Solution

The issue is not with our code, but something that the browser throws as a security measure. If we make it seem to the browser that we're trying to access a secure, HTTPS resource, we would be good to go. We make use of one of Netlify's static routing features called Proxy for this.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1663925607608/th7Kbg0s1.png align="center")

The implementation is quite direct. Here are the steps.
1. Add a plain-text file called `_redirects` to the publish directory of your site. The publish directory is where the deployed HTML files are contained. In our demo case, since `index.html` is at the root of the directory, `_redirects` will also be added there.
  
2. Add the following line to the file after making necessary changes
  ```txt
  /api/* https://api.example.com/:splat 200!
  ```
  Replace `https://api.example.com/` with your primary API endpoint. Adding query parameters will be discussed in the next step.
  In our demo case, the content would be 
  ```txt
  /api/* http://numbersapi.com/random/:splat 200!
  ```

3. In your script file, first replace the primary endpoint in the API call statement with `<your-netlify-app-URL>/api`
  Here's how the API call in our demo code would be modified
  
  ![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664001816002/hvLiv-sxU.png align="center")
  
  We're only modifying the primary endpoint. The segment in the URL that contains the query parameters remains the same.

  Here's [the branch](https://github.com/abinjohn123/netlify-http-api/tree/netlify-fix) with the modifications.

That's it! Deploy your site and see how it works now. The updated demo app can be [found here.](https://macts-fath-fixed.netlify.app/) I've deployed the corrected branch to another site since I had to retain the version with the error for the demo.

## So, what happened?

In the `_redirects` file, the line consists of two strings separated by a space. The first string `/api/*` is the original path that we'll navigate to, and `http://numbersapi.com/random/:splat 200!` is the redirected path.

From the official documentation: An asterisk indicates a splat that will match anything that follows it. 

i.e. anything that follows the * in the original path replaces the splat in the redirect URL. A path that has /api/ for the app will now be redirected to the redirect path.

Once we change the API endpoint in the script, from the browser perspective, the call is being made to a Netlify path, which has HTTPS. So no more mixed content errors. Netlify then routes the request to the correct API endpoint behind the scenes.

## Conclusion

Although this workaround works for pretty much anything that can be proxied or redirected, it is recommended to use secure resources in the interest of cybersecurity, as using insecure calls and endpoints can expose your app to a multitude of threats. [This article](https://web.dev/how-to-use-local-https/) explains how to mimic HTTPS behavior in localhost so that such mixed content errors can be identified right in the development stage.

## Resources

* [My GitHub Repo for the demo project](https://github.com/abinjohn123/netlify-http-api)
* [What is Mixed Content](https://web.dev/what-is-mixed-content/)
* [Official Netlify documentation](https://docs.netlify.com/routing/redirects/)
* [A GitHub post that first discussed this solution](https://github.com/netlify/cli/issues/158#issuecomment-540140129)
* [Set up localhost as HTTPS](https://web.dev/how-to-use-local-https/)