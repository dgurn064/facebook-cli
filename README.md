# Facebook CLI

Facebook functionality from the command line.

[![Download the gem at https://rubygems.org/gems/facebook-cli](https://img.shields.io/gem/v/facebook-cli.svg)](https://rubygems.org/gems/facebook-cli)
[![Join the chat at https://gitter.im/facebook-cli/discuss](https://badges.gitter.im/chat.svg)](https://gitter.im/facebook-cli/discuss)
[![Take a moment to thank @specious](https://img.shields.io/badge/Say%20Thanks-!-1EAEDB.svg)](https://saythanks.io/to/specious)

## Demo

<a href="https://asciinema.org/a/87129"><img src="https://asciinema.org/a/87129.png" width="400"/></a>

## Install

`gem install facebook-cli` *(might require sudo)*

## Facebook setup

To interact with the Facebook API you must create and configure a Facebook application for your personal use. Follow these instructions carefully:

- Go to https://developers.facebook.com/apps and create a new app ([screenshot](doc/images/initial-configuration/create-new-app.png))
- Set up a new "Facebook Login" product
- **Don't bother choosing a platform.** Instead click "Settings" under "Facebook Login" in the side bar
- Under "Valid OAuth redirect URIs", add: `https://localhost:3333/` (this is needed to receive auth code during authentication)
- Click "Save Changes"
- Go to "App Review" and make your app public
- Go to "Settings" > "Basic"
- Under "App Secret" click "Show" to reveal your app secret
- Open a terminal and save your App ID and App Secret by running: ([screenshot](doc/images/initial-configuration/save-app-id-and-app-secret.png))<br>

  ```
  facebook-cli config --appid=<app-id> --appsecret=<app-secret>
  ```

## Logging in

Once the [Facebook app is configured](#facebook-setup), you must authorize it to access the social graph on your behalf:

- In a terminal, run: `facebook-cli login` ([screenshot](doc/images/login-procedure/facebook-cli-login.png))
  - (If authenticating on a remote machine or using a different port to receive the auth code: `facebook-cli login --host <hostname-or-ip> --port <port>`)
- Open the given URL in a web browser, and log into your Facebook account if prompted
- Click "Continue" to approve the permissions ([screenshot](doc/images/login-procedure/approve-permissions.png))
- Select the scope of your audience for any posts you publish using this application ([screenshot](doc/images/login-procedure/set-visibility.png)) ([read more](https://www.facebook.com/help/211513702214269))
- Click "Ok" to continue
  - If you're using Firefox and you get a `SEC_ERROR_REUSED_ISSUER_AND_SERIAL` error, just copy the URL and run `curl -k <url>` in a terminal
- Close the browser tab ([screenshot](doc/images/login-procedure/facebook-cli-logged-in.png))

## Commands

Running `facebook-cli` or `facebook-cli help` shows the list of available commands:

```
COMMANDS
    api        - Make a direct Facebook API request
    config     - Save your Facebook API credentials
    feed       - List posts on your timeline
    help       - Shows a list of commands or help for one command
    likes      - List pages you have 'Liked'
    links      - Some useful URLs
    login      - Request Facebook permissions and receive an API access token
    logout     - Deauthorize your access token
    me         - Show your profile information
    photos     - List photos you have uploaded
    photosof   - List photos you are tagged in
    videos     - List videos you have uploaded
    videosof   - List videos you are tagged in
```

Run `facebook-cli help <command>` for more details on each command.

## Converting output to HTML

Use a Markdown renderer to easily convert the output to an HTML document.  For example, using [Pandoc](http://pandoc.org):

```
facebook-cli likes | pandoc -s -f markdown_github > likes.html
```

See: [example](https://specious.github.io/facebook-cli/likes.html)

## Things created with facebook-cli

- [Facebook Friends](https://github.com/specious/facebook-friends/) - Build a page that lets you click your friends' profile pictures to jump to their timelines

  <a href="https://specious.github.io/facebook-friends/"><img src="https://github.com/specious/facebook-friends/raw/master/screenshot.png" width="400"></a>

- [Facebook Browser](https://github.com/specious/facebook-browser/) - Build a searchable index of all the Facebook pages you follow

  <a href="https://specious.github.io/facebook-browser/"><img src="https://github.com/specious/facebook-browser/raw/master/screenshot.png" width="400"></a>

- [Screensaver](https://twitter.com/tknomad/status/815828633231257600) that prints the titles of all of your 'Liked' Facebook pages, made by using the output of `facebook-cli likes | awk 'NR % 3 == 1' | perl -p -e "s/\n/ - /"` as the text input to the [Phosphor](https://www.youtube.com/watch?v=G6ZWTrl7pV0) screensaver

  <a href="https://twitter.com/tknomad/status/815828633231257600"><img src="https://pbs.twimg.com/media/C1JnVlpXEAQurZW.jpg" width="400"></a>

## Why can't I...?

Facebook has removed a large portion of their Graph API starting with version 2.0. [Niraj Shah](https://github.com/niraj-shah) has done a fantastic job documenting the cutbacks and their implications in these blog posts:

* [Facebook API: Graph API v2.4 Released, Removes Groups, Notifications and Stream Permissions](https://www.webniraj.com/2015/07/14/facebook-api-graph-api-v2-4-released-removes-groups-notifications-and-stream-permissions/)
* [Facebook Announces Graph API v2.3, More Deprecations](https://www.webniraj.com/2015/03/26/facebook-announces-graph-api-v2-3-more-deprecations/)
* [Facebook API: Getting Friends Using Graph API 2.0 and PHP SDK 4.0.x](https://www.webniraj.com/2014/06/12/facebook-api-getting-friends-using-graph-api-2-0-and-php-sdk-4-0-x/)

An [article](https://developers.facebook.com/blog/post/2012/10/10/growing-quality-apps-with-open-graph/) published in October, 2012 by Facebook developer [Henry Zhang](https://www.facebook.com/hzz) provides insight into the reasoning:

> Post to friends wall via the API generate a high levels of negative user feedback, including “Hides” and “Mark as Spam" and so we are removing it from the API. If you want to allow people to post to their friend’s timeline from your app, you can invoke the feed dialog.

More recent changes [outlined](https://developers.facebook.com/docs/graph-api/changelog/breaking-changes) on Facebook's developer portal cover further massive deprecations enactedin 2018, which have eliminated facebook-cli's ability to access past and upcoming events, publish content and retrieve names of taggable friends, as was still possible with the previous long-standing restriction.  facebook-cli's available functions have been pruned to reflect the latest changes.

The functionality removed renders it impossible to build a full-featured interface to Facebook through the Graph API alone.

If you expand the functionality of facebook-cli in any way, please [open a pull request](https://github.com/specious/facebook-cli/pulls).

## Development

Clone this repository, then use [Bundler](http://bundler.io) to install Ruby dependencies:

```
gem install bundler
bundle install
```

You should now be able to run *facebook-cli* from the `bin` directory:

```
bundle exec bin/facebook-cli
```

*facebook-cli* depends on the [Koala](https://github.com/arsduo/koala) library for interfacing with Facebook's Graph API and [GLI](https://github.com/davetron5000/gli) for parsing command line arguments.

Using a ruby environment manager such as [rbenv](https://github.com/rbenv/rbenv) or [rvm](https://rvm.io) is advisable to avoid environment conflicts between projects.

Please read the guide on [how to contribute](CONTRIBUTING.md).

## Facebook client software in the open source community

Applications:

- [vhpoet/facebook-cli](https://github.com/vhpoet/facebook-cli) (Facebook CLI written in NodeJS)
- [fb-messenger-cli](https://github.com/Alex-Rose/fb-messenger-cli) (Facebook Messenger for the terminal)
- [Messenger for Desktop](https://github.com/Aluxian/Messenger-for-Desktop) (Facebook Messenger standalone app)

Libraries:

- [Koala](https://github.com/arsduo/koala/) (Ruby)
- [facebook-node-sdk](https://github.com/node-facebook/facebook-node-sdk) (JavaScript)

## License

ISC
