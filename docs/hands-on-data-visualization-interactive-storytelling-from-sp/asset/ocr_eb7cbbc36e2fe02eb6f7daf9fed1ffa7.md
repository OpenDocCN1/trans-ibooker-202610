# To Self-Hosted WordPress Sites

Make sure you understand the difference between a WordPress.com site and a self-hosted WordPress site. The latter is sometimes called a WordPress.org site because anyone can freely download the software from that address and *host it on their own web server*, or, more commonly, have access to a self-hosted WordPress server through their school or work, or by renting space on a vendor’s web server. But the web address of a self-hosted WordPress site does not necessarily need to end in .org. It also could be .com or .edu or any other ending, so don’t let that confuse you.

There are two ways to insert an embed code or iframe in a self-hosted WordPress site, but your success may depend on your WP version, your access level, and the complexity of the code. We’ll show you both Method A (which is simpler, but not always reliable) and Method B (which requires a few more steps, but works more reliably). See which method works best for your self-hosted WordPress site.

# Method A: simple, but not always reliable

Assume that you’re using self-hosted WordPress version 5.0 or above with the newer block editor, and you have editor or administrator access to your site. (This method does not work reliably with author-level access or below.)

+   1. In your block editor, select a *custom HTML* block, and directly insert the embed code or the iframe.

![](bbox=[0.223, 0.275, 0.901, 0.589])

```
```html
<iframe width="631" height="376" seamless frameborder="0" scrolling="no"
src="https://docs.google.com/spreadsheets/d/e/2PACX-
1VskwNryYcPN†saCRxn7g5o4UIR8OJXSxBwWlK531±5vYubt39hecV9yL1fic89DeEJgNGvRpT
tFFkZn/pubchart?oid=787918829&amp;format=interactive"></iframe>
```
```

+   2. Preview your WordPress page or post, and if your iframe appears, publish and view it in another browser to test how it appears to your readers.

# Method B: more steps, but more reliable

Assume that you’re using self-hosted WordPress, any version, with either the classic or block editor, and that you have author-level or above access to the site.

+   1. First, the site administrator must install and activate the iframe plug-in. This plug-in allows authors to embed iframe codes in a modified “shortcode” format surrounded by square brackets in this general format: `[iframe...]`.

![](bbox=[0.223, 0.749, 0.899, 0.877])

+   2. In the WordPress block editor, click to add a *Custom HTML* block (or in the classic editor, click the “text” tab to view the HTML code). Paste the embed code or iframe, which initially should appear similar to the “Method A: simple, but not always reliable” step 1 screen.

+   3. Initially, the code you pasted probably included HTML iframe tags at the start (`<iframe...>`) and the end (`...</iframe>`). Modify the start tag by replacing the less-than symbol (`<`) with a square opening bracket (`[`). Modify