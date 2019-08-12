---
layout: post
title: "GitHub - Pages generated with Jekyll"
categories: GitHub Jekyll
---
Creating and Hosting a Jekyll website on GitHub Pages.

## #Steps
- Sign In
- Create Repository
- Add a file: index.html
    - Add document & styling to index.html
- Add a branch `gh-pages`
- Go to Setting:
    - At GitHub Pages select `gh-pages`, if not automatically selected.
 
*GitHub Pages looks for index.html / index.md in the root directory, of gh-pages branch.*

## #Adding a custom domain
- Add a `CNAME` file to root directory.
    - with **ONLY** the domain name without space.
- Configure (Manage) the domain:
    - Zone file
        - Add 2 **A (HOST) Record** in domain register with GitHub IP addresses:
            - Record Type: A (Host)
            - Host: @
            - Points to: *IP address*
            - TTL: Custom
            - Seconds: 600

## #Add a Sub Domain
- To have a sub-domain one needs `CNAME` file to the root.
    - With the `subdomain.domain.com`, and without space.
- Configure Domain:
    - Zone file
        - Add CName (Alias) Record:
            - Record Type: CNAME (Alias)
            - Host: subdomain.domain.com
            - Points to: usrname.github.io
            - TTL: 1 Hour


## #Jekyll Blog on GitHub Pages
- Jekyll static site generator: `jekyllthemes.org` 
- Search the theme on GitHub
- Fork the repository
- To host the project, add the project to the `gh-pages` branch.
    - Sometimes there is a need to change the "baseurl" in _config.yml file. To load assets from the base of the repository.
    - If a default CNAME file exists, change it to your domain or remove it.
    - Remove "relative_permalinks" from _config.yml because it has been depreciated from Jekyll 3.0

## #Creating a Contact form in Jekyll
- Use `formspree.io` in the website.
    - Verify your email to `formspree.io`

```
    form action="https://formspree.io/email@domain.tld" method="POST">
        <input 
            type="text" 
            name="name"
        ><br/>
        <input 
            type="email" 
            name="_replyto"
        ><br/>
        <!--Add desired input attribute here-->
        <input type="submit" value="Send">
    </form>
```
- Use `_next`:
    - By default, after submitting a form the user is shown the Formspree "Thank You" page. You can provide an alternative URL for that page.

## #Sitemap on Jekyll
- A sitemap is an XML that has a link to all the posts and pages.
    - A sitemap helps bots look for all the links and index them in the search engine DB.
    - The next step is to submit the sitemap to all the webmaster tools.

- Edit `_config.yml` in the root directory
- Add:

```yml
gems: 
    - jekyll-sitemap
```
- Submit it to the webmaster.

## #Posts in Jekyll
- Post file name syntax in `_posts` directory should be:
    - `YYYY-MM-DD-title.md`
- Frontmatter:
```
---
layout: page
title: Contact
published: true
---
```

## #Using Disqus comments to Jekyll
- Sign up for Disqus:
    - Verify your email.
- Add Disqus code snippet to `_layouts/post.html`.

## #Using MailChimp subscription in Jekyll
- Sign up for MailChimp:
    - Verify your email.
- Create List
- Generate Embedded Form
    - Copy the code.
- Create a `contact.md` file in the root.
    - Paste the embedded code with `layout` & `title` frontmatter.