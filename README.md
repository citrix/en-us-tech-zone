---
layout: doc
description: Documentation for Citrix Tech Zone contributors
---
# Citrix Tech Zone readme

## Welcome to Tech Zone

Welcome to the documentation project for [techzone.citrix.com](https://techzone.citrix.come)! Citrix Tech Zone is home for technical, in-depth articles that are inspired and driven by technical communities and enthusiasts. Whether you are an architect, consultant, engineer, or technical IT manager, you have come to the right place!

Tech Zone has always been oriented towards technical community, but now you can help us to make it better by suggesting changes or even writing the whole articles to share with fellow Citrix fans.

## How To

### Contact Tech Zone administrators

Preferred method of contacting Citrix Tech Zone team is through one of the dedicated Slack channels. If you are not a Citrix employee or member of NDA communities (CTP/CTA program), you can use the Slack channel at [World of EUC](https://worldofeuc.slack.com/messages/CLKSAPFUG) workspace. For citrites, use channel `#tm-techzone`.

If you prefer to use email, feel free to reach out to us using [email](mailto:Tech-Content-Feedback@citrix.com).

### 1 - Create an account

There are two different methods how you can become one of the contributors, depending on whether or not you are a Citrix employee.

#### Citrix employees

1.  Create your GitHub account with two-factor authentication enabled
2.  Accept contributors agreement: [https://docs.citrix.com/en-us/settings.html](https://docs.citrix.com/en-us/settings.html). **This is a required step!**
3.  Send us your GitHub user name (see [Contact Tech Zone administrators](#contact-tech-zone-administrators) section)
4.  Wait for validation email – you have to accept this invitation to become one of the Tech Zone contributors
5.  Make sure that correct name and email address is configured in your GitHub Desktop (File -> Options -> Git)

#### Everybody else

Follow this [simple guide](https://citrix.github.io/tech-marketing/projects/tech-zone/crowdsourcing.html) to become one of our contributors.

### 2 - Request a branch

To make any changes to Tech Zone, you have to use a dedicated branch (think about it as an isolated clone of Tech Zone only to be used for your new article or update of an existing article). Another approach is to use you own fork of Tech Zone and just create a pull request, but this approach is recommended only for advanced GitHub users. If you are familiar with private forks and concept of pull requests, you can use this method.

1.  Use one of the dedicated Slack channels to request new branch. Specify section of Tech Zone (e.g. Tech Insight) and title of the article (e.g. "Workspace app").
2.  Wait for link to your new branch. You get it through Slack or by email from Tech Zone team.
3.  When you click on link, GitHub web editor opens with article you are working on. Read [Visual Studio Code (VSC) guide](https://citrix.github.io/tech-marketing/projects/tech-zone/visual-studio-code-guide.html) to learn how to create a local copy of this branch for editing.
4.  After you are done with your article, make sure to update category section with a link to your article (e.g. tech-insights.md file) and [upload images](#adding-images).

### 3 - Create content

As many other large companies (for example Microsoft or Amazon), we are using de-facto industry standard language called markdown to create Tech Zone content. Markdown language is a simple, lightweight language that is easy to learn and allows us to create dynamic content. Think about markdown as a text-to-HTML conversion tool. It is highly recommend to read the [most common mistakes](#most-common-mistakes) section first - there is a learning curve and this short list can greatly help you to quickly learn this language.

As a markdown editor, we recommend to use [Visual Studio Code (VSC)](https://citrix.github.io/tech-marketing/projects/tech-zone/visual-studio-code-guide.html). Don't be discouraged by Visual Studio in name - this tool is not only for developers, but useful as a general text editor.

#### Writing text

Tech Zone is using standardized markdown language with only one exception - when you are creating a list, you should use two spaces (instead of typical single space) after asterisk (unordered list) or number (ordered list).

To learn more about markdown syntax, you can read many articles online and read some of the (many) available [cheatsheets](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

Few additional recommendations:

*  Keep your sentences short--26 words or fewer
*  Write in a personal, friendly style. Do not use the style of academic discourse. Avoid using unusual words if there is a simpler variant
*  Write in present tense. For example, "the product reports the result" instead of "the product will report the result."

#### Adding images

Our readers love images, so include as many as you need to get your message across. All images must be placed in the **/media** folder of the Tech Zone repo and you can upload them through GitHub website.

**IMPORTANT:** Make sure that you are using the right branch AND right naming conventions when uploaded files. GitHub will in some cases switch back to **master** branch, double-check that your branch is still selected before uploading any images.

To add images to your article, you need to do following:

1.  Prepare images using the right naming conventions (instructions below). This is important for our automation
2.  Upload images to section /media folder (this is under learn/design/build folder). Make sure you are using the [correct branch](#using-the-right-fork--branch)
3.  Reference those images in your markdown code (instructions below)

Image conventions:

*  Use PNG format for images
*  Use basic grayscale colors and icons / diagrams from brand team to create diagrams. Look at other Tech Zone content to follow same style and format
*  When your image has white background (e.g. screenshots from Citrix Cloud), add a 1px black frame around that picture. White on white does not look good.
*  File name is using format `(category-name)_(article-name)_(image-name)`, where `category-name` and `article-name` are based on URL of the target article. For example tech-briefs_workspace-app_overview.png. “Category” is identical to category section of URL (e.g. ‘’reference-architectures”) and “article name” is identical to file name of the article (.md or .html file). `image-name` can be any format that you prefer, however it is recommended to use descriptive names.
*  Underscore character (`_`) is used as delimiter and you should not use it in image name. Feel free to use hyphen in image name instead. Each image file name should contain exactly 2 underscore characters.
*  For diagrams, try to include sources (Visio or PowerPoint format) for your diagrams.

When you are referencing your images in markdown language, you can use two different syntaxes.

For simple images (screenshots), use format `![descriptive text](image URL)` with relative URL, for example `/en-us/tech-zone/design/media/design-decisions_provisioning-solutions_diagram-explicit.png`.

For images that should open full screen after clicking them, use format `[![descriptive text](image URL)](image URL)`. This will open image after reader clicks on it. This is typically recommended for images such as diagrams.

#### Using tables

Tables are discouraged for modern articles - it is complicated to maintain them in markdown, they have limited functionality (e.g. markdown doesn't support bullet points in tables or multiple paragraphs) and articles with tables are penalized by search engines like Google or Bing (content is marked as not being mobile friendly). If you really need to include a table in your article, use one of the many online generators (for example [this one](https://www.tablesgenerator.com/markdown_tables)).

Generally - if you need to use multiple paragraphs of text, bullet points inside tables or multiple nested tables, you should consider to break down table to use native markdown format. If each cell of your table contains only one word or one sentence, table is perfectly valid option.

### 4 - Review content

When you are done with your article, contact one of the administrators (Slack, email or other communication channel) and request your changes to be reviewed and merged. Always provide a name of your branch (or URL).

Content review is done in following steps:

1.  **Technical review** - content review involves inviting subject matter experts to help you with technical validation of the content. It is recommended to involve multiple people in all stages (from outline creation to final version) and get content reviewed as often as possible. Technical review is typically responsibility of content creator, but you can reach out to Tech Zone team to help find the right reviewers.
2.  **Styling review** - there are several rounds of review, from review of the markdown syntax (markdownlint) to grammar and styling (Acrolinx). Contributor initiates this phase by contacting Tech Zone team and requesting a styling review by providing name of the branch to review. If there are any problems with your article, TZ team contacts you with a list of problems and/or suggestions. After you have fixed them, request another round of review (it is common to have multiple rounds).

    **Note:** You can run markdown validation on your own if you are using Visual Studio Code as an editor, however Acrolinx licenses are limited.

### 5 - Publish content

After you are done with review stage, contact TZ team and ask them to publish your new article.

Make sure that you've also updated a category markdown file (e.g. reference-architectures.md) with a link to your newly created article and brief description of content. Use the same description in `description:` section of your article. This is third line from top with placeholder text `Copy & paste description from TOC here`.

1.  Staging environment - after styling review is finished (no markdown violations and Acrolinx score of at least 80 points), your article is published in staging environment to validate if all rendering works as expected. You receive a link to staging environment from TZ team for final review.
2.  Production environment - after successful review in staging environment, article is released to production.
3.  RSS feed is automatically updated (usually on the same day as article is released, but sometimes we wait for 24 hours).

## Most Common Mistakes

For contributors, basic understanding of markdown language and versioning (GitHub) is required. When working on Tech Zone content, we have seen few issues that are frequently occurring and we would like to cover them in this section.

### Using the right fork / branch

Versioning systems (like Git or Subversion) are useful for professional developers (especially for large and complicated projects), however they can be confusing for non-developers. While learning about versioning systems, pull requests and protected branches is definitely worth your time, you should follow few simple rules when working on Tech Zone content if you are not 100% comfortable with these advanced concepts:

*  For any change to Tech Zone, you need a branch - this is required so that you don't impact other contributors. If you don't have a branch, contact one of the administrators
*  Make sure you are using "martinzugec-ctx" fork (not your personal or Citrix fork)
*  Never try to make any changes to `wip` or `master` branches (protected branches)

**To fix:** Always confirm that you are using the right branch before making any changes.

![Finding Branch](/_readme/readme_branch.png)

### Trailing spaces

Markdown does not allow you to end line with an unexpected whitespace (space after final punctuation mark).

**To fix:** Remove the trailing punctuation.

[More information](https://github.com/DavidAnson/markdownlint/blob/v0.17.0/doc/Rules.md#md009)

### Multiple empty lines

Markdown does not allow you to use multiple consecutive blank lines in the document. This happens often at the end of the article.

**To fix:** Remove an extra empty line.

[More information](https://github.com/DavidAnson/markdownlint/blob/v0.17.0/doc/Rules.md#md012)

### No empty lines around headers / lists

Both headers and lists should have an empty line **before** and **after** them.

**To fix:** Add an empty line before and after any header and/or list.

[More information - headers](https://github.com/DavidAnson/markdownlint/blob/v0.17.0/doc/Rules.md#md022)
[More information - lists](https://github.com/DavidAnson/markdownlint/blob/v0.17.0/doc/Rules.md#md032)

### Hard tabs

Markdown requires usage of spaces for indentation instead of tabs.

**To fix:** Replace your hard tabs with spaces instead.

[More information](https://github.com/DavidAnson/markdownlint/blob/v0.17.0/doc/Rules.md#md010)

### Spaces after list markers

Standard markdown syntax requires one space after list character. Tech Zone implementation requires 2 spaces after list character.

    - One Space - NOT correct
    -  Two spaces - correct

### Absolute links to docs.citrix.com

Normal format of links in markdown language is `[text](https://url.com)`. However, when referencing content hosted on docs.citrix.com, you should use relative links only, starting with `/en-us/`. For example if you want to reference `https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service.html`, you should use syntax `[Citrix Virtual Apps and Desktops service](/en-us/citrix-virtual-apps-desktops-service.html)`. This is important to support other languages - relative links are automatically converted to localized language if page is available.
