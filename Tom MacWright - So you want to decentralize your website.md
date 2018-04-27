# So you want to decentralize your website

<div class="body __reader_view_article_wrap_622066308103491__">

Blogging in 2017 is absurd: most people don’t own websites anymore. Microblogging might be still in, video blogging might be the last-next frontier, and maybe tinyletter will stay underground-cool. It’s pretty weird to run your own website, to care about [making it efficient](https://macwright.org/2016/05/03/the-featherweight-website.html), to keep using last-gen tech like [Jekyll](https://jekyllrb.com/).

![](./So you want to decentralize your website - macwright.org_files/2017-07-20-decentralize-your-website-1.jpg)

So, if it’s going to be weird, we might as well go whole-hog and become decentralization hipsters. Decentralizing the web, or at least avoiding the monopoly-fueled monoculture of the web, has been around forever - projects like [WASTE](https://en.wikipedia.org/wiki/WASTE) and [Tor](https://en.wikipedia.org/wiki/Tor_(anonymity_network)) hidden services have been around for _forever_, but a popular, daily-usable, semi-mainstream alternative to the internet has been elusive and in the period of 2009-2013ish was overshadowed by other trends.

But now in 2017, a re-decentralized web - ‘dex’ for short - is back in vogue and there are some promising projects. There are lots of projects in the space now, with slightly different goals and cultures, like [scuttlebutt](http://scuttlebot.io/), [IPFS](https://ipfs.io/), and [Dat](http://datproject.org/). And, critically, [Beaker Browser](https://beakerbrowser.com/) just launched, a user-friendly browser that transparently supports Dat websites: you can go to `dat://` URLs just like you can `https://` URLs. I’ve been trying it out, and it’s great.

I’m no decentralized-services expert. Dat and other projects are based on deep observations about network topology and cryptography that I’m only barely starting to understand. So, a lot like my [data science scratchpad](https://macwright.org/2017/06/01/data-science-scratchpad.html), this is a personal journal and space for loose notes than it is a guide or recommendation: I think folks are starting to tinker with this tech, and we’re all figuring out what’s great, what might be unfinished, and where it’s going.

## Intent & aim

I’ve got macwright.org, why also have a dex website? First of all, for fun. And dex also has the potential to remove moving parts: right now when I publish my website with [Netlify](https://macwright.org/2017/05/08/https.html), it relies on GitHub for code hosting, Netlify for servers, Let’s Encrypt for the SSL certificate, and on the services under those services, like AWS and, hopefully, nothing crazy.

A dex website can just be you, your computer, and the world. Instead of needing to support HTTPS - which GitHub Pages still doesn’t do with custom domain names, and instead of worrying about CDN performance, you can hope that the system does those things correctly, from the ground up.

## Starting

I’m starting by trying to host a site on **Dat** with Beaker Browser and/or the Dat client. Beaker Browser [doesn’t support IPFS](https://github.com/beakerbrowser/beaker/issues/480) yet. I also like IPFS, and so does [Kyle at Neocities](https://github.com/neocities/neocities/issues/246#issuecomment-304742893), who knows a lot more about it. Ideally I’d support both, but I have to start with one.

Major kudos to the community around this for their classy and humane discussions, like the one linked above.

## Summary of how a plain-vanilla website works

Since this is a _leap beyond_ current websites, we should have a reference point: current websites. Or, to simplify, this website. macwright.org takes this path from me to you, the reader:

1.  I have a directory, `~/src/tmcw.github.com` in which I edit files like this one. It is a git repository as well as a directory - it contains a `.git` database and can be ‘pushed’ to other git repositories.
2.  When I’ve decided that a post is absolutely perfect, I push a commit in the `master` branch of that git repository from my computer to GitHub, to the `tmcw/tmcw.github.com` repository.
3.  GitHub sends a notification of that pushed commit to [Netlify, my web host](https://macwright.org/2017/05/08/https.html). Netlify clones the repository, runs Jekyll to build Markdown files into HTML, and keeps a copy of that built site (HTML & CSS) on their origin servers.
4.  Netlify has a lot of servers that form a Content Distribution Network that handle the final step from their origin servers to you. Content Distribution Networks hold copies of this website on servers that are numerous and located all over the world, so that responses are semi-uniformly fast and floods of traffic don’t have a single point of failure.

![The original web publishing workflow](./So you want to decentralize your website - macwright.org_files/2017-07-20-decentralize-your-website-the-original-web-publishing-workflow.jpg)

Vital notes on this workflow:

*   My computer, a 2017 MacBook Pro, doesn’t do much. A lot of the time it’s offline, or out of batteries. I replace it about every 2 years and install as few programs as I can on it.
*   The ‘keys to the castle’, so to speak, are my [RSA keys](https://github.com/tmcw.keys), the things that let me push commits to GitHub. If I add someone new to the repository, they have access to publish. If I remove or change a key, I remove or change access.
*   There are a bunch of moving parts. If GitHub, Netlify, or one of their constituent services goes down, it could be bad. I’d still have at least one copy of my site that I could re-publish anywhere, but nevertheless the whole system requires trust.

## How a Dat website works: you are the first host

The first big difference between Dat and a ‘normal website’ is that your personal computer typically plays a much larger role: **you are a host**.

Running your own web host used to be more common in the early days of the internet, when desktop computers were commonplace and ISPs were relatively unsophisticated. Nowadays, it’s not so easy, or recommended. ISPs frown upon local hosting, and given the incredible guarantees of uptime from commercial providers like [AWS](https://aws.amazon.com/), hosting it yourself simply doesn’t make a lot of sense.

Dat relieves the ‘computer turns off’ issue to some degree, by saying that, if your website is popular, you can turn your computer off and it’ll keep working based on the momentum of people who had use the Dat before and were thus hosting it now. Networking-wise, it’s still not foolproof - it’s able to host a Dat with my home Comcast connection, but tethered to my iPhone, I’m behind a [symmetric NAT](https://en.wikipedia.org/wiki/Network_address_translation#Methods_of_translation) and thus would have to try some more tricks.

This problem of having a _reliable host_ for your Dat data has a bunch of competing solutions: you could [build a home server](https://github.com/datproject/datasilo), or you could use [hashbase.io](https://hashbase.io/), a new service that basically acts as a persistent mirror of a website that you initially host.

## How a Dat website works: it’s kind of confusing where the keys to the castle are

![Security. I totally reused this graphic from a post in 2012.](./So you want to decentralize your website - macwright.org_files/2017-07-20-decentralize-your-website-security-i-totally-reused-this-graphic-from-a-post-in-2012.jpg)

_This is something that will probably be fixed pretty soon, I hope or think. I’ve mentioned it to the Beaker team already. I’ll update this article as things change. If you are the person changing things and want me to update, hit me up on twitter at @tmcw._

So, with a typical website you gain the ability to publish via one of the following:

*   DSA or RSA public keys that let you push commits to GitHub or another git repository
*   Access tokens in AWS or another service, or temporary 2FA tokens
*   A username/password combination if you’re using SFTP like it’s 2001

Dat & Beaker both have plenty of documentation for _how to publish_, but not very much in terms of how to continue publishing. If your computer explodes, how do you publish your hot new blog post?

You’ll find part of the answer in, and only in, Dat’s [terminology docs](https://docs.datproject.org/terms), which refer to secret keys stored in a folder. If you crack that folder open, you might be surprised to learn that, unlike DSA/RSA secret keys, Dat secret keys are one-per-dat, rather than one-per-user.

Beaker also tiptoes around the question of access: it says that [a secret key is generated when you host a site](https://beakerbrowser.com/docs/inside-beaker/faq.html#who-is-allowed-to-change-a-dat-site), but not where. It turns out that the secret key lives in `~/Library/Application Support/Beaker Browser/`, at least for my system.

There’s a pretty good reason why both Dat and Beaker are sketchy about describing keys: right now, you can kind of break it if you try to write to the same Dat from two computers. So, if you were to share your secret keys, you could make bad things happen. Which, I totally _get_ - Dat is a work-in-progress and simultaneous writing is just one of the legions of issues they’re dealing with.

But, as a system built around the strong and impressive guarantee that updates are cryptographically safe, that if you don’t have the key you can’t update the site, it gives me significant anxiety to think of the situation in which I’m locked out forever because I didn’t know where the key that was enabling my access lived, and thus didn’t back it up. Will uninstalling Beaker also take away my publishing privileges forever? It’s not a great situation.

It’s also kind of a weird situation with Beaker & Dat: though Beaker makes it drag-and-drop easy to host a site with Dat, it doesn’t advertise any way to start hosting it anywhere else, or any other way: it has its own Dat keys and doesn’t use `.dat/secret_keys`, so the Dat CLI client won’t work.

## How a Dat website works: the strict CSP rules

Okay, so let’s say you have a website hosted with Dat. If you happen to be using Beaker Browser right now, here’s a link to a page on macwright.org, hosted on Dat and mirrored on hashbase:

[dat://tmcw.hashbase.io/2017/07/01/recently.html](dat://tmcw.hashbase.io/2017/07/01/recently.html)

It’s my blog post about a trip to Cuba: [here it is on HTTPS](https://macwright.org/2017/07/01/recently.html). You’ll notice that the version hosted on Dat doesn’t have any images or embeds available. That’s by design: Beaker Browser enforces a strict [Content Security Policy](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/Content_Security_Policy):

<div class="highlighter-rouge">

    default-src 'self' dat: blob:;
    script-src 'self' 'unsafe-eval' 'unsafe-inline' dat: blob:;
    style-src 'self' 'unsafe-inline' dat: blob:;
    img-src 'self' data: dat:  blob:;
    font-src 'self' data: dat:  blob:;
    media-src 'self' dat:  blob:;
    connect-src 'self' dat:;
    object-src 'none';

</div>

So, pages like my blog posts tend to load other resources. As few as possible, of course, but photos are from Flickr, audio embeds are from Bandcamp, and I use gaug.es for web analytics. All of these are loaded over HTTPS URLs, and all are verboten according to the CSP policy in Beaker.

This is part of the design, and totally justifiable. When you visit a Dat website, it should obey the basic guarantees of Dat, and loading an HTTPS URL opens you up to the wild world of HTTPS, which can change at any time and won’t always be available when Dat is. But it’s a pretty strict policy, and it’s hard to think of a workaround that’d be good for my usecase - I don’t want to host all my own photos, and I can’t legally copy all the music I embed on this site.

## How macwright.org on Dat (hopefully) works

1.  I have a directory, `~/src/tmcw.github.com` in which I edit files like this one. It is a git repository as well as a directory - it contains a `.git` database and can be ‘pushed’ to other git repositories.
2.  Locally, I run Jekyll to build Markdown files into HTML, and keep a copy of that built site (HTML & CSS) locally. This builds the `_site` directory.
3.  I run `dat share` locally, to run a Dat server.
4.  [Hashbase](https://hashbase.io/) ‘peers’ with me, mirroring the content I’m serving.
5.  I can eventually turn off my computer and hope that everything continues working.
6.  From https://macwright.org, I use [a .well-known/dat](https://github.com/beakerbrowser/beaker/wiki/Authenticated-Dat-URLs-and-HTTPS-to-Dat-Discovery) file to point Beaker Browser users from my HTTPS website to my Dat website.

Quick note: Jekyll won’t publish a file called `.well-known/dat` by default, because it excludes dotfiles by default. You can include the file by adding it to the include entry in `_config.yml`:

<div class="highlighter-rouge">

    include: [".well-known"]

</div>

![Dat and hashbase flow](./So you want to decentralize your website - macwright.org_files/2017-07-20-decentralize-your-website-dat-and-hashbase-flow.jpg)

## Reflections

**I think Beaker Browser’s entry into this space is a big deal.** The project has been really user-centric and has made a lot of progress, quickly, in terms of baking decentralized technology into software that everyone can use.

Even more importantly, Beaker Browser is a _coherent entry point_ for the Dat ecosystem, which is something that the [Dat project homepage](https://datproject.org/) doesn’t do, and really _can’t_ do. If you pull up datproject.org, you’re faced with a PDF whitepaper, a desktop application for downloading datasets (but not browsing Dat websites), a terminal tool, as well as HTTP mirrors of Dat datasets that only, in my opinion, make the concept of Dat harder to understand.

Dat has, variously, been a database-like row-by-row storage solution, a Dropbox-like file sharing system, and a system for websites. Compared to IPFS, for instance, hosting websites is at best the third-highest priority for the Dat project. It makes sense that IPFS is the choice of NeoCities: IPFS is pretty clearly intended to be a replacement for the internet.

If your entry point to Dat is through datproject.org, you’d first hear the Dropbox usecase:

> Ever tried moving large files and folders to other computers? Usually this involves one of a few strategies: being in the same location (usb stick), using a cloud service (Dropbox), or using old but reliable technical tools (rsync). None of these easily store, track, and share your data securely over time. People often are stuck choosing between security, speed, or ease of use. Dat provides all three by using a state of the art technical foundation and user friendly tools for fast and secure file sharing that you control.

Dat’s documentation, even to an experienced internet person like myself, can be a struggle. In some ways, this is because it doesn’t _weight_ decisions: you _can_ start out using Dat via a Node.js library, a desktop client, a CLI, an HTTPS gateway, or Beaker Browser, but which one _should_ you start out with?

Similarly, the homepage hotlinks the [PDF whitepaper](https://datproject.org/paper) describing the protocol. It’s well-written paper, and it’s a beautiful protocol, but it will terrify anyone without a computer science education:

> Every time the tree is updated we sign the current roots of the Merkle tree, and append them to the signatures file. The signatures file starts with no entries. Each time a new leaf is appended to the tree file (aka whenever data is added to a Dat), we take all root hashes at the current state of the Merkle tree and hash and sign them, then append them as a new entry to the signatures file.

Dat seems like one of those websites where they could hit you with a question: are you a programmer? Do you want to use Dat to distribute your own data? Are you a researcher? That way you can tailor the pitch to the audience.

## Dat is incredibly ambitious and it already delivers on many of its promises

![Grand promises of the future](./So you want to decentralize your website - macwright.org_files/2017-07-20-decentralize-your-website-grand-promises-of-the-future.jpg)

I’ve been in the open source space long enough to see many, many what-ifs about distributed systems. What if OpenStreetMap was hosted on Bittorrent, or a community of peer-to-peer CDNs? What if we created a distributed version of GitHub? Of npm? Of Facebook?

None of these what-ifs materialized. Sure, there are many attempts, but none of them own more than 1% marketshare, none of them became accessible to anyone but the tech elite. Distributed, decentralized systems don’t just fail because of the tech-oligopoly conspiracy: they fail because decentralization is incredibly, incredibly hard. And, while you’re struggling to conquer PhD-worthy Computer Science problems, you’ve got to grapple with _unfamiliarity_, the single hardest problem in usability.

The Dat strategy reflects this challenge. It isn’t a personal project or a startup with a 4-year window of success, it’s a foundation-supported project with core parts owned by multiple contributors. This is a long haul, and if you’re creating network protocols people will rely on for critical data, you need to get it right.

I think that the _combination_ of Dat and Beaker Browser (or IPFS and Beaker Browser) is what matters most. Beaker makes it possible to potentially use these technologies _without knowing that you’re using them_: that’s powerful. And there’s real value of having a stakeholder in the dex community that’s so user-focused.

## Next’ish: IPFS, my own host

_✨ Update: [read about decentralization using IPFS](https://macwright.org/2017/08/09/decentralize-ipfs.html) ✨_

*   I’m going to try out IPFS next, and see if I can get macwright.org going on another alt-internet.
*   Also, I swear I’m going to finally use that [Raspberry Pi](https://www.raspberrypi.org/) that’s been collecting dust in my drawer to power a local mirror for dat. It seems like the perfect task for a Pi: requires internet access, but not tons of compute power or bandwidth, hopefully.

I hope this is useful to someone also diving into dex web, or folks trying to build better explanations of it. Nothing but 💕 fuzzy warm vibes to the folks working on this technology: they’ve been building something great, and, as far as I’ve seen, fostered a great culture around decentralized tech. It’s certainly a work-in-progress, and I’m eager to follow progress as some of these kinks get ironed out.

Also, some of my experiences here may be simply mistakes or misunderstandings on my part. If so, absolutely let me know: my email’s on the [/about page](https://macwright.org/about/).

</div>

<div class="pad2y">

<postamble datetime="2017-07-20">July 20, 2017</postamble> [@tmcw](https://twitter.com/intent/follow?screen_name=tmcw&user_id=1458271 "Follow me on Twitter")

</div>

</div>
