# Improving UX Through Front-End Performance

Imagine you’re at an intersection waiting for your turn to walk across the
street. You push the button to call the walk signal, and you take out your
phone. You want to accomplish one thing: maybe check your e-mail, add an item to
your to-do list, or check Twitter. You have a limited amount of time to
accomplish that one thing.

That amount of time is how long users have to finish what they want to do on
your site. And it matters.

Adding half a second to a search results page can decrease traffic and ad
revenues by 20 percent, according to a Google study. The [same article][1]
reports Amazon found that every additional 100 *milliseconds* of load time
decreased sales by 1 percent. Users expect pages to load in two seconds—and
after three seconds, up to 40 percent will [simply leave][2].

Can you keep up? If you’re designing sites with rich content, lots of dynamic
elements, larger JavaScript files, and complex graphics—like so many of us
are—the answer might be “no.”

It’s time we make performance optimization a fundamental part of how we design,
build, and test every single site we create—for every single device.

## Designing for performance

Website performance starts with design. Weigh a design choice’s impact on page
speed against its impact on your site’s conversion rate. Do you really need
eight different shades of blue? What value does this 1,000px-wide background
image add? Will replacing a sprite with an icon font actually add more page
weight and slow rendering, or will it be faster than the original image?

Not every design decision will favor performance. I’ve found that a button style
that slightly slows page speed can still increase conversions, and it’s worth
the small web performance sacrifice.

But sometimes, performance will win. I once had a landing page redesign that
added a significant amount of images to a page, and I wasn’t sure whether the
performance hit would have a negative impact on conversions, so I rolled the
redesign out to a small subset of users in an [A/B test][3] to see what the
impact would be. The new design took twice as long to load, and I immediately
saw a high exit rate and lower conversion rate, so we kept the original
lightweight design. Being wrong is okay—it’s what gives you a benchmark.

In another experiment, the [dyn.com][4] homepage featured a thumbnail image
section with 26 images that rotated in and out of 10 slots.

![dyn.com][dyn.com homepage]

*dyn.com homepage.*

My teammate at the time put all 26 images into a sprite, which:

* Increased the total homepage size by 60K with the increased CSS, JavaScript, 
and image size needed to recreate this effect with the sprite

* Decreased the number of requests by 21 percent

* Cut the total homepage load time by a whopping 35 percent

This proves that it’s worth experimenting: We weren’t sure whether or not this
would be a page speed success, but we felt it was worth it to learn from the
experiment.

## Coding for performance

Clean your HTML, and everything else will follow.

Start by renaming non-semantic elements in your HTML. This is probably the
toughest, but once you start thinking about theming in terms of semantics like
“nav” or “article” and less with design or grid names, you’ll make significant
headway. Often we get to elements with non-semantic names by way of needing more
weight in CSS selectors, and instead of cleaning our CSS and adding specificity
the right way, we add unnecessary IDs and elements to our HTML.

Then, clean up your CSS. Remove inefficient selectors first. In a [study I
performed][5] for [writegoodcode.com][6], I found that adding inefficient
selectors to a CSS file actually increased page load time by 5.5 percent. More
efficient CSS selectors will actually be easier to redesign and customize the
styles of in the future since they are easier to read in your stylesheet and
have semantic meaning. Repurposable, editable code often goes hand-in-hand with
good performance. In that case study, I saved 39 percent of the CSS file size by
cleaning my CSS files.

Next, focus on curing your HTML of `div`-itis. Typically the cleaner your markup
ends up, the smaller your CSS will be, and the easier redesigning and editing
will be in the future. It saves you not just page load time, but development
time too.

Last, focus on creating repurposable code, which saves time and results in
smaller CSS and HTML files. Less HTML and CSS will be significantly easier to
maintain and redesign later, and the smaller page sizes will have a positive
impact on page speed.

## Optimizing requests

Requests are when your browser has to go fetch something like a file or a DNS
record. The cleaner your markup, the fewer requests the browser has to make—and
the less time users will spend waiting for their browser to make those round
trips.

In addition to clean markup, minimize JavaScript requests by only loading it
when absolutely necessary. Don’t call a file on every page if you don’t need it
on every page. Don’t load a JavaScript file on a responsive design that is only
needed for larger screens; for example, [replace social scripts with simple
links][7] instead. You can also load JavaScript asynchronously so that the
JavaScript won’t block any content from rendering.

Though a responsive design typically means more CSS and images (larger page
weight), you can still get faster load times from larger page sizes if you cut
requests.

## Optimizing images

Save as many image requests as you can, too. First, focus on [creating
sprites][8]. In my writegoodcode.com study, I found that a sprite for the icons
in the example cut page load time by 16.6 percent. I like to start cleaning up
images by creating one sprite for repeating backgrounds. You may need to create
one for vertical repeats and one for horizontal repeats.

Next, create one transparent sprite for no-repeat backgrounds. This will include
things like your logo and icons. As you get more advanced, you can also use
tools like [Grunticon][9], which takes SVG icon and background images and
figures out how best to serve them based on the user’s browser’s capabilities.

After you regenerate your images, run them through an optimizer like
[ImageOptim][10]. Similarly, retina-sized images can still be made smaller with
[extensive compression][11] that isn’t noticeable in the end result.

Now see which images you can replace with CSS3 gradients. This will not only
make a dent in page load time, but it will also make it infinitely easier to
edit the site later, as developers won’t have to find original image files to
edit, regenerate, or re-optimize in the future.

Last, look at using [Base64 encode][12], which allows you to embed an image into
your CSS file instead of calling it using a separate URL. It ends up looking
like this:

    #nav li:after {
        content:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAI0lEQVQIW2P4//8/w8yZM//DMIjPAGPAMIiPWxCIMQQxzAQAoFpF7lGFr24AAAAASUVORK5CYII=);
    }

The random letters and numbers add up to a small circle that’s used in many
places within [dyn.com][13]. Embedding the image allows you to save an image
request each time you want to use it in your design. Embedding images using this
method will make your CSS file larger, so it’s worth testing the page load time
before and after to ensure you’re making improvements.

## Measuring performance

Now for the fun part: determining whether your efforts are paying off.

Both Google’s [PageSpeed][14] and Yahoo!’s [YSlow][15] offer suggestions on how
you can improve page load time, including identifying which elements block page
rendering and the size of your page’s different components like CSS or HTML.

I also recommend the YSlow extension [3PO][16], which checks your site for
integration with popular third-party scripts like Twitter, Facebook, and
Google+. The plugin can give you recommendations on how to further optimize the
social scripts on your page to improve page load time.

[WebPageTest.org][17] has been my go-to benchmarking tool ever since I first 
started making improvements based on PageSpeed and YSlow’s suggestions. It gives 
very detailed information about requests, file size, and timing, and it offers 
multiple locations and browsers to test in.

Benchmarking can help you troubleshoot as you design. Measuring performance and
analyzing the results will help you make both your large- and small-screen
designs faster. You can also test and benchmark techniques like [conditional
loading of images][18] as you get more comfortable developing for performance.

## The impact of web performance

Web performance affects your users—and that means its everyone’s job to
understand it, measure it, and improve it. All of these techniques will lead to
better page load time, which creates a significant improvement to your site’s
user experience.

Happier users mean better conversion rates, whether you’re measuring in revenue,
signups, returning visits, or downloads. With a fast page load time, people can
use your site and accomplish what they want in a short amount of time—even if
it’s just while they’re waiting for a walk signal.

[1]: http://www.websiteoptimization.com/speed/tweak/psychology-web-performance/
[2]: http://www.gomez.com/pdfs/wp_why_web_performance_matters.pdf
[3]: http://alistapart.com/article/a-primer-on-a-b-testing
[4]: http://dyn.com/
[5]: http://dyn.com/blog/how-we-got-dyndns-com-to-load-faster-and-how-you-can-learn-from-it/
[6]: http://writegoodcode.com/
[7]: http://www.zurb.com/article/883/small-painful-buttons-why-social-media-bu
[8]: http://alistapart.com/article/sprites/
[9]: http://filamentgroup.com/lab/grunticon
[10]: http://imageoptim.com/
[11]: http://blog.netvlies.nl/design-interactie/retina-revolution/
[12]: http://www.greywyvern.com/code/php/binary2base64
[13]: http://dyn.com/
[14]: https://developers.google.com/speed/pagespeed/
[15]: http://developer.yahoo.com/yslow/
[16]: http://www.phpied.com/3po/
[17]: http://www.webpagetest.org/
[18]: http://adactio.com/journal/5414/

[dyn.com homepage]: img/dyncomhomepage.jpg?raw=true&amp;repo=improving-ux-through-front-end-performance