## Are you using Docusaurus v2?

If so you may want to check out this [repo](https://github.com/sajclarke/docusaurus2-netlifycms) instead


## Using Netlify CMS with Docusaurus v1

This repo is for using [Netlify-CMS](https://www.netlifycms.org/) in order to edit the documentation and blog posts found in the default website built using [Docusaurus](https://docusaurus.io/) version 1. According to the [official website](https://v2.docusaurus.io), version 2 is not ready for production

### Demo
1. Video demo: https://www.loom.com/share/cb7066a1e55542608b4fa911ce2891c1
2. Live demo site: https://elegant-galileo-a5ce1d.netlify.app/

### Getting Started

:point_up:
If you want to test locally then skip ahead to [the test locally section](#Test-Locally). Otherwise, read below for how you can setup this project from scratch

### Pre-requisites
The following are required to complete the step-by-step tutorial below:
1. Github account (www.github.com)
2. Git CLI and Github CLI installed
3. Netlify account (www.netlify.com)

### Tutorial

:point_up:
Please replace [projectName] with your desired title for the project e.g. myApp

##### Setup Docusaurus Default Site
1. Create project folder in your desired location using `mkdir [projectName] && cd [projectName]`
2. Inside the project folder, run the command `npx docusaurus-init`
3. Try the default website on localhost by running the command `cd website && yarn start`. Visit `http://localhost:3000` on your browser and confirm the default site is working

:point_up:
Note that the default site provides example markdown files in the /docs and /website/blog folders. We will be using Netlify CMS

##### Configure Netlify CMS
4. Before we set up Netlify CMS, we need to have deployed the project to Github (or alternatively, where ever we intend to host the project). We can do running the Git and Github CLI commands from the root of the project as follows:
    1. Initialize the local git repo:`git init`
    2. Stage the new files to the git repo:`git add .`
    3. Update the local repo with the staged files: `git commit -m 'Initial commit'`
    4. Create a remote repo on github using Github CLI: `gh repo create [projectName]` (be sure to confirm adding the new repo as a remote)
    5. Update the remote repo with the staged files: `git push -u origin master`
5. According to [Netlify docs](https://www.netlifycms.org/docs/add-to-your-site/), the first step to configure Netlify CMS requires creating the necessary configuration files within an `admin` folder. We can do so by using the following commands
    1. `mkdir admin && cd admin`
    2. `touch index.html`
    3. `touch config.yml`
6. The `index.html` file will load the Netlify CMS application. We can do so by modifying `index.html` with the following code:
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
7. The `config.yml` file will provide the configuration settings used by Netlify CMS for editing the identified files. We can configure `config.yml` to use Github authentication and edit the files in the `docs` and `blog` folder by adding the following code:

:point_up:
Replace [github-username] with your github username
```
    backend:
      name: github
      branch: master # Branch to update (optional; defaults to master)
      repo: [github-username]/[projectName]

    # These lines should *not* be indented
    media_folder: "static/images/uploads" # Media files will be stored in the repo under static/images/uploads
    public_folder: "/images/uploads" # The src attribute for uploaded media will begin with /images/uploads

    collections:
    - name: blog
        label: "blog"
        folder: website/blog
        identifier_field: title
        extension: md
        widget: "list"
        create: true
        slug: "{{year}}-{{month}}-{{day}}-{{slug}}" # Filename template, e.g., YYYY-MM-DD-title.md
        fields:
          - { name: title, label: Title, widget: string }
          - { name: author, label: Label, widget: string }
          - { name: authorURL, label: "Author URL", widget: string }
          - { name: authorFBID, label: "Author FBID", widget: number }
          - { name: body, label: Body, widget: markdown }
```

:point_up:
Note that the 'blog' collection reflect is based on the default structure for the markdown files in the `blog` folder. More information on [widgets for Netlify](https://www.netlifycms.org/docs/widgets/)

8. Visit the `localhost:3000/admin` and authorize connection with Github profile
9. Once logged in, you will view the Netlify CMS admin section where you can view a list of the markdown files in the `docs` folder!
10. Select any of the files from the list and update any of the fields then click on the button `Publish now` in the upper right-hand corner. NetlifyCMS will update the remote repo identified in the `config.yml`
11. You can view these changes on your local repo by downloading the changes to the remote repo using the command `git pull`. Once completed, revisit `localhost:3000` in order to view changes

##### Deploy to Netlify
12. According to the official docs for [deploying to Netlify](https://docusaurus.io/docs/en/publishing#hosting-on-netlify), your website can be deployed to Netlify using the following steps:
    1. Login to Netlify
    2. Create a new site from Github by clicking on `New Site from Git`
    3. Select Github and your project's repository
    4. In the Build Settings, use the following:
        1. Choose the `master` branch
        2. Enter the build command as `cd website; npm install; npm run build;`
        3. Enter the publish directory as `website/build/[projectName]` 
        4. Click on `Deploy Site` button
![](https://i.imgur.com/O6Oek4T.png)
 
13. Now your site should be successfully deployed to Netlify :relieved: ! However, if you visit `/admin` at the Netlify generated domain, you may be faced with an error similar to "No authentication provider":sweat_smile: . You may choose to use Netlify Identity or a custom authentication provider however we will utilize Github for this tutorial.
14. You can add Github as an authentication provider using the following steps:
    1. Create a Github OAuth app at this [link](https://github.com/settings/applications/new)![](https://i.imgur.com/bNq3EmS.png)
    2. Visit the Netlify console to install a new OAuth provider via `Site Settings > Access control > OAuth > Authentication Providers > Install Provider`.
    3. Copy the Client ID and Client Secret for the Github OAuth app over to Netlify![](https://i.imgur.com/lQ5pXJq.png)
  
15. Reload the site at the deployed URL and then you will be able to access the Netlify console at the deployed URL just like you can on `http://localhost:3000`

### Test Locally
- Clone repo to local machine by running the command `git clone https://github.com/sajclarke/docusaurus-netlifycms.git`
- Install dependencies using `cd website && yarn`
- Deploy to localhost using `yarn start`

### Licence
MIT Licence [View LICENSE](LICENSE.md)
