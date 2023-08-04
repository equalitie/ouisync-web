# Contributing
The website is hosted by GitHub Pages and most content is authored in Markdown format. HTML is also acceptable. 

## Add a New Page

To easily add a new page, you can use the `hugo new` command. For example, lets say we want to add a new page at `https://ouisync.net/mesh`, we can run the command:

```hugo new mesh.md```

This creates a `mesh.md` inside the `content/en/` path. When you first open the file you will notice it has premade [front matter](https://gohugo.io/content-management/front-matter/). This information is pulled from the `archetypes/default.md` file, feel free to edit it if you want to add information that gets automatically added when running `hugo new`.

Now that we have a new blank markdown file, we can write an entire article in markdown format and hugo will automatically push these changes to the website when the changes are merged into the `main` branch of the git repo.

To learn how to ready articles for translation, please read the "Translations" section below.

## Edit an Exisiting Page

When editing content on a hugo site, there are a few different directories that are important. They represent the "content directory" these content directories are the input values that hugo uses to generate the output found in the `public/` directory.

- `layouts/index.html`: the homepage
- `layouts/partials/`: this directory holds HTML fragments which can be called from other partials, the homepage, or, by using a custom shortcode, from markdown files.
- `content/en/`: in our english-centric hugo project, this is the base content directory where markdown files can be placed. Placing a translated copy of a markdown file with the same name, in a seperate `content/` folder with a language code will automatically add the translation to the hugo site. For example if we have an english markdown file: `content/en/example.md`. We can translate the file to French and place it in `content/fr/english.md`. Hugo will automatically recognize this as a french translation for `example.md`.

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

For markdown we use the custom `render-i18n` shortcode, lets say this file is in `content/fr/hello.md`:

```
# {{% render-i18n "title" %}}

{{% render-i18n "intro" %}}
```

Hugo will also server multilingual content based on the *content directory* of the markdown file. So if translation software allows for the translation of entire markdown files, this approach can be used instead. The string-based, and entire-markdown-file based approached can also be combined. For example let's say an english sentance was added to the `content/en/hello.md` file:

```
# {{% render-i18n "title" %}}

{{% render-i18n "intro" %}}

Once when I was six years old I saw a magnificent picture in a book, called True Stories from Nature, about the primeval forest. It was a picture of a boa constrictor in the act of swallowing an animal. Here is a copy of the drawing. 
```

Now instead of converting everything to custom strings and editing in Weblate, we could translate the markdown directly if we want, `content/fr/hello.md`:

```
# {{% render-i18n "title" %}}

{{% render-i18n "intro" %}}

Lorsque j’avais six ans j’ai vu, une fois, une magnifique image, dans un livre sur la Forêt Vierge qui s’appelait « Histoires Vécues ». Ça représentait un serpent boa qui avalait un fauve. Voilà la copie du dessin.
```

After rebuilding and deploying this site, you will be able to view the following at `https://{{.BaseURL}}/fr/hello/`:

```html
<h1>Bonjour</h1>
<p>Je m’appelle Alice</p>
<p>Lorsque j’avais six ans j’ai vu, une fois, une magnifique image, dans un livre sur la Forêt Vierge qui s’appelait « Histoires Vécues ». Ça représentait un serpent boa qui avalait un fauve. Voilà la copie du dessin.</p>
```

