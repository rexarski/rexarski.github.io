# README

A new old friend. Currently using `qrui.xyz` and `interetro.xyz` as domain name.

Made some modifications based on [Fuji](https://github.com/amzrk2/hugo-theme-fuji/).

## Update Submodule

```bash
git submodule update --remote --merge
```

## Trigger a build through a build hook

```bash
curl -X POST -d {} https://api.netlify.com/build_hooks/xxxxxxxxxxxxx
```

## TODO

- [x] fix missing tags in deployment
- [x] check 404
- [x] favicon
- [x] add Google analytics, `googleAnalytics`
- [x] `description = "A minimal Hugo theme with nice theme color."`
- [x] customize blog fonts
- [x] `images = ["img/og.png"] # This will use the image called og.png in static/img folder`
- [ ] GitHub script css fix in dark theme
- [ ] syntax highlighting for more languages (?)
- [x] add mobile responsive support
