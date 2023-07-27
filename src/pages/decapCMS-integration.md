---
layout: layouts/documentation.njk
permalink: /decapCMS/
eleventyNavigation:
  key: DecapCMS
  parent: Documentation
header_title: DecapCMS Integration
header_subtitle: Setting up DecapCMS with Github-Pages
---
# Adding DecapCMS to Existing GitHub Pages Site
---
</br>

1. Creating an GitHub OAuth App

    First, go to ***[GitHub Dev Settings](https://github.com/settings/developers)*** and click **New OAuth App**.

    Enter whatever you like for **Application name** and **Homepage URL**.

    In **Authorization callback URL**, enter: `https://api.netlify.com/auth/done`

    You will need the **Client ID** and **Client Secret** on this page later.

2. Creating a Netlfiy Site

        Go to ***[Netlify](https://app.netlify.com/account/sites)*** and create a new site from your site's repo. We are not really using Netlify to host.

        After that, go to **Settings**, and copy your **Site name**. It should be something like `deluxe-lama-123456c`.

        From the sidebar go to **Domain Management** and add your GitHub Pages domain (`you.github.io`) or your custom domain as a custom Netlify domain. Choose **Yes** when asked if you are `github.io`’s owner.

        From the sidebar go to **Access control**, scroll down to **OAuth** and click **Install provider**.

        Choose **GitHub** as provider, and enter the **Client ID** and **Client Secret** from GitHub OAuth app page mentioned above.

3. “Installing” the CMS

      We are now going to add the CMS files into our static Eleventy site.

      Under the root directory of your site, create a folder named admin, and create two files `index.html` and `config.yml`

      Copy and paste this into your `index.html` </br></br>

      ```
      <!doctype html>
      <html>
      <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Content Manager</title>
      </head>
      <body>
        <!-- Include the script that builds the page and powers Netlify CMS -->
        <script src="https://unpkg.com/netlify-cms@^2.0.0/dist/netlify-cms.js"></script>
      </body>
      </html>
      ```
      </br></br>

      Next, Copy and Paste this into your `config.yml` </br></br>

      Be sure to replace `you/you.github.io` with your repo, and `deluxe-lama-123456c.netlify.com` with `your-site-name.netlify.com` </br></br>

      ```
        backend:
          name: github
          repo: you/you.github.io
          branch: main
          site_domain: deluxe-lama-123456c.netlify.com

          collections:
              - name: "posts"
                label: "Posts"
                folder: "src/posts"
                create: true
                fields:
                  - { label: "Title", name: "title", widget: "string" }
                  - { label: "Description", name: "description", widget: "text" }
                  - { label: "Date", name: "date", widget: "datetime" }
      ```
      </br></br>

      For more information about the fields field, please go to ***[DecapCMS](https://www.netlifycms.org/docs/add-to-your-site/#collections.)***


      Save the files, commit, and push to GitHub. Done. Visit ***https://you.github.io/admin*** to see your CMS </br></br>


# Additional Resources:
---
***[Adding NetlifyCMS to existing GitHub Pages](https://cnly.github.io/2018/04/14/just-3-steps-adding-netlify-cms-to-existing-github-pages-site-within-10-minutes.html)***
