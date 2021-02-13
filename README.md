<!-- logo (start) -->
<p align="center">
  <img src=".github/img/logo.svg" width="300px">
</p>
<!-- logo (end) -->

<!-- badges (start) -->
<p align="center">
  <img src="https://img.shields.io/github/package-json/v/uicrooks/shopify-settings-control?color=%236e78ff">
</p>
<!-- badges (end) -->

<!-- title / description (start) -->
<h2 align="center">Shopify Settings Control</h2>

Automatic Git version control for Shopify `settings_data.json` via CI/CD. Create a dedicated repo to backup `settings_data.json` for a Shopify store.
> Designed to work with [Shopify Theme Lab](https://github.com/uicrooks/shopify-theme-lab).
<!-- title / description (end) -->

<!-- how it works (start) -->
## How it works
A GitHub action runs automatically once during the specified interval. It downloads the `settings_data.json` file from the provided Shopify store. If the remote file has changed, it's committed and then pushed to this repo, if not, nothing happens. This project has a similar directory structure to Shopify Theme Lab, you will find the settings file in `shopify/config/settings_data.json` once it's added to this repo.
<!-- how it works (end) -->

<!-- getting started (start) -->
## Getting started

### GitHub actions

1. “Use this template” to create a new repository on GitHub
2. Add the following four secrets to your Shopify Theme Lab repo in `settings` → `secrets`:
```sh
SHOPIFY_API_PASSWORD # your-api-password
SHOPIFY_STORE_URL # your-store.myshopify.com
SHOPIFY_ENV # dev or live
SHOPIFY_THEME_ID # theme-id (without quotation marks) - find the id either in shopify.[env].config.yml or with shopify:themes task in your Shopify Theme Lab project
```
3. Test the action by going to `actions` → `Shopify Settings Control` and click on `Run workflow`
<!-- getting started (end) -->

<!-- adjusting schedule (start) -->
## Adjusting schedule
By default, the action is set to run once every hour. You can adjust the `cron` schedule inside [main.yml](.github/workflows/shopify-settings-control.yml) to change the interval.

```yml
on:
  schedule:
    - cron: "*/60 * * * *"
```

```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of the month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of the week (0 - 6)
│ │ │ │ │
│ │ │ │ │
│ │ │ │ │
* * * * *
```

An asterisk `*` means that the field is considered “any time” - so `* * * * *` means every minute of every day. Note that, at least on GitHub, times are based in UTC so you may have to do some timezone conversion!

To visualize chron expressions you can use [crontab.guru](https://crontab.guru).
<!-- adjusting schedule (end) -->