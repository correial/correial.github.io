+++
title = "About Me"
+++

# Lucca Correia


## Custom Styles

To add your own or override existing styles, create a custom style and add it in the `config.toml`:

```toml
[extra]
styles = [
  "YOUR_STYLE.css",
  "ALSO_YOUR_STYLE.css"
]
```

Additional styles are expected it to be in the `static` directory. If you are using Sass they will be compiled there by default.

If for some reason overridden style is not respected, try using `!important` (don't use it unless needed). You can import styles from Duckquill using:

```scss
@use "../themes/duckquill/sass/NEEDED_FILE.scss";
```

You can also load styles per page/section by setting them inside page's front matter:

```toml
[extra]
styles = [
  "YOUR_PAGE_STYLE.css"
]
```

### Accent Color

Duckquill respects chosen accent color everywhere. To use your own, simply change it in `config.toml`:

```toml
[extra]
accent_color = "#3584e4"
```

Additionally, you can set a separate color for dark mode:

```toml
[extra]
accent_color_dark = "#ff7800"
```

### Favicon

Files named `favicon.png` and `apple-touch-icon.png` are used as favicon and Apple Touch Icon respectively. For animated favicon you can use APNG with the `png` file extension.

## In the Wild

<details>
  <summary>This list is starting to get long, so click on it to expand it.</summary>

- [agustinramirodiaz.github.io](https://agustinramirodiaz.github.io)
- [alavi.me](https://alavi.me)
- [aparoksha.dev](https://www.aparoksha.dev)
- [arfh.pages.dev](https://arfh.pages.dev)
- [bambalabs.co](https://www.bambalabs.co)
- [bano.dev](https://bano.dev)
- [benjaminandre.be](https://benjaminandre.be)
- [blog.digital-horror.co](https://blog.digital-horror.com)
- [blog.millefeuille42.fr](https://blog.millefeuille42.fr)
- [blog.pansi21.xyz](https://blog.pansi21.xyz)
- [blog.thundernetwork.org](https://blog.thundernetwork.org)
- [blog2.maki0419.com](https://blog2.maki0419.com)
- [cabysm.github.io](https://cabysm.github.io)
- [daveparr.info](https://www.daveparr.info)
- [davepoltorak.com](https://davepoltorak.com)
- [drismir.ca](https://drismir.ca)
- [enriquekesslerm.com](https://enriquekesslerm.com)
- [gregorni.gitlab.io](https://gregorni.gitlab.io)
- [harrypotterexplained.com](http://harrypotterexplained.com)
- [hyouteki.github.io](https://hyouteki.github.io)
- [ikergimenez.neocities.org](https://ikergimenez.neocities.org)
- [kaipeacock.com](https://kaipeacock.com)
- [larrabyte.dev](https://larrabyte.dev)
- [lifailon.github.io](https://lifailon.github.io)
- [luciengheerbrant.com](https://luciengheerbrant.com)
- [lukoktonos.com](http://www.lukoktonos.com)
- [matteorisso.github.io](https://matteorisso.github.io)
- [maxffarrell.com](https://maxffarrell.com)
- [maxime.letemple.fr](https://maxime.letemple.fr)
- [mourelask.xyz](https://mourelask.xyz)
- [muelsyse.codeberg.page](https://muelsyse.codeberg.page)
- [mukuljoshi.xyz](https://mukuljoshi.xyz)
- [nbenedek.me](https://nbenedek.me)
- [nikos-archive.org](https://nikos-archive.org)
- [nisf.be](https://nisf.be)
- [nullpuppy.github.io](https://nullpuppy.github.io)
- [nutn-isc.gitlab.io](https://nutn-isc.gitlab.io/nutn-isc-website/)
- [orzklv.uz](https://orzklv.uz)
- [penandink.work](https://penandink.work)
- [pyter.at](https://pyter.at)
- [reallysimple.io](https://www.reallysimple.io)
- [rerere.unlogic.co.uk](https://rerere.unlogic.co.uk)
- [rossjr.dev](https://rossjr.dev)
- [samienr.com](https://samienr.com)
- [shrimple.srht.site](https://shrimple.srht.site)
- [siddharthsabron.in](https://siddharthsabron.in)
- [sorg.codeberg.page](https://sorg.codeberg.page)
- [sungsphinx.codeberg.page](https://sungsphinx.codeberg.page)
- [tmblog.pages.dev](https://tmblog.pages.dev)
- [treeniks.github.io](https://treeniks.github.io)
- [vegner.io](https://vegner.io)
- [voluxyy.github.io](https://voluxyy.github.io/portfolio/)
- [winnydows.com](https://winnydows.com)
- [zlog.si-on.top](https://zlog.si-on.top)
- [zstg.is-a.dev](https://zstg.is-a.dev)
- Yours? <small>(feel free to [contact me](https://daudix.one/find/#contacts) or send a pull request)</small>

</details>

## In Credits

- [andreatitolo.com](https://www.andreatitolo.com/credits)
- [aplos.gxbs.me](https://aplos.gxbs.me)
- [archaeoramblings.com](https://www.archaeoramblings.com/credits)
- [oomfie.town](https://oomfie.town/credits)

## Assets Sources

All sources for Duckquill's assets are available [here](https://codeberg.org/daudix/archive/src/branch/main/duckquill/src) and licensed under CC BY-SA 4.0. The reason for not putting the sources in the same repo as Duckquill itself is simple: I want it to be as small as possible, so that repo cloning is fast and doesn't make the site significantly heavier; this is also why the demo uses remote images instead of local copies.

## Credits

- [Quill image used in the metadata card](https://commons.wikimedia.org/wiki/File:3quills.jpg)

## Tools Used

- [VSCodium](https://vscodium.com) - Free/Libre Open Source Software Binaries of VS Code
  - [Capitalize](https://marketplace.visualstudio.com/items?itemName=viablelab.capitalize) - Title capitalization without random websites.
  - [Even Better TOML](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml) - For `config.toml` basically.
  - [Monokai Pro](https://marketplace.visualstudio.com/items?itemName=monokai.theme-monokai-pro-vscode) - Awfully pretty theme.
  - [PX to REM](https://marketplace.visualstudio.com/items?itemName=cipchk.cssrem) - Easy conversion from PX to REM and vice versa.
  - [SCSS IntelliSense](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-scss) - Not sure if it actually works. ¯\\\_(ツ)_/¯
  - [Sort CSS](https://marketplace.visualstudio.com/items?itemName=piyushsarkar.sort-css-properties) - A lifesaver for long CSS properties.
  - [Tera](https://marketplace.visualstudio.com/items?itemName=karunamurti.tera) - Tera template engine (the one Zola uses) support.
- [Firefox developer tools](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Tools_and_setup/What_are_browser_developer_tools) - Best of its kind.

As for the code formatter I use built-in VSCodium one. Prettier is good but I don't like how it tries to make code fit in a very narrow column, this can be changed of course, but built-in formatter does it's job so I don't bother doing so.


