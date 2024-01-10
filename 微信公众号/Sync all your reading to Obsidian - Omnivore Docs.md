---
id: e5f681ae-55ff-40e2-bb9c-803ef95a4ae0
---

# Sync all your reading to Obsidian | Omnivore Docs
#Omnivore

[Read on Omnivore](https://omnivore.app/me/sync-all-your-reading-to-obsidian-omnivore-docs-18cf14213a8)
[Read Original](https://docs.omnivore.app/integrations/obsidian.html)


Omninove integrates with Obsidian via the [obsidian-omnivore](https://github.com/omnivore-app/obsidian-omnivore) plugin. The plugin allows for customizing the data synced from Omnivore to Obsidian and controlling the layout of the data.

* [Features](#features)
* [Installation](#installation)
* [Customizing which data is synced from Omnivore to Obsidian](#customizing-which-data-is-synced-from-omnivore-to-obsidian)
* [Controlling the layout of the data imported to Obsidian](#controlling-the-layout-of-the-data-imported-to-obsidian)  
   * [Mustache template language](#mustache-template-language)  
   * [Variables available in the article template](#variables-available-in-the-template)  
   * [Importing the Full Article Content](#importing-the-full-article-content)  
   * [Function map in the article template](#function-map-in-the-article-template)  
   * [Front Matter](#front-matter)  
   * [Front Matter Template](#front-matter-template)
* [Sync all the articles into a single note](#sync-all-the-articles-into-a-single-note)
* [Filename](#filename)
* [Scheduled Sync](#scheduled-sync)

## Features 

* Import your highlights and saved articles
* Create graphs based on Omnivore data
* Filter imported data using Omnivore's [advanced search syntax](https://omnivore.app/help/search)
* Custom templates for imported data

## Installation 

1. Install and enable the plugin from the Community Plugins section of Obsidian or [build it from source](https://github.com/omnivore-app/obsidian-omnivore) and load the unpacked plugin.
2. Sign up for an [Omnivore account](https://omnivore.app/).
3. Go to [Omnivore](https://omnivore.app/settings/api) and create an API key.
4. Open the Omnivore plugin settings in Obsidian and add your API key.
5. Run Omnivore sync by opening the command palette (Ctrl + P or CMD + P). Type Omnivore and select the option **Omnivore: Sync**.

## Customizing which data is synced from Omnivore to Obsidian 

[Obsidian Sync Filter](https://docs.omnivore.app/integrations/images/obsidian-sync-filter.png)

The Omnivore Obsidian plugin offers three options for which data is imported:

* Import all my articles: This will import every article you save to Omnivore and the labels (tags), highlights, and notes you have added to the article.
* Import just highlights: This will import every article you have saved that has highlights
* Advanced: This allows you to define a search filter to import. For example, import just Newsletters by using `label:Newsletter`.

## Controlling the layout of the data imported to Obsidian 

The Omnivore Obsidian plugin uses the [mustache](https://mustache.github.io/) template language to define how imported data is saved to Obsidian. The plugin settings allow you to define the template for an article layout. The default templates will be used if you do not define your own.

### Mustache template language 

Mustache is a simple and logic-less templating language that allows you to create templates that are easy to read and maintain. Mustache templates consist of tags, which are placeholders that are replaced with actual values when the template is rendered.

There are several types of Mustache tags, including:

* `{{ variable }}`: Renders the value of a variable.
* `{{# section }}` ... `{{/ section }}`: Renders a section of the template if the value of the section is true, such as an array or an object.
* `{{^ inverted section }}` ... `{{/ inverted section }}`: Renders a section of the template if the value of the section is false.
* `{{! comment }}`: Adds a comment to the template, which is ignored when the template is rendered.

In addition to the basic Mustache tags, there are also several built-in Mustache functions that you can use in your templates, such as:

* `{{# list }}` ... `{{/ list }}`: Renders a section of the template for each item in a list.
* `{{^ emptyList }}` ... `{{/ emptyList }}`: Renders a section of the template if a list is not empty.
* `{{& variable }}` or `{{{ variable }}}`: Renders the value of a variable without escaping HTML characters.
* `{{> partial }}`: Renders a partial template, which is a reusable template that can be included in other templates.

### Variables available in the article template 

* id: The id of the article. This will be used as the id of the file in frontmatter
* title: The title of the article
* omnivoreUrl: The URL of the article in Omnivore
* siteName: The name of the site the article was saved from
* originalUrl: The URL of the original article. If the article was created from an emailed binary (eg PDF) attachment, this URL will reference the attachment and will not be accessible.
* author: The author of the article
* dateSaved: The date the article was saved to Omnivore in your perferrred date format
* description: The description of the article
* content: The content of the article in Markdown format. This may be empty if the article was created from an emailed binary (eg PDF) attachment.
* labels: A list of labels attached to the page  
   * name: The name of the label, for example `Newsletter`
* note: The note attached to the article
* type: The type of page, for example `ARTICLE` or `FILE`
* datePublished: The date the article was published in your perferrred date format
* dateRead: The date the article was read in your perferrred date format
* dateArchived: The date the article was archived in your perferrred date format
* fileAttachment: The file attachment attached to the article
* state: The state of the article, for example `READING` or `COMPLETED`
* wordsCount: The number of words in the article
* readLength: The length of time it took to read the article in minutes
* highlights: A list of highlights attached to the article  
   * text: The text of the highlight  
   * highlightUrl: The URL of the highlight in Omnivore  
   * labels: A list of labels attached to the highlight  
         * name: The name of the label, for example `Newsletter`  
   * note: The note attached to the highlight  
   * dateHighlighted: The date the highlight was created in your perferrred date format  
   * highlightID: The uuid of the highlight  
   * color: The color of the highlight, for example `yellow`  
   * positionPercent: The position of the highlight in the article as a percentage  
   * positionAnchorIndex: The position of the highlight in the article as an index/page number

Default template:

```markdown
# {{{title}}}
#Omnivore

[Read on Omnivore]({{{omnivoreUrl}}})
[Read Original]({{{originalUrl}}})

{{#highlights.length}}
## Highlights

{{#highlights}}
> {{{text}}} [⤴️]({{{highlightUrl}}}) {{#labels}} #{{name}} {{/labels}}
{{#note}}

{{{note}}}
{{/note}}

{{/highlights}}
{{/highlights.length}}

```

### Importing the Full Article Content 

Full content import does not work for PDFs

The content of articles is converted to Markdown before import. This means we need them to avoid HTML escaping when being inserted into Obsidian. To do this, use the triple Mustache syntax. Any highlights that you have created in Omnivore will be rendered as Markdown highlights. So to import the full article content:

For example:

```yaml
---
id: {{{id}}}
title: >
  {{{title}}}
{{#author}}
author: >
  {{{author}}}
{{/author}}
{{#labels.length}}
tags:
{{#labels}} - {{{name}}}
{{/labels}}
{{/labels.length}}
date_saved: {{{dateSaved}}}
{{#datePublished}}
date_published: {{{datePublished}}}
{{/datePublished}}
---

# {{{title}}}
#Omnivore

[Read on Omnivore]({{{omnivoreUrl}}})
[Read Original]({{{originalUrl}}})

{{{ content }}}


```

Simplified template with full content and file attachment, if it there is one.

> Note if the article was created via an emailed binary (eg PDF) attachment, `content` will be empty.

```yaml
# {{{title}}}

{{{content}}}

{{#fileAttachment}}
---

![[{{{fileAttachment}}}]]
{{/fileAttachment}}

```

### Function map in the article template 

* lowerCase: Converts a string to lowercase
* upperCase: Converts a string to uppercase
* upperCaseFirst: Converts the first character of a string to uppercase
* formatDate: Converts a date to a different format. For example, `{{#formatDate}}{{dateSaved}},"yyyy-MM-dd"{{/formatDate}}` will convert the date to .

Example of using a function in a template to convert the state of an article to lowerCase:

```pf
state:: [[{{#lowerCase}}{{state}}{{/lowerCase}}]]

```

### Front Matter 

[While most of the text in a note is meant to be read by a human, metadata is text that's meant to be easily readable by a program, for example a community plugin or Obsidian itself.](https://help.obsidian.md/Editing+and+formatting/Metadata) The front matter includes the metadata at the top of the note in YAML format. You can enter the metadata separated by comma and you can also use custom aliases in the format of `metadata::alias`. For example, `date_saved::date` will generate in the front matter.

The available metadata are in snake case:

* title
* author
* tags
* date\_saved
* date\_published
* omnivore\_url
* site\_name
* original\_url
* description
* note
* type
* date\_read
* words\_count
* read\_length
* state
* date\_archived

The default metadata are `title, author, tags, date_saved, date_published`.

Please note that `id` will always be included in the front matter for the purpose of deduplication.

### Front Matter Template 

The front matter template is used to override the front matter of the file. It will only be used when it is set. If it is not set, the front matter will be generated automatically based on the metadata defined in "Front Matter" section under basic settings.

The available variables are the same as the [variables in the article template](#variables-available-in-the-template).

A sample front matter template:

```handlebars
id: {{{id}}}
title: >
  {{{title}}}
{{#author}}
author: >
  {{{author}}}
{{/author}}
{{#labels.length}}
tags:
{{#labels}} - {{{name}}}
{{/labels}}
{{/labels.length}}
date_saved: {{{dateSaved}}}
{{#datePublished}}
date_published: {{{datePublished}}}
{{/datePublished}}

```

## Sync all the articles into a single note 

By default, Omnivore will sync articles into separate pages in Obsidian. If you would like to sync all your articles into a single note, you can turn on the option **Is Single File** and use a fixed **Filename** in the plugin settings.

## Filename 

Enter a filename for the article. You can either use a fixed filename or a template. If you use a template, the filename will be generated based on the template. For example, if you enter `{{{ title }}}`, the filename will be the title of the article.

Available variables for the template are:

* id
* title
* author
* dateSaved
* datePublished

## Scheduled Sync 

By default, Omnivore does not automatically sync your data. You can enable scheduled sync by entering a number of minutes in the **Frequency** setting. For example, if you enter `60`, Omnivore will sync your data every hour and you can always turn it off by entering `0`.