# Contributing

First off, thank you for considering contributing towards Homeview! 🙌
There are several ways that you can help out, and any contributions, however small will always be very much appreciated.
You will be appropriately credited in the readme - huge thank you to [everyone who has helped](/docs/credits.md) so far 💞

## Take a 2-minute survey

Help improve Homeview by taking a very short, 6-question survey. This will give me a better understanding of what is important to you, so that I can make Homeview better in the future :)

[![Take the Survey](https://img.shields.io/badge/Take_the-Survey-%231a86fd?style=for-the-badge&logo=buddy)](https://survey.typeform.com/to/gl0L68ou)

## Share your dashboard

Homeview now has a [Showcase](https://github.com/KhulnaSoft/homeview/blob/master/docs/showcase.md#homeview-showcase-) where you can show off a screenshot of your dashboard, and get inspiration from other users (and I really love seeing how people are using Homeview). To [submit your dashboard](https://github.com/KhulnaSoft/homeview/blob/master/docs/showcase.md#submitting-your-dashboard), either open a PR or raise an issue.

[![Add your Dashboard to the Showcase](https://img.shields.io/badge/Add_your_Dashboard-Showcase-%238616ee?style=for-the-badge&logo=feathub&logoColor=8616ee)](https://github.com/KhulnaSoft/homeview/issues/new?assignees=&labels=%F0%9F%92%AF+Showcase&template=showcase-addition.yml&title=%5BSHOWCASE%5D+%3Ctitle%3E)

## Make a small donation

Donations help to cover server costs, development time and caffeine ;)
Don't feel any pressure to donate anything, as Homeview and my other projects will always be 100% free, for everyone, for ever.

[![Sponsor KhulnaSoft on GitHub](https://img.shields.io/badge/Sponsor_on_GitHub-KhulnaSoft-%23ff4dda?style=for-the-badge&logo=githubsponsors&logoColor=ff4dda)](https://github.com/sponsors/KhulnaSoft)

Sponsoring will give you several perks - for $1 / £0.75 per month, you'll get a sponsor badge on your profile, be credited on the Homeview's readme, with a link to your website/ profile/ socials, get priority support,  have your feature ideas implemented, plus lots more. For more info, see [@KhulnaSoft's Sponsor Page](https://github.com/sponsors/KhulnaSoft).

<details>
	<summary>You can also send a one-off small contribution using crypto</summary>
	<p>

[![Donate with BTC](https://en.cryptobadges.io/badge/big/3853bSxupMjvxEYfwGDGAaLZhTKxB2vEVC)](https://en.cryptobadges.io/donate/3853bSxupMjvxEYfwGDGAaLZhTKxB2vEVC)[![Donate with Ethereum](https://en.cryptobadges.io/badge/big/0x0fc98cBf8bea932B4470C46C0FbE1ed1f6765017)](https://en.cryptobadges.io/donate/0x0fc98cBf8bea932B4470C46C0FbE1ed1f6765017)

- **BTC**: `3853bSxupMjvxEYfwGDGAaLZhTKxB2vEVC`
- **ETH**: `0x0fc98cBf8bea932B4470C46C0FbE1ed1f6765017` / `khulnasoft.eth`
- **XMR**: `471KZdxb6N63aABR4WYwMRjTVkc1p1x7wGsUTEF7AMYzL8L94A5pCuYWkosgJQ5Ze8Y2PscVCGZFJa3hDPg6MaDq47GUm8r`
- **LTC**: `MAuck6Ea1qaNihwKfXutkR1R6BorMth86H`
- **ZEC**: `t1bw1SefijsXRDQVxC9w64XsRK8hBhtQohQ`

  </p>

</details>

## Enable Anonymous Bug Reports

Bug reports helps me to discover bugs I was unaware of, and then fix them, in order to make Homeview more reliable long term. This is a simple, yet really helpful step you can take to help improve Homeview. [Sentry](https://github.com/getsentry/sentry) is an open source error tracking and performance monitoring tool, which enables the identification any errors which occur in the production app (only if you enable it).

To enable error reporting:

```yaml
appConfig:
  enableErrorReporting: true
```

All reporting is **disabled** by default, and no data will ever be sent to any external endpoint without your explicit consent. All statistics are anonymized and stored securely. For more about privacy and security, see the [Sentry Security Docs](https://sentry.io/security/).

## Add Translations

If you speak another language, then adding translations will help make Homeview available to non-native English speakers. This is a very quick and easy task, as all application text is located in [`locales/en.json`](https://github.com/KhulnaSoft/homeview/blob/master/src/assets/locales/en.json), so adding a new language is as simple as copying this file and translating the values. You don't have to translate it all, as any missing attributes will just fallback to English. For a full tutorial, see the [Multi-Language Support Docs](https://github.com/KhulnaSoft/homeview/blob/master/docs/multi-language-support.md).

## Submit a PR

Contributing to the code or docs is super helpful. You can fix a bug, add a new feature or improve an existing one. If you've built your own custom widget, theme or view, consider sharing it in a PR. I've written [several guides](/docs/development-guides.md) to help you get started, and the steps for setting up the development environment are outlined in the [Development Docs](/docs/developing.md). Feel free to ask if you have any questions.

## Improve the Docs

Found a typo, or something that isn't as clear as it could be? Maybe I've missed something off altogether, or you hit a roadblock that took you a while to figure out. Submitting a pull request to add to or improve the documentation will help future users get Homeview up and running more easily.
All content is located either in the [`./README.md`](/README.md) or [`/docs/`](/docs) directory, and synced to the Wiki and website using a GH [action](/actions/workflows/wiki-sync.yml).

## Raise a bug

If you've found a bug, then please do raise it as an issue. This will help me know if there's something that needs fixing. Try and include as much detail as possible, such as your environment, steps to reproduce, any console output and maybe an example screenshot or recording if necessary.

[![Raise a Bug](https://img.shields.io/badge/Raise_a-Bug-%23dc2d76?style=for-the-badge&logo=dependabot)](https://github.com/KhulnaSoft/homeview/issues/new?assignees=khulnasoft&labels=%F0%9F%90%9B+Bug&template=bug.yml&title=%5BBUG%5D+%3Ctitle%3E)

## Join the discussion

I've enabled the discussion feature on GitHub, here you can share tips and tricks, useful information, or your dashboard. You can also ask questions, and offer basic support to other users.

[![Join the Discussion on GitHub](https://img.shields.io/badge/Join_the-Discussion-%23ffd000?style=for-the-badge&logo=livechat)](https://github.com/KhulnaSoft/homeview/discussions)

## Request a feature via BountySource

BountySource is a platform for sponsoring the development of certain features on open source projects. If there is a feature you'd like implemented into Homeview, but either isn't high enough priority or is deemed to be more work than it's worth, then you can instead contribute a bounty towards it's development. You won't pay a penny until your proposal is fully built, and you are satisfied with the result. This helps support the developers, and makes Homeview better for everyone.

[![Request a Feature on BountySource](https://img.shields.io/badge/BountySource-Homeview-%23F67909?style=for-the-badge&logo=openbugbounty)](https://www.bountysource.com/teams/homeview)

## Spread the word

Homeview is still a relatively young project, and as such not many people know of it. It would be great to see more users, and so it would be awesome if you could consider sharing with your friends or on social platforms.

[![Share Homeview on Mastodon](https://img.shields.io/badge/Share-Mastodon-%232b90d9?style=flat-square&logo=mastodon)](https://mastodon.social/?text=Check%20out%20Homeview%2C%20the%20privacy-friendly%2C%20self-hosted%20startpage%20for%20organizing%20your%20life%3A%20https%3A%2F%2Fgithub.com%2FKhulnaSoft%2Fhomeview%20-%20By%20%40khulnasoft%40mastodon.social)
[![Share Homeview on Reddit](https://img.shields.io/badge/Share-Reddit-%23FF5700?style=flat-square&logo=reddit)](http://www.reddit.com/submit?url=https://github.com/KhulnaSoft/homeview&title=Homeview%20-%20The%20self-hosted%20dashboard%20for%20your%20homelab%20%F0%9F%9A%80)
[![Share Homeview on Twitter](https://img.shields.io/badge/Share-Twitter-%231DA1F2?style=flat-square&logo=twitter)](https://twitter.com/intent/tweet?url=https://github.com/khulnasoft/homeview&text=Check%20out%20Homeview%20by%20@Lissy_Sykes,%20the%20self-hosted%20dashboard%20for%20your%20homelab%20%F0%9F%9A%80)
[![Share Homeview on Facebook](https://img.shields.io/badge/Share-Facebook-%234267B2?style=flat-square&logo=facebook)](https://www.facebook.com/sharer/sharer.php?u=https://github.com/khulnasoft/homeview)
[![Share Homeview on LinkedIn](https://img.shields.io/badge/Share-LinkedIn-%230077b5?style=flat-square&logo=linkedin)](https://www.linkedin.com/shareArticle?mini=true&url=https://github.com/khulnasoft/homeview)
[![Share Homeview on Pinterest](https://img.shields.io/badge/Share-Pinterest-%23E60023?style=flat-square&logo=pinterest)](https://pinterest.com/pin/create/button/?url=https://github.com/khulnasoft/homeview&media=https://raw.githubusercontent.com/KhulnaSoft/homeview/master/docs/showcase/1-home-lab-material.png&description=Check%20out%20Homeview,%20the%20self-hosted%20dashboard%20for%20your%20homelab%20%F0%9F%9A%80)
[![Share Homeview on VK](https://img.shields.io/badge/Share-VK-%234C75A3?style=flat-square&logo=vk)](https://vk.com/share.php?url=https%3A%2F%2Fgithub.com%2Fkhulnasoft%2Fhomeview%2F&title=Check%20out%20Homeview%20-%20The%20Self-Hosted%20Dashboard%20for%20your%20Homelab%20%F0%9F%9A%80)
[![Share Homeview via Viber](https://img.shields.io/badge/Share-Viber-%238176d6?style=flat-square&logo=viber)](viber://forward?text=https%3A%2F%2Fgithub.com%2Fkhulnasoft%2Fhomeview%0ACheck%20out%20Homeview%2C%20the%20self-hosted%20dashboard%20for%20your%20homelab%20%F0%9F%9A%80)
[![Share Homeview via Telegram](https://img.shields.io/badge/Share-Telegram-%230088cc?style=flat-square&logo=telegram)](https://t.me/share/url?url=https%3A%2F%2Fgithub.com%2Fkhulnasoft%2Fhomeview&text=Check%20out%20Homeview%2C%20the%20self-hosted%20dashboard%20for%20your%20homelab%20%F0%9F%9A%80)
[![Share Homeview via Email](https://img.shields.io/badge/Share-Email-%238A90C7?style=flat-square&logo=protonmail)](mailto:info@example.com?&subject=Check%20out%20Homeview%20-%20The%20self-hosted%20dashboard%20for%20your%20homelab%20%F0%9F%9A%80&cc=&bcc=&body=https://github.com/khulnasoft/homeview)

## Star, Upvote or Leave a Review

Homeview is on the following platforms, and if you could spare a few seconds to give it an upvote or review, this will also help new users discover Homeview

[![ProductHunt](https://img.shields.io/badge/Review-ProductHunt-%23b74424?style=flat-square&logo=producthunt)](https://www.producthunt.com/posts/homeview)
[![AlternativeTo](https://img.shields.io/badge/Review-AlternativeTo-%235581a6?style=flat-square&logo=abletonlive)](https://alternativeto.net/software/homeview/about/)
[![Slant](https://img.shields.io/badge/Review-Slant-%2346a1df?style=flat-square&logo=capacitor)](https://www.slant.co/improve/topics/27783/viewpoints/1/~self-hosted-homelab-startpage~homeview)
[![Star on GitHub](https://img.shields.io/github/stars/KhulnaSoft/Homeview?color=ba96d6&label=Star%20-%20GitHub&logo=github&style=flat-square)](https://github.com/KhulnaSoft/homeview/stargazers)
[![Star on DockerHub](https://img.shields.io/docker/stars/khulnasoft/homeview?color=4cb6e0&label=Star%20-%20Docker&logo=docker&style=flat-square)](https://hub.docker.com/r/khulnasoft/homeview)

## Follow for More

If you've enjoyed Homeview, you can follow the me to get updates about other projects that I am working on.

[![KhulnaSoft Ltd on Twitter](https://img.shields.io/twitter/follow/Lissy_Sykes?style=social&logo=twitter)](https://twitter.com/Lissy_Sykes)
[![KhulnaSoft Ltd on GitHub](https://img.shields.io/github/followers/khulnasoft?label=KhulnaSoft&style=social)](https://github.com/KhulnaSoft)
[![KhulnaSoft Ltd on Mastodon](https://img.shields.io/mastodon/follow/1032965?domain=https%3A%2F%2Fmastodon.social)](https://mastodon.social/web/accounts/1032965)
[![KhulnaSoft Ltd on Keybase](https://img.shields.io/badge/khulnasoft--lightgrey?style=social&logo=Keybase)](https://keybase.io/khulnasoft)
[![KhulnaSoft Ltd's Website](https://img.shields.io/badge/khulnasoft.com--lightgrey?style=social&logo=Tencent%20QQ)](https://khulnasoft.com)
[![KhulnaSoft Ltd's Blog](https://img.shields.io/badge/Blog--lightgrey?style=social&logo=micro.blog)](https://notes.khulnasoft.com/)
[![KhulnaSoft Ltd's PGP](https://img.shields.io/badge/PGP--lightgrey?style=social&logo=Let%E2%80%99s%20Encrypt)](https://keybase.io/khulnasoft/pgp_keys.asc)

If you like, you could also consider [subscribing to my mailing list](https://notes.khulnasoft.com/subscribe) for occasional blog post updates.

---

### Contributors

For a full list of Homeview's contributors, see the [Credits Page](/docs/credits.md)

[![Auto-generated contributors](https://raw.githubusercontent.com/KhulnaSoft/homeview/master/docs/assets/CONTRIBUTORS.svg)](https://github.com/KhulnaSoft/homeview/blob/master/docs/credits.md)

### Star-Gazers Over Time

[![Stargazers](https://starchart.cc/KhulnaSoft/homeview.svg)](https://seladb.github.io/StarTrack-js/#/preload?r=KhulnaSoft,homeview)
