# University of Windsor CSS Hub

http://css.uwindsor.ca

## Overview

The University of Windsor Computer Science Society (CSS) website was built to provide students with the many resources that students and CSS has to offer. It provides a location for news, events, a job board, an open-source student guide, and even our exclusive authentication into our Discord server!

## Contributing

Please feel free to contribute if you see an issue or think of a sweet feature! Simply fork this repo and make a pull request.

If this sounds like gibberish to you, you'll probably want to learn the basics of `git`. Afterwards, you can follow [this tutorial](https://akrabat.com/the-beginners-guide-to-contributing-to-a-github-project/) on contributing to open-source projects on GitHub.

### Rails Setup

You can follow [these instructions](https://www.tutorialspoint.com/ruby-on-rails/rails-installation.htm) to install `ruby` and the `rails` framework.

After installing rails, you need to install the dependencies (gems). Navigate to the project directory and run:

`$ bundle install`

Bundler will install all of the project dependencies (located in `Gemfile`) of the project.

Next you'll need to create the local database using

`$ rails db:setup`

After that you're ready to launch the site using:

`$ rails server`

By default, rails will launch the server at [http://localhost:3000](http://localhost:3000).

### Google OAuth Setup

This is not necessary, however to sign in to your UWindsor email on your local instance and use admin features you will need to set up Google OAuth.

Signing in will allow you to
- Create/modify events
- Modify page content in-site

Follow these steps from Google to create a project and obtain the client ID and secret: https://developers.google.com/adwords/api/docs/guides/authentication#create_a_client_id_and_client_secret
- Your project can be named whatever you like
- This is completely free, don't worry about the billing account options
- Application type is a Web Application
- Under Authorized Redirect URI's, enter `http://localhost:3000/auth/google_oauth2/callback`

Now that you have a Google client ID and client secret, you just need to add those to your environment file where the app reads the keys.

In the rails project, copy the `local_env.example.yml` to a new file called `local_env.yml`. This will be your private file that contains your API credentials - git will ignore this file. Fill in the `GOOGLE_CLIENT` and `GOOGLE_SECRET` fields in `local_env.yml` with your respective credentials.

After restarting the rails server, you should be able to sign in using UWindsor accounts. You can now add yourself as an admin through the rails console. After signing into the app at least once (so that your user record is created), run `rails console` or `rails c` to open the console, then change your role to admin using:
`User.where(email: "youruwindsoremail").first.update(role: "admin")`

### Discord OAuth Setup

Discord auth setup will add the Discord functionality to your application. This is very similar to how we set up Google OAuth. This application uses Discord to:
- Allow users to sign in/be added to the CSS Discord Server
- Post messages in the Discord server (e.g. new events)

1. Go to the Discord Developer Portal
2. Create a new application (name it whatever you want)
3. Under your new application -> OAuth2, add `http://localhost:3000/auth/discord/callback` to your list of redirects
4. Under "Bot", create a new bot (name it whatever you want)
5. Copy the bot's client secret to `DISCORD_BOT_TOKEN` in `local_env.yml`
6. Under "General information", copy the client ID and secret (different from bot secret) to your `local_env.yml` file's `DISCORD_CLIENT_ID` and `DISCORD_CLIENT_SECRET` respectively
7. Now we need to add the bot your Discord server. Under "OAuth2", navigate to scopes and ensure that `bot` is checked. Under "Bot permissions", check `Administrator`
8. Under scopes you should see a generated invitation URL based on the permissions/scopes you've selected. Paste that link into your browser and you'll be able to add the bot to whichever server you choose
9. Finally, the application needs to know which server and channel it's sending messages and adding users to. You'll need to [find your server and channel IDs](https://support.discordapp.com/hc/en-us/articles/206346498-Where-can-I-find-my-User-Server-Message-ID-) and fill in `DISCORD_GUILD_ID` and `DISCORD_EVENTS_CHANNEL_ID`.

Now you're all set!


### Common issues

A few people reported issues with a dependency, `libv8`. This can be fixed by installing the `libv8-dev` package using:
`sudo apt-get install libv8-dev` or `brew install libv8-dev`.

