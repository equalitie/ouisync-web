# Contributing
The website is hosted by GitHub Pages and most content is authored in Markdown format. HTML is also acceptable.

## Project Structure
The project is primarily split into two types of documents:
- Complex
- Basic

**Complex** documents are content that require custom layouts and importantly render translations using individual, manually created, i18n strings found in the `i18n/` dir.

The complex pages for the Ouisync website are:
- `layouts/index.html`
- `content/en/about.md`
- `content/en/community.md`
- `content/en/support.md`

**Basic** documents are content that simply requires simple markdown rich text styling. This content is written entirely in a single markdown file, which Weblate ingests and automatically parses into individual i18n strings. Any page not a complex page is a basic page, and can just be styled using markdown.

## Add a New Page

### Local Development with Hugo

To easily add a new page, you can use the `hugo new` command. For example, lets say we want to add a new page at `https://ouisync.net/mesh`, we can run the command:

```hugo new mesh.md```

This creates a `mesh.md` inside the `content/en/` path. Now that we have a new blank markdown file, we can write an entire article in markdown format and hugo will automatically push these changes to the website when the changes are merged into the `main` branch of the git repo.

To learn how to ready articles for translation, please read the "Translations" section below.

### Via Github Interface

It is also possible to add content with just a Github account. To start visit the `content/en/` directory in this repo. Then click on "Add File" -> "Create New File" 

You are now free to write! If you want to create a recurring set of posts, you can group markdown files in directories, and this will automatically create [Hugo List Pages](https://gohugo.io/templates/lists/). So for example if you create `content/en/mesh/mesh-post-1.md` and `content/en/mesh/mesh-post-2.md`, there will now be a page at `https://ouisync.net/mesh` which will list `mesh-post-1` and `mesh-post-2`.

## Edit an Exisiting Page

When editing content on a hugo site, there are a few different directories that are important. They represent the "content directory" these content directories are the input values that hugo uses to generate the output found in the `public/` directory.

- `layouts/index.html`: the homepage
- `layouts/partials/`: this directory holds HTML fragments which can be called from other partials, the homepage, or, by using a custom shortcode, from markdown files.
- `content/en/`: in our english-centric hugo project, this is the base content directory where markdown files can be placed. Placing a translated copy of a markdown file with the same name, in a seperate `content/` folder with a language code will automatically add the translation to the hugo site. For example if we have an english markdown file: `content/en/example.md`. We can translate the file to French and place it in `content/fr/english.md`. Hugo will automatically recognize this as a French translation for `example.md`.

Images can be added to the ```static/img/``` directory.

Icons can be added to the ```static/icons/``` directory.

## Translations

This repository is linked to a set of components in Weblate to sync translations: https://hosted.weblate.org/projects/ouisync/

### Translation Data Flow

Translation data is pulled from two seperate content sources inside of the hugo project: `i18n/` and `content/`.

#### i18n Directory

The `i18n/` directory contains `.json` files which hold key-value pairs. These key-value pair mappings are pulled from [Weblate](https://weblate.com) and they map a string `key` to a string containing the translated value of the original `en.json` file. These strings can be used inside *both* `layouts/` files (except in the `shortcodes` subdirectory because shortcodes cannot use Page variables) and also direct inside markdown documents in the `content/` directory.

##### Toy Demo

To better explain how to use these translation tools, here is a small example demonstrating the workflow.

Here is the base `en.json` file for this example: 

```json
{
    "title": "Hello",
    "intro": "My name is Alice",
}
```

And here would be the french translations pulled down from Weblate:

```json
{
    "title": "Bonjour",
    "intro": "Je m’appelle Alice",
}
```

Now we are able to access these strings in `layouts/index.html` and any partial layout in `layouts/partials/`:

```html
<h1>{{ i18n "title" }}</h1>
<p>{{ i18n "intro" }}</p>
```

For markdown we use the custom `render-i18n` shortcode, lets say this file is in `content/en/hello.md`:

```
# {{% render-i18n "title" %}}

{{% render-i18n "intro" %}}
```
## Translating Full Markdown Documents with Weblate

Weblate allows for the translation of entire markdown files, so for long-form content like Blog posts, we can let Weblate do the heavy lifting for us. For example, we can have an english markdown file with the following data:

```markdown
Once when I was six years old I saw a magnificent picture in a book, called True Stories from Nature, about the primeval forest. It was a picture of a boa constrictor in the act of swallowing an animal. Here is a copy of the drawing. 
```

If we place this content in a newly create file at the following path: `content/en/petite-prince.md`, we can then pull this information directly into Weblate by creating a new translation component with the following settings:

key | value
--- | ---
|Source code repository | https://github.com/equalitie/ouisync-web |
|Repository branch | main |
|File mask 	| content/*/petite-prince.md |
|Monolingual base language file | content/en/petite-prince.md |

Using this method, Weblate will automatically parse `content/en/petite-prince.md` and split it into translatable strings for the translators!

## Adding New Languages

1. Add config section in `config.toml`:
   In the `languages` section add the following lines, with the language-specific details updated, please note the `disabled: true` line, this language will start disabled:
    ```toml
    [languages.fr]
        disabled = true
        title = 'Ouisync (fr)'
        languageName = 'French'
        contentDir = 'content/fr'
        weight = 2
    ```
Next we will add two `mount` definitions, the first will set each language's content directory as the base:
   ```toml
   [[module.mounts]]
        source = 'content/fr'
        target = 'content'
        lang = 'fr'
   ```

Finally, we will add the "fallback mount" which will add in the english version for any missing files, so untranslated pages won't 404:

NOTE: only replace the `lang` value!
   ```toml
   [[module.mounts]]
        source = 'content/en'
        target = 'content'
        lang = 'fr'
   ```

Now you will just need to add in the new language to Weblate and it should automatically create the strings needed for your translators!

## Enabling Translations

Once a language is ready to go live on the site, make a commit changing the `disabled = true` to `disabled = false`, removing the line will also work. When the Github action runs next, it will deploy the newly added language.

## Custom Shortcodes

This hugo project contains a number of custom [Hugo Shortcodes](https://gohugo.io/content-management/shortcodes/) to make content authoring simpler. All of these shortcodes are to be used inside of markdown documents.

### `render-i18n`

Use: 

```
{{% render-i18n "<stringKey>" %}}
```

This is analogous to the {{ i18n "<stringKey>" }} function used in hugo partials. This allows for strings to be reused in markdown files.

### `markdown`

Use:

```html
<div>
{{% markdown %}}
# Some arbitrary markdown
{{% /markdown %}}
</div>
```

This shortcode allows for markdown to be written inside of custom HTML that is included inside markdown files. This seems fairly insane, but is useful because sometimes markdown is too limited in it's restrictions. This allows to write arbitrary HTML subsections that can render markdown inside of them.

### `render-partial`

Use:

```html
{{% render-partial "some-partial.html" %}}
```

Used to easily embed [Hugo Partials](https://gohugo.io/templates/partials/) inside of markdown documents.

### `center`

Use:

```html
{{% center padding-top="3rem" padding-bot="2rem" %}}
# Some Heading to Center
{{% /center %}}
```

Used to center arbitrary markdown sections and add top and bottom padding. NOTE: if you decide to pass a padding argument, you need to pass both `padding-top` and `padding-bot`. The only shortcode calls resemble the following forms:
- `{{% center %}}<content>{{% /center %}}`
- `{{% center padding-top="<val>" padding-bot="<val>" %}}<content>{{% /center %}}`.
