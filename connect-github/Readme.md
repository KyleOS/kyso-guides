# Connecting a Github repo to Kyso

In this guide we will go through how to add a Github repo to Kyso so that every commit you make to Github will be reflected on Kyso.

It's very easy to add connect Github to Kyso - all you need to do is navigate to the Github page by clicking New > Connect Github Repository from the New button on the top right.

If you've signed up to Kyso with Github you will already be connected and you should see all your repos.
If not just click the Connect Github account button. Using the dropdown on the right you have the option to authorize all your Github repositories, or just your public ones (default). Its not possible to authorize just one repo at this time given the way Github's OAuth authorisation works.

You can search and filter your repositories, and import them to Kyso by clicking Connect to Kyso. You can then browse to your post on Kyso and choose whichever file you want to be shown to your visitors by default.

From then on whenever you push a commit to Github it will update the Kyso post.

### Extra Configuration

You can optionally add a .kyso.yaml file to your directory with the following options:

- **branch**: this allows you to specify only a single branch which will be posted to Kyso.
- **main**: Specify the main file you want your visitors to see
- **title**: Specify the title of the post
- **description**: Specify the description of the post
- **tags**: any tags you want added to the post
- **email**:  an email address that a Kyso team member used to sign up to Kyso and they will be attributed as the creator on Kyso. __This only applies to child posts__.
- **created_at**: to set the post date just add this field as a string that can be parse by moment.js https://momentjs.com/guides/#/parsing/ __This only applies to child posts__.
- **updated_at**: same as created_at but for when the post is updated. __This only applies to child posts__.

So for example if you had a directory with a 'my-article.ipynb' notebook and you wanted Kyso to only accept the staging branch you would create a .kyso.yaml file like the following:

```
title: "My awesome post"
description: "This is a description of what I did in my awesome post"
branch: staging
main: my-article.ipynb
tags:
  - apples
  - organges
```

You can also set the title and description, and preview image in the post config on Kyso.

**Recommendation:** Always add a title, description and preview image to your posts. You will
get a ton more readers.

If you want to validate your yaml before pushing to Github - checkout this [YAML Validator](http://www.yamllint.com/)

### Importing one Repository with many posts

If you repository contains many different folders which would be better presented
as different posts on Kyso you can do that. There are various fields you can add
to your .kyso.yaml to configure the behaviour you like

- **Posts**:  All you need is to add a 'posts' field
  to the .kyso.yaml with a list of folders you want to include

  ```
  posts:
    - folder1
    - folder2
  ```

  You can use wildcards, so for example, if you wanted to include all folders in the root directory just use:

  ```
  posts:
   - "*"
  ```

  Or if you wanted to include all folders starting with "article" use:

  ```
  posts:
    - "article*"
  ```

- **main**: this is the default main file for all the include posts - you can overwrite this on a per post basis (see below).
- **hideRoot**: (optional - default false) use this if you dont want the root repository appearing as its own Kyso post.

#### Overriding config in the child posts

You can add a .kyso.yaml to each child folder and override any of the defaults the same as you do when your importing just one repo.

## Troubleshotting

If you get a 'verification error' go to your repository on Github and navigate to the Settings > Webhooks page
and if you see a webhook that points to 'api.kyso.io' simply delete it and then re import your repository to Kyso.