## Jupyter Notebooks and Kyso
*Connect your Github repositories with Kyso to supercharge the way you communicate your data & analysis.*

***

![Octocat]["/images/octocat.png"]

Sharing and reproducing data science is still not easy — most work just sits on Github, undiscovered, as very technical documents. Most companies still do not have a central hub for knowledge. Data teams typically use tools like Github for project management, but this means their work is not shared with the non-technical people in the company. This also holds true for individual projects.

Kyso solves this problem with an elegant blogging platform, making accessible to everyone information that was previously only possessed by a select few. Kyso lets you blog and share your analyses. Think of it like Medium, but for data science — you can publish Jupyter notebooks, charts, code, datasets and even write articles. Any code is hidden by default and can be toggled so that your post is readable for both technical and non-technical audiences.

This guide is about sharing the results of your Jupyter notebooks with people who aren’t comfortable with code. You will learn how to integrate your Github repositories with Kyso to create an effortless computation-to-dissemination workflow.

### Getting Started

If you don’t have an account — head over to Kyso and sign up — it’s free! If you like you can sign up with your Github account, which will automatically sync with Kyso.

### Authorization

It’s very easy to connect Github to Kyso — all you need to do is navigate to the Github page by clicking New > Connect Github Repository in the top-right corner of the navigation bar.

![Octocat]["/images/github-auth.png"]

If you did sign up to Kyso with Github you will be automatically connected and you should see all your public repositories. If not, on our Github page, just click the Connect Github account button. Using the dropdown on the right you have the option to authorize all your Github repositories, or just your public ones (default). It's not possible to authorize just one repo at this time given the way Github’s OAuth authorization works.

![Octocat]["/images/github-connect.png"]

On the Github OAuth page, you will have the option to authorize your own account and any organizations you may be a part of.

You can search and filter your repositories, and import them to Kyso by clicking Connect to Kyso. You can then browse to your post on Kyso and choose whichever file you want to be shown to your visitors by default.
From then on whenever you push a commit to Github, the sister post on Kyso will be automatically updated.

### Configuration

You can optionally add a kyso.yaml file to your directory with the following options:
branch: this allows you to specify only a single branch which will be posted to Kyso.

- **main**: Specify the main file you want your visitors to see
- **title**: Specify the title of the post
- **description**: Specify the description of the post
- **tags**: any tags you want to be added to the post
- **email**: an email address that a Kyso team member used to sign up to Kyso and they will be attributed as the creator on Kyso. **This only applies to child posts.**
- **created_at**: to set the post date just add this field as a string that can be parsed by moment.js (https://momentjs.com/guides/#/parsing/). **This only applies to child posts.**
- **updated_at**: same as created_at but for when the post is updated. **This only applies to child posts.**

So, for example, if you had a directory with a my-article.ipynb notebook and you wanted Kyso to only accept the staging branch you would create a kyso.yaml file with the following:

```
title: "My awesome post"
description: "This is a description of what I did in my awesome post"
branch: staging
main: my-article.ipynb
tags:
  - apples
  - organges
```

You can also set the title and description, and preview image in the post configuration file on Kyso.

**Recommendation**: Always add a title, description and preview image to your posts. It makes your posts on Kyso a lot more readable and searchable.

If you want to validate your YAML file before pushing to Github, check out this [YAML Validator](http://www.yamllint.com/).

### Importing A Github Repository With Multiple Posts

Remember that, while a post on Kyso may look like a single rendered notebook, it is, in fact, a reflection of a complete Github repository (or a sub-directory of a Github repository), meaning you can browse all attached files on Kyso.

However, if your repository contains many different folders which would be better presented as different posts on Kyso, this is possible. There are various fields you can add to your kyso.yaml file to configure the behavior you like:

- **posts**: All you need to do is to add a posts field to the kyso.yaml file with a list of folders you want to include. Currently, the child posts must be organized in folders — importing different files from one folder as different child posts is not possible.

```
posts:
  - folder1
  - folder2
```

You can use wildcards — for example, if you want to include all folders in the root directory just use:

```
posts:
  - "*"
```

Or if you want to include all folders starting with “article” use:

```
posts:
  - "article*"
```

You must specify the posts field in order to import child posts.

- **main**: this is the default main file for all the included posts — you can overwrite this on a per-post basis (see below).
- **hideRoot**: (optional — the default is false) — use this if you don't want the root repository appearing as its own Kyso post. Usually, this is the README.md.

### Deleting A Repository With Child Posts

It is very important to remember that when you delete the parent repository all the child posts will be deleted too.

### Overriding Configuration Of The Child Posts

You can add a kyso.yaml to each child folder and override any of the defaults, the same as you do when you're importing just one repository.

### Troubleshooting

If your post is not appearing the way you expected — make sure that the YAML in the kyso.yaml file is correct — verify it at YAML Validator.

If you receive a verification error go to your repository on Github and navigate to the Settings > Webhooks page and if you see a webhook that points to api.kyso.io simply delete it and then re-import your repository to Kyso.

### Case Study

For a working example of how Kyso’s Github integration can work with your Jupyter notebooks, check out my Github repository for [various data analysis projects](https://github.com/KyleOS/Data-Analysis). You’ll see a lot of sub-directories there which I have set as posts on Kyso. Check out the main YAML file and then look at a YAML file in one of the folders, which overrides the defaults.

Finally, head over to my [Kyso profile](https://kyso.io/KyleOS) to view how these posts render beautifully. People can directly comment on any given post. If you have a pro account, you will be able to set these posts to private and add your friends & colleagues as collaborators.

***

## For Teams

Kyso’s Github integration can also be leveraged for internal private knowledge-sharing. In 2016, Airbnb open-sourced their [Knowledge Repository](https://github.com/airbnb/knowledge-repo), built to tackle their own needs of scaling internal knowledge sharing and management.

And we’ve made it super simple to transition from the current workflow using Airbnb’s knowledge repo to Kyso through our Github integration. You can even continue using the knowledge repo if you like and use Kyso to present and collaborate on your posts among your team.

This works the exact same way as connecting a Github repository as explained above. Your team’s knowledge repo will show up among your list of Github repositories just like any other repository on Github. Select it to connect. You can then *View on Kyso*. You will see that we automatically create a main post of the knowledge repo itself.

**Important**: We identify the knowledge repo by the existence of a `.knowledge_repo_config.yml` file in the knowledge repo itself, so it is important that you do not delete this file. We then read in the knowledge.md file as the main file to render on the main post. You can browse through the files and select different files to render.

Each .kp directory within the knowledge repo is published to Kyso as a separate post. Again, the knowledge.md file created from the source file will be rendered as the main file on Kyso, and all other files within the specific .kp folder are attached. Unlike when connecting a regular Github repository, there is no need to specify a main file to render in a *kyso.yaml* file.

**Important**: As mentioned in the previous section, if you delete the main knowledge repository then all the child posts synced to Kyso (i.e. the .kp folders) will be deleted too.

For your workflow, any commits you push to your internal Knowledge Repository on Github will be automatically reflected on Kyso.
Read about why we started Kyso for Teams in a previous post [here](https://medium.com/@kyle_oshea/from-airbnbs-knowledge-repo-to-kyso-for-teams-735ce8c7eec0).