<h1>MULTILINGUAL APPROACH</h1>

To handle multilingualism in Moodle requires a specific approach as all the languages are contained in the same container (page, dropdowns...).

[TOC]

# Available languages

## Language packs

Find **Language packs** in Site Administration and import the language(s). Install the French langugage.

## Default language

Open the *Administration / Language* category to adjust the default language (English or French, as wished).

# Multilingual filter

## Setup the filter

To enable multilingual dropdowns and labels: Find **Manage filters** in Site Administration and set **Multi-Language Content** filter on and choose **Content and Headings** option.

## Add the languages to the content

In fields definitions, dropdown options, etc... indicate the different languages with a `<span>` as below:

```html
<span class="multilang" lang="en">Other</span><span class="multilang" lang="fr">Autre</span>
```

> This technique is used for the **custom profile fields**, the **front page**....

Attention: this may slow down the site as every single page is scanned for the language tag.

# Language menu

To avoid displaying English and French in the language menu and replace it by a shorter version (EN, FR).

> Note: I couldn't find how to show flags.

Search **langlist** in Site administration and put these values (with Moove plugin, I choose not to use it):

```
en|EN,fr|FR
```

