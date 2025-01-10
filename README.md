## Mastodon BirdUI with Catppuccin Mocha colors

This is a Catppuccin Mocha variant of Roni Laukkarinen's [Mastodon BirdUI](https://github.com/ronilaukkarinen/mastodon-bird-ui).

## Installation for Mastodon instance admins

1. Copy the contents of layout-single-column.css and layout-multiple-columns.css and paste them (or one of them) to the **Custom CSS** in the Appearance settings in your instance (https://_yourinstance_/admin/settings/appearance). It might be recommended using the single layout CSS as "base" and use the advanced view CSS with browser extension (as it's desktop only anyway).

![Screen-Shot-2023-03-31-13-25-52](https://user-images.githubusercontent.com/1534150/229111630-c8975708-134b-4887-b259-a87857193387.png)

## Make Mastodon Bird UI as optional by integrating it as 'Site theme' in settings for all users

Mastodon Bird UI can be integrated as a **Site theme** for all instance users as optional.

**Please note**: These include modifications to the Mastodon core so do it only at your own risk! I highly recommend you to make the modifications in local development envinronment, push them to your fork, then git pull that fork to live after confirmed working.

![image](https://github.com/ronilaukkarinen/mastodon-bird-ui/assets/1534150/b30f19e2-2835-4d92-b40d-cac9922f64b3)

If you'd like a different branding for your instance like "Elephant" without any [mention of birds](https://github.com/ronilaukkarinen/mastodon-bird-ui/issues/30), use [Bird UI Theme Admins](https://github.com/mstdn/Bird-UI-Theme-Admins) by [@stux](https://mstdn.social/@stux). If you want Mastodon Bird UI to be as default, read along.

Cd to your Mastodon directory (usually $HOME/live) you can run these bash commands (**Please note:** These add Mastodon Bird UI as name "Mastodon Bird UI (Dark)" + variants as default, while retaining the original themes as secondary themes) and run below.

**Mastodon main/nightly:** `nightly`<br>
**Mastodon stable:** `main`

```bash
export MASTODON_VERSION_FOR_BIRD_UI="main"

# Create a new folder for the theme
mkdir -p app/javascript/styles/mastodon-bird-ui

# Download the CSS file for single column layout
wget -N --no-check-certificate --no-cache --no-cookies --no-http-keep-alive https://raw.githubusercontent.com/jcrabapple/mastodon-bird-ui-catppuccin-mocha/$MASTODON_VERSION_FOR_BIRD_UI/layout-single-column.css -O app/javascript/styles/mastodon-bird-ui-catppuccin-mocha/layout-single-column.scss

# Download the CSS file for multiple column layout
wget -N --no-check-certificate --no-cache --no-cookies --no-http-keep-alive https://raw.githubusercontent.com/jcrabapple/mastodon-bird-ui-catppuccin-mocha/$MASTODON_VERSION_FOR_BIRD_UI/layout-multiple-columns.css -O app/javascript/styles/mastodon-bird-ui-catppuccin-mocha/layout-multiple-columns.scss

# Create dark theme file
echo -e "@import 'application';\n@import 'mastodon-bird-ui-catppuccin-mocha/layout-single-column.scss';\n@import 'mastodon-bird-ui-catppuccin-mocha/layout-multiple-columns.scss';" > app/javascript/styles/mastodon-bird-ui-catppuccin-mocha-dark.scss

# Overwrite config/themes.yml with new settings, Mastodon Bird UI dark as default
echo -e "default: styles/mastodon-bird-ui-catppuccin-mocha-dark.scss\nmastodon-dark: styles/application.scss\nmastodon-light: styles/mastodon-light.scss\ncontrast: styles/contrast.scss" > config/themes.yml
```

After this you need to edit localisations in `config/locales/en.yml` (`nano config/locales/en.yml`) and add this line:

```yml
     mastodon-bird-ui-catppuccin-mocha-dark: Mastodon Bird UI Catppuccin Mocha
```

Same for the localizations of your choice, for example `config/locales/fi.yml` (`nano config/locales/fi.yml`):


```yml
  mastodon-bird-ui-catppuccin-mocha-dark: Mastodon Bird UI Catppuccin Mocha (Tumma)
```

Make sure everything is set in place, then rebuild all the assets and restart all the services:

```bash
bundle install
rm -rfv public/packs
yarn install
RAILS_ENV=production bundle exec rails assets:precompile
sudo systemctl restart mastodon-web mastodon-sidekiq mastodon-streaming
sudo systemctl restart postgresql
```

And you're done!

## Installation for regular users, contributing and testing

1. Install [Live CSS Editor](https://github.com/webextensions/live-css-editor) (or any other extension like [Stylus](https://chrome.google.com/webstore/detail/stylus/clngdbkpkpeebahjckkjfobafhncgmne?hl=en) that allows you to inject CSS into web pages) or use [Unite for macOS](https://www.bzgapps.com/unite) or use the [user.js by eg](https://ieji.de/@eg/110174544387143309)
2. Copy the contents of layout-single-column.css and layout-multiple-columns.css
3. Open extension and paste the contents of both CSS files into the editor
4. If you use Live CSS Editor, click 📌-icon so the styles will be remembered for the domain or if you want just to use it as needed, activate styles from the extension's popup
