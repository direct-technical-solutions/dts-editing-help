# Editing Help

A WordPress plugin that shows the content guide inside WordPress: an "Editing help" panel at the
top of the Dashboard and a page with the whole guide, linked in the left menu under Dashboard. The
guide ends with the site's own page templates and ready-made sections, read from the active theme.
It names no site and can be used by any site that wants it. Requires WordPress 7.0 and PHP 8.4.

## Using it in a site

A DTS site lists it in `site-packages/composer.json` at a release tag (the version without its
`v`), from this repository:

```json
{
    "repositories": [
        { "type": "vcs", "url": "https://github.com/direct-technical-solutions/dts-editing-help" }
    ],
    "require": { "dts/editing-help": "0.1.1" }
}
```

Composer places it in `wp-content/plugins/editing-help/`. Its header says `Update URI: false`, so
WordPress never offers a plugin of the same name from WordPress.org as an update of this one. Its
updates reach a site only through a new tag and the site's own pin.

The site's theme supplies what only it knows, through three filters:

- `editinghelp_template_descriptions`: an array of a page template's slug (`page` for the
  default) => one line saying what it does; a template without one shows its name only.
- `editinghelp_link_example`: a short link text in the site's own words, used in the guide's
  example; without it, a neutral one.
- `editinghelp_contact`: an email address or a short line, shown at the end of the guide as who to
  ask; without it, the line is left out.

## Working on it

The checks come from [`dts-site-tools`](https://github.com/direct-technical-solutions/dts-site-tools),
required in `composer.json`'s `require-dev`. After a fresh clone, and after each change to
`composer.lock`, the tools are installed with Composer in its pinned container:

```sh
mkdir -p vendor bin && docker run --rm -w /app -v "$PWD/composer.json:/app/composer.json:ro" -v "$PWD/composer.lock:/app/composer.lock:ro" -v "$PWD/vendor:/app/vendor" -v "$PWD/bin:/app/bin" composer:2.11.1 composer install --no-interaction --no-progress
```

`bin/plugin-check` runs every check, as the `check` workflow does on each pull request, on each
tag and weekly: the WordPress Coding Standards (`phpcs.xml.dist`, prefix `editinghelp`, text
domain `editing-help`), PHPStan at its strictest level (`phpstan.neon.dist`), and the plugin's
file rules. The plugin's browser tests are kept with the tools and run in the sites that use it.

**The guide's text** is also kept in the DTS Operations Handbook (`customer/content-guide.md`),
with the same version number as `EDITINGHELP_GUIDE_VERSION`; a change to the text goes into both.

**A release:** raise the main file's `Version:`, give the version its section in `CHANGELOG.md`,
merge, then run `bin/plugin-check --tag v<version>` on the merged `main` before pushing the tag
`v<version>`. Tags are never moved or deleted.

## License

GPL-2.0-or-later, as WordPress. See [`LICENSE`](LICENSE).
