# discourse-locale-override

Replaced example locale files with "en" and overrode several strings of interest to our project

## Installation on top of Docker image

Add to your `/var/discourse/containers/app.yml`:

```ruby
hooks:
  after_code:
    - exec:
        cd: $home/plugins
        cmd:
          - mkdir -p plugins
          - git clone https://github.com/lidel/discourse-locale-override.git
```

Rebuild Discourse: `/var/discourse/launcher rebuild app`

## FAQ

### How can I use this to override locale X?

1. Fork this repository
2. Remove `config/locales/*.pl_PL.yml` files *(optional)*
3. Add `config/locales/*.xx_XX.yml` ones    
  **Warning:** `xx_XX` should be the language code you want to *override*.    
  Files with this locale code must be already present upstream at  [discourse/config/locales](https://github.com/discourse/discourse/tree/master/config/locales).    
  This is important: adding non-existing locale codes will *break your instance*.
4. **Crucial:** make sure your `.yml` files are valid.    
  Test them at the [YAML Validator](http://www.yamllint.com) before you commit them to your repository.
5. Update repository URL in Docker hook to point to your fork

### Where do I get original `.yml` files from?

There are two places I am aware of:
- Github: https://github.com/discourse/discourse/tree/master/config/locales
- Transifex: https://www.transifex.com/projects/p/discourse-org/

If you care about latest translations use Transifex.
Github is updated with Transifex changes about once a week.

### Can I override entire locale?

Yes, if your `.yml` file has translation for all keys the entire locale with be overridden.

### Can I override only one label?

Yes, but make sure you understand YAML and all key segments used in label key are present.
For example, to override `pl_PL.site_settings.enable_emoji` make sure that `config/locales/server.pl_PL.yml` has these three lines (one for each segment):

```
pl_PL:
  site_settings:
    enable_emoji: "Włącz wyświetlanie Emoji"
```
