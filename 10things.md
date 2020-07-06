# 10 Things We Need to Take Seriously about Speed and How to Do It

1. Optimize for new metrics on a per-case basis: 
2. LCP (Largest Contentful Paint)
    - Images are the big offender here
        - Make sure compression is optimized to reduce filesize; consider serving different formats to different browsers is it makes sense
        - Downsizing is punished; make sure you're serving images at the size they will be viewed
        - Make sure cross-origin images are served with the Timing-Allow-Origin header; otherwise, paint time will be identical to onload time
        - LazyLoad images in a way to not fetch any images that are below the fold; but since LCP is  calculated based on the viewport, if a link takes a user to a certain part of a page, the LCP would be different than at the top.
        - We should try to defer any non critical javascript or CSS in order to speed up the loading process. For CSS this could look like running critical CSS inline rather than keeping it inside style sheets that load later, or maybe having a specific ‘critical.css’ file with critical stylin, or even keeping non-critical style sheets in an array on the server side and fetch them with some sort of deferred script.
        - Server Push can be used to push critical content that we know will be requested before it has been requested. This could look like returning the HTML when it is requested, as well as the CSS followed by the Javscript. There is nuance though, as Server Push is not http cache-aware. This can be used to gain efficiency in an idle network.


6. Monitor performance using CRux or custom dashboard


# Remedies for 64labs.com