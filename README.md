# Citrix Tech Zone readme

## Welcome to Tech Zone

Welcome to the documentation project for [techzone.citrix.com](https://techzone.citrix.come)! Citrix Tech Zone is home for technical, in-depth articles that are inspired and driven by technical communities and enthusiasts. Whether you are an architect, consultant, engineer or technical IT manager, you have come to the right place!

Tech Zone has always been oriented towards technical community, but now you can help us to make it better by suggesting changes or even writing the whole articles to share with fellow Citrix fans.

## Welcome to Markdown language

As many other large companies (including Microsoft or Amazon), we are using de-facto industry standard language called markdown to create Tech Zone content. Markdown is a very simple, lightweight language that is easy to learn and allows us to create very dynamic content. Think about markdown as a text-to-HTML conversion tool. It based on easy-to-read and easy-to-write plain text syntax.

To learn more about markdown syntax, you can read many articles online and read some of the (many) available [cheatsheets](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

## How To

### Create an account

TODO

### Request a branch

TODO

### Create content

TODO

#### Adding images

Our readers love images, so include as many as you need to get your message across. All images must be placed in the /media folder of the Tech Zone repo and you can upload them through GitHub website.

**IMPORTANT:** Make sure that you are using the right branch AND right naming conventions when uploaded files.

Image conventions:

*  Use PNG format for images
*  Use basic grayscale colors and icons / diagrams from brand team to create diagrams. Look at other Tech Zone content to follow same style and format
*  File name is using format `(category-name)_(article-name)_(image-name)`, where `category-name` and `article-name` are based on URL of the target article. For example tech-briefs_workspace-app_overview.png. “Category” is identical to category section of URL (e.g. ‘’reference-architectures”) and “article name” is identical to filename of the article (.md or .html file). `image-name` can be any format that you prefer, however it is recommended to use descriptive names.
*  Underscore character (`_`) is used as delimiter and you should not use it in image name. Feel free to use hyphen in image name instead. Each image file name should contain exactly 2 underscore characters.
*  For diagrams, try to include sources (Visio or PowerPoint format) for your diagrams.

When you are referencing your images in markdown language, you can use two different syntaxes.

For simple images (screenshots), use format `![descriptive text](image URL)` with relative URL, for example `/en-us/tech-zone/design/media/design-decisions_provisioning-solutions_diagram-explicit.png`.

For images that should open full screen after clicking on them, use format `[![descriptive text](image URL)](image URL)`. This will open image after reader clicks on it.

#### Using tables

Tables are generally discouraged for modern articles - it is complicated to maintain them in markdown, they have very limited functionality (e.g. markdown doesn't support bulletpoints in tables or multiple paragraphs) and articles with tables are penalized by search engines like Google or Bing (content is marked as not being mobile friendly). If you really need to include a table in your article, please use one of the many online generators (for example [this one](https://www.tablesgenerator.com/markdown_tables)).

As a general rule - if you need to use multiple paragraphs of text, bulletpoints inside tables or multiple nested tables, you should consider to break down table to use native markdown format. If each cell of your table contains only one word or one sentence, table is perfectly valid option.

### Publish content

When you are done with your article, contact one of the administrators (Slack, email or other communication channel) and request your changes to be reviewed and merged. Always provide a name of your branch (or URL).

Publishing content is done in following steps:

1  Content review - content review involves inviting subject matter experts to help you with technical validation of the content. It is strongly recommended to involve multiple people in all stages (from outline creation to final version) and get content reviewed as often as possible. Technical review is typically responsibility of content creator.
1  Styling review - there are several rounds of review, from review of the markdown syntax (markdownlint) to grammar and styling (Acrolinx). If your article has any issues, administrator will contact you with list of problems and/or suggestions. You need to contact your administrator to initiate a review stage by providing name or URL of your branch. Administrator is responsible for generating report, however content creator is responsible for fixing issues that have been found.
1  Staging environment - staging environment is used to validate if all rendering works as expected. You will receive a link to staging environment from administrator for final review.
1  Production environment - after successful review in staging environment, article is release to production.

## Most Common Mistakes

For contributors, basic understanding of markdown language and versioning (GitHub) is required. When working on Tech Zone content, we have seen few issues that are frequently occuring and we would like to cover them in this section.

### Using the right fork / branch

Versioning systems (like Git or Subversion) are extremely useful for professional developers (especially for large and complicated projects), however they can be very confusing for non-developers. While learning about versioning systems, pull requests and protected branches is definitely worth your time, you should follow few simple rules when working on Tech Zone content if you are not 100% comfortable with these advanced concepts:

*  For any change to Tech Zone, you'll need a branch - this is required so that you don't impact other contributors. If you don't have a branch, contact one of the administrators
*  Make sure you are using "martinzugec-ctx" fork (not your personal or Citrix fork)
*  Never try to make any changes to `wip` or `master` branches (protected branches)

**To fix:** Always confirm that you are using the right branch before making any changes.

![Finding Branch](/_readme/readme_branch.png)

### Trailing spaces

Markdown does not allow you to end line with an unexpected whitespace (space after final punctuation mark).

**To fix:** Remove the trailing punctuation.

[More information](https://github.com/DavidAnson/markdownlint/blob/v0.17.0/doc/Rules.md#md009)

### Multiple empty lines

Markdown does not allow you to use multiple consecurive blank lines in the document. This happens very often at the end of the article.

**To fix:** Remove an extra empty line.

[More information](https://github.com/DavidAnson/markdownlint/blob/v0.17.0/doc/Rules.md#md012)

### No empty lines around headers / lists

Both headers and lists should have an empty line **before** and **after** them.

**To fix:** Add empty line before and after any header and/or list.

[More information - headers](https://github.com/DavidAnson/markdownlint/blob/v0.17.0/doc/Rules.md#md022)
[More information - lists](https://github.com/DavidAnson/markdownlint/blob/v0.17.0/doc/Rules.md#md032)

### Hard tabs

Markdown requires usage of spaces for indetation instead of tabs.

**To fix:** Replace your hard tabs with spaces instead.

[More information](https://github.com/DavidAnson/markdownlint/blob/v0.17.0/doc/Rules.md#md010)

### Spaces after list markers

Standard markdown syntax requires one space after list character. Tech Zone implementation requires 2 spaces after list character.

    - One Space - NOT correct
    -  Two spaces - correct
