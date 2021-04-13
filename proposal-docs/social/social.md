# The "podcast:social" Specification

<small>Version 1.0 by Benjamin Bellamy - 2021.04.13</small>

<br />

## Purpose

Social networks (Facebook, Instagram, Twitter, Mastodon…) and discussion platforms (Slack, Discord, Jabber, IRC, Matrix…) are a convenient way
for podcasters to interact with their audience, and for listeners to interact with podcasters.  
Thanks to them listeners can comment, share or like podcast episodes.  
The purpose of this specification is to allow podcast apps to know where they should guide the listeners to make these interactions - and the onboarding process
necessary to make them possible - as seamless as possible.

There are three elements:
- **"podcast:social"** for the \<channel> element: tells the user **which platform(s)** is/are used for this podcast.
- **"podcast:socialSignUp"** for the \<podcast:social> element: tells the user **where to sign up** on this platform.
- **"podcast:socialInteract"** for the \<item> element: tells the user where to find the specific link to **interact with this specific episode**.

## Elements

### Social Element

- **\<podcast:social platform="[platform_id]" podcastAccountId="[podcast_account_id]" podcastAccountUrl="[podcast_account_url]" priority="[platform_priority]">**[one or more "podcast:socialPlatform" elements]**\</podcast:social>**

   Channel (optional | multiple)

   This element allows a podcaster to specify one or many platform where listeners can interact.
   There may be several occurences of this tag for the same element (on several platforms, the podcast may have several accounts on the same plaforms…)

   - `platform` (required): This is the platform id. It can be on of the following:
     - activitypub
     - facebook
     - twitter
     - instagram
     - slack
     - discord
     - jabber
     - irc
     - matrix
     - …
   - `podcastAccountId` (required): The podcast ID on this platform.
   - `podcastAccountUrl` (required): The podcast URL on this platform.
   - `priority` (optional): This platform priority (useful if the podcaster wants to tell which platform is prefered, lower is better)
 
   Examples:
   - `<podcast:social platform="twitter" podcastAccountId="@PodcastindexOrg" podcastAccountUrl="https://twitter.com/PodcastindexOrg"></podcast:social>`
   - `<podcast:social platform="activitypub" podcastAccountId="@podcastindex@noagendasocial.com" podcastAccountUrl="https://noagendasocial.com/@podcastindex"></podcast:social>`

### SocialSignUp Element

- **\<podcast:socialSignUp homeUrl="[home_url]" signUpUrl="sign_up_url" priority="[platform_priority]" />**

  podcast:social (optional | multiple)

  This element allows to make easy onboarding for listeners on social/discussion platforms, especially for decentralized ones (such as Matrix or ActivityPub) where podcasters and listeners can register on different servers.

   - `homeUrl` (required): The platform home URL.
   - `signUpUrl` (required): The platform sign up URL.
   - `priority` (optional): This platform priority (useful if the podcaster wants to tell which platform is prefered, lower is better)

  Examples:
  - `<podcast:socialSignUp homeUrl="https://twitter.com/" signUpUrl="https://twitter.com/login" priority="1" />`
  - `<podcast:socialSignUp homeUrl="https://podcastindex.social/public" signUpUrl="https://podcastindex.social/auth/sign_up" priority="2" />`

### SocialInteract Element

- **\<podcast:socialInteract platform="platform_id" podcastAccountId="podcast_account_id" priority="platform_priority">**[URL to this episode on this platform]**</podcast:social>**

  Item (optional | multiple)
  
  This element allows listeners to interact (comment, share, like…) with an episode.

  - `platform` (required): This is the platform id. It can be on of the following:
       - activitypub
       - facebook
       - twitter
       - instagram
       - slack
       - discord
       - jabber
       - irc
       - matrix
       - …
   - `podcastAccountId` (required): The podcast ID on this platform.
   - `priority` (optional): This platform priority (useful if the podcaster wants to tell which platform is prefered, lower is better)
   - element's content: URL to this episode on this platform

  Examples:
  - `<podcast:socialInteract platform="twitter" podcastAccountId="@Podverse" priority="2">https://twitter.com/Podverse/status/1375624446296395781<podcast:socialInteract>`

## Full RSS feed example

```
<?xml version="1.0" encoding="UTF-8"?>
<rss xmlns:itunes="http://www.itunes.com/dtds/podcast-1.0.dtd" xmlns:podcast="https://github.com/Podcastindex-org/podcast-namespace/blob/main/docs/1.0.md" xmlns:content="http://purl.org/rss/1.0/modules/content/" version="2.0">
  <channel>
    <atom:link xmlns:atom="http://www.w3.org/2005/Atom" href="https://lespoesiesdheloise.fr/@heloise/feed.xml" rel="self" type="application/rss+xml"/>
    <title>Les Poésies d’Héloïse</title>
    […]
    
    <podcast:social priority="1" platform="activitypub" podcastAccountId="@heloise@lespoesiesdheloise.fr" podcastAccountUrl="https://lespoesiesdheloise.fr/@heloise">
      <podcast:socialSignUp priority="1" homeUrl="https://enfants-et-famille.podcasts.chat/public" signUpUrl="https://enfants-et-famille.podcasts.chat/auth/sign_up" />
      <podcast:socialSignUp priority="2" homeUrl="https://mamot.fr/public" signUpUrl="https://mamot.fr/auth/sign_up" />
      <podcast:socialSignUp priority="3" homeUrl="https://podcastindex.social/public" signUpUrl="https://podcastindex.social/auth/sign_up" />
    </podcast:social>
    <podcast:social priority="666" platform="facebook" podcastAccountId="LesPoesiesDHeloise" podcastAccountUrl="https://www.facebook.com/LesPoesiesDHeloise">
      <podcast:socialSignUp homeUrl="https://www.facebook.com/" signUpUrl="https://www.facebook.com/r.php?display=page" />
    </podcast:social>
    
    <item>
      <title>Oisillon bleu</title>
      <enclosure url="https://lespoesiesdheloise.fr/audio/AQAAAFoAAAC4gSwAnlI2AEwAAABk.q1c/podcasts/heloise/oisillon-bleu.mp3" length="3560094" type="audio/mpeg"/>
      <guid>https://lespoesiesdheloise.fr/episode_2019-04-10_lpdh_oisillonbleu</guid>
      <pubDate>Wed, 10 Apr 2019 14:15:00 +0000</pubDate>
      <link>https://lespoesiesdheloise.fr/@heloise/episodes/oisillon-bleu</link>
      […]

      <podcast:socialInteract priority="1" platform="activitypub" podcastAccountId="@heloise@lespoesiesdheloise.fr">https://lespoesiesdheloise.fr/@heloise/notes/4ba8df51-d67d-405d-a475-6471e1235c1c</podcast:socialInteract>
      <podcast:socialInteract priority="666" platform="facebook" podcastAccountId="LesPoesiesDHeloise">https://www.facebook.com/LesPoesiesDHeloise/posts/399766303947452</podcast:socialInteract>

    </item>
    […]
    
  </channel>
</rss>
```

Discussion here:
- https://github.com/Podcastindex-org/podcast-namespace/issues/153
