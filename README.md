<!-- title / description (start) -->
<h2 align="center">Shopify Backup</h2>

Git version control for specific Shopify files
<!-- title / description (end) -->

<!-- ci/cd (start) -->
### CI/CD

#### GitHub actions

1. Add the following four secrets to your Shopify Theme Lab repo in `settings` → `secrets`:

```sh
SHOPIFY_API_PASSWORD # your-api-password
SHOPIFY_STORE_URL # your-store.myshopify.com
SHOPIFY_ENV # dev or live
SHOPIFY_THEME_ID # theme-id (without quotation marks) - find the id either in shopify.[env].config.yml or with shopify:themes task
```

2. Copy and paste into a GitHub action (adjust contents if necessary):

```yml
# Shopify Backup CI/CD integration for GitHub actions
name: Shopify Backup

on:
  schedule:
    - cron: "*/15 * * * *"
  workflow_dispatch: # allows to manually run from GitHub actions panel

jobs:
  backup:
    name: Backup
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [ 14.x ]

    steps:
      - name: Checkout master branch
        uses: actions/checkout@v2

      - name: Use Node.js
        uses: actions/setup-node@v2.1.4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Install dependencies
        run: |
          npm install

      - name: Download from Shopify store
        run: |
          npx themelab shopify:init -p ${{ secrets.SHOPIFY_API_PASSWORD }} -s ${{ secrets.SHOPIFY_STORE_URL }} -e ${{ secrets.SHOPIFY_ENV }} -i ${{ secrets.SHOPIFY_THEME_ID }}
          npm run settings:${{ secrets.SHOPIFY_ENV }}

      - name: Push to repo
        run: |
          git config --global user.name 'GitHub Action'
          git config --global user.email 'your-username@users.noreply.github.com'
          git commit -am 'Automated backup'
          git push
```
<!-- ci/cd (end) -->