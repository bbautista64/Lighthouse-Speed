# TOC

1. **[Summary](#Summary)**
2. **[64labs.com perf](#64LABSCOM)**
    1. [Summary, recommendations & results](#64LABSCOM-PERF-SUMMARY)
    2. [Notes, errata & resources](#64labscom-perf-notes-errata--resources)
3. **[Lighthouse 6](#LIGHTHOUSE-6)**
    1. [Summary](#Lighthouse-6-Summary)
    2. [Notes, errata & resources](#Lighthouse-6-notes-errata--resources)


# Summary

Two separate but intertwined issues at play here:

1. [64labs.com perf](#64LABSCOM)
2. [Lighthouse 6 & accompanying changes](#LIGHTHOUSE-6)
 
    
# 64LABS.COM

### 64LABS.COM Perf Summary
- **Good news / bad news is that this is affecting many (all?) Gatsby sites**, including Gatsby's own; store.gatsbyjs.org went from a 99 to a 54. Citations for following claims available in the [notes, errata & resources subsection](#64labscom-perf-notes-errata--resources)
    - As Eric observed, LCP is the biggest culprit here, with TBT also contributing
    - There is a closed but active GitHub issues thread with dozens of accounts mirroring our own
    - Google is aware of and looking into the issue
    - Gatsby is aware of the issue and says it will be optimizing further
    - This appears to be a Google-specific issue; LCP for 64labs.com on other performance-measuring services like webpagetest.org reports in the 1s to 3s range
    - To give you an idea of how bonkers the issue is, people are reporting LCP scores tanking on text-only pages; one dev reported a 20-point jump in score from moving a single paragraph

#### Recommendations
- Despite the above, some devs have made recommendations about ways to improve LCP/TBT scores in the meantime
    - "Make sure when using fonts & images, they aren't loaded lazily and are loaded as soon as possible. These assets should be loaded from the same origin as your site, they should not be loaded from a CDN."
    - "If you want to drop TBT, you should checkout preact. Preact has the same API as React but has a smaller javascript footprint."
    - "We had some success just today by using the loading prop for Gatsby image on images that are above the fold (Hero images for us). We've been trying to work on Largest Contentful Paint and this has yielded good results in some initial tests. There is an important part I almost missed to this: If you set loading="eager" for your topmost image, you might want to set fadeIn={false} as well for that image because the transition between the base64 and fully loaded image takes time which delays LCP."
    - "I tried setting loading="eager" and fadeIn={false} on my gatsby image components that were above the fold. Next, I got rid of base64 from my queries. These made a huge difference. Adding _noBase64 to my image fragments brought my score up 20 points!"
    - "My site dropped from ~100 to ~70 with Lighthouse v6."
            - "In order to bring it back to 100 on desktop, I had to
                - remove a background image that was not needed (as it was wrongly chosen as the LCP)
                - optimize size of images
                - set gatsby-image to loading="eager" and fadeIn=false (that's really a bummer as I like the blur-up effect)"

#### Results
- Can attempt some of the above and report results here


### 64LABS.COM Perf notes, errata & resources

- **Link to GH issue on Gatsby perf under Lighthouse 6, and selected quotes from thread:** https://github.com/gatsbyjs/gatsby/issues/24332
    - "Is LQIP (Low-Quality Image Placeholders) on gatsby-image helping or harming when it comes to LCP? LCP of store.gatsby.org on the run above was 4.7s!"
    - "To me I'm having big difficulties in understanding Largest Contentful Paint, in the sense that I'm getting very large values without knowing why, and seeing a discrepancy between the value in the report (for example 7.4s, and the LCP label that appears in the Performance - ViewTrace tab (~800 ms)"
    - "I also have another site where the is screen i just text and it’s blaming the hero text as the main cause for LCP. And LCP only accounts for whatever is in view, which doesn’t make sense. How can be a text be that big of a problem"
    - From former Lighthouse dev now @ Gatsby:
        - "So let's talk about Gatsby. Gatsby itself is still pretty fast and we're improving it even more. We're creating new API's so page builders like MDX, Contentful's rich text, ... can optimize the bundle as well. A lot can be done by optimizing your LCP. Make sure when using fonts & images, they aren't loaded lazily and are loaded as soon as possible. These assets should be loaded from the same origin as your site, they should not be loaded from a CDN."
        - "If you want to drop TBT, you should checkout preact. Preact has the same API as React but has a smaller javascript footprint."
        - "Somethings I noticed when profiling gatsbyjs.com & gatsbyjs.org is that we should load google analytics, etc a bit later than we do now to make sure it doesn't become part of TBT."
        - "We'll be optimizing Gatsby even more to make sure our users can get 100's for free."
        - "if you have content that uses setTimeout/setInterval that switches react component, it will take it into account. The latter approach will give you really bad CLS scores."
    - "We had some success just today by using the loading prop for Gatsby image on images that are above the fold (Hero images for us). We've been trying to work on Largest Contentful Paint and this has yielded good results in some initial tests. There is an important part I almost missed to this: If you set loading="eager" for your topmost image, you might want to set fadeIn={false} as well for that image because the transition between the base64 and fully loaded image takes time which delays LCP."
    - "I found a way to work with gatsby image inside contentful's rich text without that query hack! Here's the gist. The code was copy pasted from gatsby-source-contentful. Basically you can generate the contentful fluid or fixed props outside of GQL. Which is perfect for contentful's rich text." https://gist.github.com/daydream05/b5befd50f9c9001fb094f331f98a3ec5
    - "Something just doesn't add up for me. I built a very simplistic website with about an image per page, Im using SVG for images without gatsby-image, I also tried removing google analytics and that didn't make much difference, my score was about 50 - 60 for performance."
    - "I think I made some good progress here. I got my scores up from 57 to 84 with very basic changes. My LCP went from 12s to 2s.

        That said, it is inconsistent. Since making the changes I'll describe below, my score varies from 69 - 84. There's clearly some random variance to the performance scores.
        TLDR

        First, like @KyleAMathews and @Jimmydalecleveland suggested, I tried setting loading="eager" and fadeIn={false} on my gatsby image components that were above the fold.

        Next, I got rid of base64 from my queries.

        These made a huge difference.
        
        Adding _noBase64 to my image fragments brought my score up 20 points!"
    - "I think LCP penalizes any animations in our hero pages which is a bummer."
    - "Thanks for this thread as a discussion on achieving good lighthouse scores is needed. Superb scores have become more difficult with v6, mostly due to the new LCP metric. My site (https://www.jamify.org) dropped from ~100 to ~70 with Lighthouse v6."
        - "In order to bring it back to 100 on desktop, I had to
            - remove a background image that was not needed (as it was wrongly chosen as the LCP)
            - optimize size of images
            - set gatsby-image to loading="eager" and fadeIn=false (that's really a bummer as I like the blur-up effect)
- Google dev Tweet/Twitter thread on matter: https://twitter.com/addyosmani/status/1277293541878673411. 
    - That tweet: 
        - "We discussed LQIP/placeholders this week. FCP should reward visual updates before LCP, but a few ideas being considered..."
        - Accept the first image as LCP if it is the exact same size as the second. This help?
        - Keep removed images as LCP
    - From above thread: "Lesson here is don't treat the new perf scores as absolute just yet — they're refining edge cases still."

## LIGHTHOUSE 6

### Lighthouse 6 Summary
- One new speed metric is Largest Contentful Paint, LCP
    - LCP is the render time of the largest content element visible in the viewport
    - Content elements:
        - Foreground images
            - ```<img>```
            - ```<image>``` inside ```<svg>```
        - Contentful bg images
            - The element referencing the background image
        - Video elements (with poster images)
        - Text
            - Block-level elements containing text
    - Size = width * height
    - Margin, padding & border are not considered part of the size of the element
    - Actual element size is used (i.e., downsizing is punished), except in cases where the content overflows the viewport; in that case, only the dimensions in the viewport are considered
    - **Render time**
        - Images
            - The time of the paint immediately following the image's onload
            - **Progressive images get punished; time to complete download**
                - This might be the crux of the issue
        - Cross-origin images
            - If served with Timing-Allow-Origin header, paint time will remain accurate
            - Otherwise, it will be identical to image's onload time
        - Video elements (with poster images)
            - The render time of the poster image
        - Text
            - The time of the first paint of the text at its earliest font
            - Even if a webfont upgrades the text later, the render time points to the earliest paint
    - **LCP's peculiarities**
        - Paints aren't considered after input
            - If the user taps, scrolls, etc. the broswer stops looking for another LCP candidate
        - LCP is considered final when user input is received, or on page unload
        - If an LCP candidate element is removed from the DOM, a new LCP candidate is found
    - Can be measured with WebPerf APIs
        - PerformanceObserver listening for ```type: 'largest-contentful-paint' ```
        - ```   const po = new PerformanceObserver(list => { ```  
            ```     const entries = list.getEntries(); ```  
            ```     const lastEntry = entries[entries.length - 1]; ```  
            ```     console.log('LCP is', lastEntry.startTime); ```  
            ``` }); ```  
            ``` po.observe({type: 'largest-contentful-paint', buffered: true }) ```
        - Can be loaded async; doesn't need to be in head
    - LCP Chrome Extension
    - LCP is not part of Paint Timing API

### Lighthouse 6 notes, errata & resources
- Moovweb also reporting widespread perf regressions: https://www.moovweb.com/ecommerce-lighthousev6-0-scores-plummet/
    - 72.1% of IR500 sites are seeing lower scores than before
    - "Google Play’s performance score dropped by 32 points, Net-a-Porter lost 28 points and the eCommerce legend, Amazon, saw its performance score drop 21 points to a score of 32. Other brand names to lose points include Adidas, Gap and the eCommerce darling Warby Parker, all of which lost 20 points or more. And Apple, Nike and Walmart’s performance scores dropped by 15 points."
- **Paul Irish (Lighthouse dev): Investigating LCP: Largest Contentful Paint:** https://www.youtube.com/watch?v=diAc65p15ag
    

## WEB.DEV 2020 DAY 1

### CORE WEB VITALS

- Content needs to load quickly
    - Largest contentful paint (LCP)
        - Threshholds:
            - < 2.5s === good
            - Between 2.5s & 4.0s === needs improvement
            - Greater than 4.0s === poor
- Quick interactivity
    - First input delay (FID)
- Visual stability
    - Cumulative layout shift (CLS)

- Core Web Vitals getting planned refresh every year around the time of Google.io (May/June)

- Tools:
    - PageSpeed Insights
        - https://developers.google.com/speed/pagespeed/insights
    - Chrome UX report
    - Search Console
    - Chrome DevTools
    - Lighthouse
    - Web Vitals Extension


- TBT (Total Blocking Time) is a lab proxy for FID

- Re: Monitoring:
    - https://github.com/bejamas/gatsby-plugin-web-vitals

- Overviews/misc resources:
    - https://web.dev/vitals
    - https://web.dev/vitals-tools
    - https://tooling.report

### WHAT'S NEW IN SPEED TOOLING

- Core web vitals aim to measure 75th percentile experiences
- Suggested tooling flow:
    - Get a side-wide view with Search Console's Web Vitals report
    - Diagnose issues with PageSpeed Insights
    - Optimize locally with Lighthouse and Chrome Dev Tools
    - Get tailored guidance with web.dev/measures
    - Prevent regressions by using Lighthouse CI on PRs before deployment
    - Monitor with a dashboard via the CrUX API/Dashboard to see performance over the past month

### OPTIMIZING FOR CORE WEB VITALS

- CLS
    - Poor CLS caused by
        - Images without dimensions
            - Give images dimensions
            - Can also use aspect ratio
        - Ads, embeds, iframes w/out dimensions
            - Reserve space for ads or dynamic content
            - Avoid inserting new content above existing content, unless it's in response to a user action
        - Dynamically injected content
            - Render widgets with better defaults server-side to avoid CLS due to rehydration
        - Web fonts causing FOIT/FOUT

- LCP
    - Poor LCP caused by
        - Slow server response times
        - Render-blocking JS & CSS
        - Slow resource load times
        - Client-side rendering
    - Optimize images
        - Get rid of images where practical
        - Compress images & use better formats
        - Use an image CDN
    - Defer any non-critical JS & CSS to speed up loading of main content
    - Server push

- TBT / FID
    - Poor TBT / FID caused by
        - Long tasks
        - Long JS execution time
        - Large JS bundles
        - Render-blocking JS


## QUESTIONS

- BigQuery?