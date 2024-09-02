# Wiki Setup

**About the technical setup of this wiki**

This is a meta-page for housekeeping and documentation.  This information is really only mainly relevant to admins, in order to keep the wiki running as intended, should anything need to be re-implemented.

The settings below are the main things that one would need to configure, in order to "rebuild" the wiki as it exists, from scratch, from a fresh install of DokuWiki.

## Plugins (extensions)

### 404 error code

The plugin [Whennotfound plugin](https://www.dokuwiki.org/plugin:notfound) is a good one.  Set it up like this:
 - Under `actions`: check `pagelist` only, and enter the text `PAGE:404` in the textbox.
 - Then, make a wiki page at `/404` containing the following markdown, for example:

```
# Error 404 - Page Not Found

That page does not exist.  Try another one!
```

### Cookie option plugin
- The [cookiebanner Plugin](https://www.dokuwiki.org/plugin:cookiebanner) is "A Consent Banner Plugin that actually controls JS loading".

### CommonMark Plugin

With [DokuWiki CommonMark Plugin](https://www.dokuwiki.org/plugin:commonmark) installed, disable section edits in order to avoid breaking page contents/formatting, by setting the following Configuration page value:
 - `maxseclevel` (Maximum section edit level) = `0`

Currently we have installed and enabled the **DokuWiki CommonMark Plugin** for great Markdown support and compatibility.  For the version of CommonMark that is installed, check [the CommonMark 0.30 syntax reference](https://spec.commonmark.org/0.30/) for a complete syntax description.  Support is anticipated to be pretty close to that.

  - Previously, we had the [markdowku Plugin](https://www.dokuwiki.org/plugin:markdowku), but compatibility with the currently installed version of DokuWiki was not 100%, so this was removed.

### HTML blocks

Currently, the [config:htmlok Plugin](https://www.dokuwiki.org/plugin:confightmlok) plugin is not in use.  However, installing and enabling the **config:htmlok Plugin** plugin (by checking its two boxes in the Configuration page) then would allow HTML to be embedded freely, even with the Commonmark plugin in use.

Exercise great caution if using this plugin, and only highly trusted editors can be allowed when the HTML plugin is in use.

To use it, place your custom HTML inside of `<html></html>` tags.

### Link removal

- The [tplmod (template modifier) Plugin](https://www.dokuwiki.org/plugin:tplmod) plugin adds config options for the default theme, to remove various kinds of links for non-logged-in users.
- Once enabled, the `Tplmod` Configuration section can have the following settings made:
  - Under `Site Tools` check `Media Manager` to hide the Media Manager link
  - Under `Pages Tools` check `Edit/View Source`, `Backlinks`, `Revert`, and `Subscribe`).
  - Under `ptools_xcl` set the string to an empty string (a totally empty box).  The default value here is: `dokuwiki__top,login`.

### Page Move
- The [Move Plugin](https://www.dokuwiki.org/plugin:move) is installed and enabled, but only the admin can control it currently.
  - It is easy to inadvertently move an entire directory of pages at once using this plugin.
    - Use the Move plugin carefully if at all.  Review the test output before applying a change.
  - If you need to bulk move pages or move a page and update links, just ask.

## Configuration settings

### Sneaky index
- The [sneaky_index](https://www.dokuwiki.org/config:sneaky_index) setting is enabled, so that namespace names cannot "leak" via the Sitemap to users who are not logged in.  If you are not concerned with this, don't worry about it.

### Breadcrumbs

In Admin>configuration, set `breadcrumbs` to 0 (to show 0 recent breadcrumbs).
In Admin>configuration, set `youarehere` to "checked" (to show "hierarcical breadcrumbs").

### Clean URLS
For shorter, more readable URLS, (to avoid PHP page names in the URLs):
- In Configuration, set: `userewrite` or `userewritedanger` ("Use nice URLs") = `.htaccess
- Update the file `conf/local.php` to contain: `$conf['userewrite'] = 1;`
- Copy `.htaccess.dist` to `.htaccess` and uncomment the section under "Uncomment these rules if you want to have nice URLs using".
  - Update or include/comment-out the line for `RewriteBase` based on the installation location.

### Slash-separated namespacse

So that URLs contain hierarchical formatting like `example.com/section/subsection/page` instead of `example.com/section:subsection:page`:
- In Configuration, check `useslash` so that the "slash namespace separator in URLs" option is selected.

## Template Style Settings

### Page width
We set the "The width of the full site" to `100%` instead of the default of `75em` at: [https://example.com/start?do=admin&page=styling](https://example.com/start?do=admin&page=styling)
- This allows wide tables to be more useful on wide monitors
- This allows people on desktops to customize the width of their window, with the contents reflowing appropriately.
- This does not prevent phone or tablet resolutions from working as intended.

## Users and roles

In the Access Control List here are two custom user roles that have been created and used:

- Users with the "editors" role can modify and create pages, scoped to the entire wiki.
- There are some users with just the "editors" role.
- Users with the "viewonly" role can only read pages, scoped to the entire wiki.
- There are some users with just the "viewonly" role.

The current plan is to rotate the user passwords every n days when appropriate, and update the locations where the passwords are stored for access, accordingly.

## Logo

We upload the custom logo, overwriting the default theme/template locations within the `tpl` directory where appropriate (e.g., in `lib/tpl/dokuwiki/images`):

- `logo.png` (128x128 px)
- `favicon.ico` (generated from a higher res source)
- `apple-touch-icon.png` (generated from a higher res source)

## Tooltips (acronyms and abbreviations)

For now, tooltips are disabled by commenting out all acronyms in `conf/acronyms.conf`.  To add tooltips for acronyms, uncomment or add some lines there.

- We haven't chosen to add any custom tooltips yet, but, if wanted, custom tooltips could be added to the `https://www.dokuwiki.org/abbreviations` file.

- If tooltips get enabled, you might notice that some acronyms automatically receive tooltips.  For example, the word URL can be mouse-hovered to see the explanation, "Uniform Resource Locator".  DokuWiki comes with some built-in tooltips, as described in the [documentation for Abbreviations and Acronyms](https://www.dokuwiki.org/abbreviations).

# Force HTTPS
Added these lines to .htaccess as described at [https://my.hostmantis.com/knowledgebase/263/How-do-I-automatically-redirect-HTTP-to-HTTPS.html](https://my.hostmantis.com/knowledgebase/263/How-do-I-automatically-redirect-HTTP-to-HTTPS.html):
```
# Added below line RewriteEngine On to Force HTTPS
RewriteCond %{HTTPS} !=on
RewriteRule ^(.*)$ https://%{SERVER_NAME}%{REQUEST_URI} [L,R=301]
```

# Customize footer image links
Remove Donate, PHP, HTML5, CSS, and DOKUWIKI images from the following file: `lib/tpl/dokuwiki/tpl_footer.php`.  E.g., remove the  following lines:
```
    <div class="buttons">
        <?php
            tpl_license('button', true, false, false); // license button, no wrapper
            $target = ($conf['target']['extern']) ? 'target="' . $conf['target']['extern'] . '"' : '';
        ?>
        <a href="https://www.dokuwiki.org/donate" title="Donate" <?php echo $target?>><img
            src="<?php echo tpl_basedir(); ?>images/button-donate.gif" width="80" height="15" alt="Donate" /></a>
        <a href="https://php.net" title="Powered by PHP" <?php echo $target?>><img
            src="<?php echo tpl_basedir(); ?>images/button-php.gif" width="80" height="15" alt="Powered by PHP" /></a>
        <a href="//validator.w3.org/check/referer" title="Valid HTML5" <?php echo $target?>><img
            src="<?php echo tpl_basedir(); ?>images/button-html5.png" width="80" height="15" alt="Valid HTML5" /></a>
        <a href="//jigsaw.w3.org/css-validator/check/referer?profile=css3" title="Valid CSS" <?php echo $target?>><img
            src="<?php echo tpl_basedir(); ?>images/button-css.png" width="80" height="15" alt="Valid CSS" /></a>
        <a href="https://dokuwiki.org/" title="Driven by DokuWiki" <?php echo $target?>><img
            src="<?php echo tpl_basedir(); ?>images/button-dw.png" width="80" height="15"
            alt="Driven by DokuWiki" /></a>
    </div>
```

# Exporting

Any page can automatically be exported as plain HTML by appending `?do=export_xhtml` to the page URL, as per [export docs](https://www.dokuwiki.org/export).


# Hiding Log In link and graphic from pages

You can still log in at `https://example.com/?do=login`.
When using the default theme, hide the login link create files in `<dokuwikiroot>/conf/` named `userstyle.css` and `userall.css` that contain the following CSS:
```
/* Hide the entire top-of-page Log In list item, including span, svg, and all child elements */
#dokuwiki__usertools li.login {
        display: none !important;
}
#dokuwiki__usertools .login {
        display: none !important;
}
/* Additional specificity if needed to ensure the SVG is hidden */
#dokuwiki__usertools .login svg {
        display: none !important;
}
```

Troubleshooting: **If the custom CSS in the userstyles.css or userall.css files is not being picked up** (e.g., "Log In" is still visible), then see the tips in the [Styles are not loading](https://www.dokuwiki.org/faq:no_styles) page, such as the following:
- In Configuration, enable (check) the `compress` option for "Compact CSS".  This disables CSS compression in the Dokuwiki configuration.  Optionally, then refresh an affected page, see that it is fixed, and then enable compression again, and re-test to make sure the fix persisted.
- Or, view CSS separately at `http://yourdomain/lib/exe/css.php?purge=tru` to inspect for strangeness.


# Hiding username who last modified page in page footer

Edit file `<dokuwikiroot>/lib/tpl/dokuwiki/main.php` to contain the following:

`<div class="docInfo"><?php echo preg_replace("/^.*·| by.*/", "", tpl_pageinfo(true)) ?></div>`

 - which shows a format like: `Last modified: 2024/08/13 18:56`

or use the following:

`<div class="docInfo"><?php echo preg_replace("/ by.*/", "", tpl_pageinfo(true)) ?></div>`

 - which shows a format like: `home.txt · Last modified: 2024/08/13 18:56`

instead of the default line which matches the following:

`<div class="docInfo"><?php tpl_pageinfo() ?></div>`

 - which shows a format like: `home.txt · Last modified: 2024/08/13 18:56 by dwdevwikiadmin`

# Add sidebar navigation

Create a page at `/sidebar`.  This [becomes the sidebar](https://www.dokuwiki.org/config:sidebar), when using the default theme.

For specific sidebars specific to namespaces and their children, include correctly-located sidebar pages, for example:

- Create a page at `/namespace:sidebar` for a custom sidebar specific to /namespace.
- Create a page at `/namespace:subspace:sidebar` for a custom sidebar specific to /namespace:subspace.

# Hide sidebar and specific other pages from showing up in the Sitemap

Edit `/conf/local.php` and add the following line:

`$conf['hidepages'] = '(admin*|sidebar|playground|labels|templates|wiki)';`

which, for example, hides the following page matches so that they are not listed in the Sitemap page:

- pages or pages within namespaces prefixed with admin,
- the main sidebar page
- the playground namespace
- the wiki namespace
- the templates namespace
- the labels namespace

# Add a robots.txt

At /robots.txt, you can create a file containing the following:

```
# bots not allowed
User-agent: *
Disallow: /
```

# Add custom JavaScript to each page, e.g. for an analytics tag

Edit the file at `/lib/tpl/dokuwiki/main.php` and add the JS `<script></script>` tags in the `HEAD` section just before `</head>`.

