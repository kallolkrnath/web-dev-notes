# Web Performance Optimisation

_**Web Performance Optimization (WPO)** is the process of making your website load faster and work smoothly for users. The goal is to improve the user experience and keep visitors engaged._
#### Why it's important:
- Faster websites keep users happy and reduce bounce rates.
- Search engines (like Google) rank faster sites higher.
#### Common Optimization Techniques:
- Minimize file sizes (e.g., compress images and code).
- Use caching to store data for faster loading.
- Reduce server response time.
- Lazy load images (load them only when needed).
- Optimize JavaScript and CSS (minify and bundle them).

A faster website = better performance, user satisfaction, and SEO!
<hr>

1. **What is Cumulative Layout Shift (CLS)?**

=> In web development, **CLS (Cumulative Layout Shift)** is a metric that measures how much a webpage's content moves around unexpectedly while it's loading. It’s part of Google's Core Web Vitals and helps assess a site's user experience.

For example:

- If text shifts because an image loads late, that adds to the CLS score.
- A low CLS score means fewer layout shifts and a better user experience.
Aim for a CLS score of **0.1 or less** for good performance!

2. **What is Largest Contenful Paint (LCP)?**

=> **LCP (Largest Contentful Paint)** is a metric that measures how long it takes for the largest visible element on a webpage (like a big image or text block) to load and appear on the screen. It shows how quickly users see the main content of a page.

For example:

- If your page has a large banner image or a big headline, LCP measures how fast that loads.

Aim for an LCP score of 2.5 seconds or less for a good user experience!

3. **What is First Input Delay(FID)?**

=> **FID (First Input Delay)** measures how quickly a webpage responds when a user interacts with it for the first time, like clicking a button, a link, or typing in a form.

For example:

- If you click a button, FID measures the time it takes for the site to start processing your action.

A good FID score is 100 milliseconds or less, ensuring the site feels responsive to users.

4. **What is lighthouse?**

=> **Lighthouse** is a free tool from Google that helps analyze and improve the performance, accessibility, and SEO of your website.  

What it does:  
- Runs tests on your website and gives you scores for **Performance**, **Accessibility**, **Best Practices**, and **SEO**.  
- Provides suggestions to fix issues, like speeding up your site or making it more mobile-friendly.  

#### Why use it:  
It’s great for improving user experience, ensuring your site ranks well on search engines, and meeting web standards. You can use it in **Chrome DevTools** or as a standalone tool!

#### Steps to Generate a Lighthouse Report:
- Open the webpage you want to test in **Google Chrome**.
- Right-click on the page and select Inspect
- In DevTools, find the Lighthouse tab at the top. If you don't see it, click the >> icon to locate it.
- Choose what you want to test: Performance, Accessibility, Best Practices, Search Engine Optimization (SEO), or Progressive Web App (PWA).
- Select the device type: Mobile or Desktop.
- Click **Generate report** and wait a few seconds while Lighthouse runs the tests.

5. **What is Responsive view?**

=> **Responsive view** lets you see how a webpage looks and behaves on different screen sizes and devices (like phones, tablets, or desktops). It’s used to check if your website adapts well and stays user-friendly across various devices.

For example:

- You can test how your site looks on a mobile screen versus a desktop screen.
- It helps spot layout issues, like text or buttons that don’t fit on smaller screens.

**What is Meta Dsc?**

=> A **meta description** is a short summary of a webpage's content. It appears below the title in search engine results and helps users understand what the page is about.

For example:
- If your webpage is about "Healthy Recipes," the meta description might say:

"Discover quick and easy healthy recipes for every meal, perfect for busy days!"
#### Why it’s important:
- It can attract more users to click on your link.
- Helps improve your site's SEO and visibility.

*You add a meta description in your HTML like this:*

```html
<meta name="description" content="Your short summary here.">
```
