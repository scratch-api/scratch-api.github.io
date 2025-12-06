# GET [`/news`](https://api.scratch.mit.edu/news)

??? "Query params"

    `offset`: How many posts ago to start

    `limit`: The number of posts to return

Returns the 'Scratch News' section of the homepage as JSON

Example response:

```json
[
  {
    "id": 724656224564543500,
    "stamp": "2023-08-03T18:07:09.000Z",
    "headline": "Become a Scratch Member!",
    "url": "https://scratch.mit.edu/membership",
    "image": "https://64.media.tumblr.com/b6a6e61377317334739f8fd744022e20/22145c27fae5e44b-22/s540x810/e373b72fab8cfc76249bd762b302ab389a3b4e10.png",
    "copy": "Scratch Members help keep Scratch free for everyone. Members get access to special sprites, events, and more as a small thank you for their support! Learn more."
  },
  {
    "id": 724366034063261700,
    "stamp": "2023-07-31T13:14:42.000Z",
    "headline": "New Scratch Design Studio!",
    "url": "https://scratch.mit.edu/studios/51081744/",
    "image": "https://64.media.tumblr.com/b88ede539189dba2642eff1d1c3644ab/7a2a86982ad9bbcc-57/s540x810/faa64fbf6cce2e4b922b6ee0a5ee2473fe7f87c8.png",
    "copy": "This new Scratch Design Studio invites you to get to know the Scratch mascot, Gobo, a bit better by making a project related to them!"
  },
  {
    "id": 723926401680621600,
    "stamp": "2023-07-26T16:46:56.000Z",
    "headline": "Hour of AI 2025",
    "url": "https://scratch.mit.edu/discuss/topic/852058/",
    "image": "https://64.media.tumblr.com/9a68a4b4b0a7d129dfa331eaf0b1f6c7/54244a45d3eba71d-83/s540x810/3a1bdfe416260f0c3509221696bbe4cddacbda84.png",
    "copy": "CSforALL wants to help learners everywhere take their first step into understanding and creating with AI. Check out Scratch’s activity and learn more here!"
  },
  {
    "id": 723731814065340400,
    "stamp": "2023-07-24T13:14:03.000Z",
    "headline": "Scratch Camp is here!",
    "url": "https://scratch.mit.edu/studios/33647149",
    "image": "https://64.media.tumblr.com/b88ede539189dba2642eff1d1c3644ab/c8c2380189725eb8-96/s540x810/86f5653337372b815f3e21079cc5f5aefefcbcd9.png",
    "copy": "The first week of Scratch Camp has officially arrived and you’re invited! Join us as we take to the stars. See here for more information!"
  },
  {
    "id": 721384728397234200,
    "stamp": "2023-06-28T15:28:07.000Z",
    "headline": "Color Contrast is here!",
    "url": "https://scratch.mit.edu/discuss/topic/694186/",
    "image": "https://64.media.tumblr.com/c83273b16d28ab82f5c0b34b23346115/42c260bdeddbf11c-d7/s540x810/288a77cd9842370a2506a14f330561337b6d7b26.png",
    "copy": "We’re excited to share our color contrast updates have officially released! See here to learn more."
  },
  {
    "id": 721379485099982800,
    "stamp": "2023-06-28T14:04:47.000Z",
    "headline": "Wiki Wednesday!",
    "url": "https://scratch.mit.edu/discuss/topic/694202/",
    "image": "https://64.media.tumblr.com/9a68a4b4b0a7d129dfa331eaf0b1f6c7/d40ab85041ec8bb2-5b/s540x810/de156bdc284c5466edc439ee323c6a77b6d488ed.png",
    "copy": "Check out the new Wiki Wednesday forum post, a news series highlighting the Scratch Wiki!"
  },
  {
    "id": 720654382479360000,
    "stamp": "2023-06-20T13:59:35.000Z",
    "headline": "New Scratch Design Studio!",
    "url": "https://scratch.mit.edu/studios/33516071",
    "image": "https://64.media.tumblr.com/b88ede539189dba2642eff1d1c3644ab/1ab7bccb9f3e8afb-8a/s540x810/429e7791e133d4a65ebe71db4a2aeea75c1aec43.png",
    "copy": "From trees, to endangered animal species, to flowing rivers, and wild berries, this SDS invites you to create a project about the woods. See here for more information."
  },
  {
    "id": 718930058046259200,
    "stamp": "2023-06-01T13:12:11.000Z",
    "headline": "Happy Pride Month!",
    "url": "https://scratch.mit.edu/discuss/topic/688848/",
    "image": "https://64.media.tumblr.com/c83273b16d28ab82f5c0b34b23346115/ac7af0cb74abc4bb-f9/s540x810/c9103e513d0c409a3616f63845e020f188fc8f26.png",
    "copy": "Happy Pride Month, everyone! We hope you enjoy celebrating and look forward to seeing what you might create in recognition of the month."
  },
  {
    "id": 718929980237676500,
    "stamp": "2023-06-01T13:10:57.000Z",
    "headline": "Wiki Wednesday!",
    "url": "https://scratch.mit.edu/discuss/topic/688849/",
    "image": "https://64.media.tumblr.com/9a68a4b4b0a7d129dfa331eaf0b1f6c7/dc5dd05b3cea8352-a3/s540x810/9a8160c80b3402f0932650ebc2ec7d352cbf7f9c.png",
    "copy": "Check out the new Wiki Wednesday forum post, a news series highlighting the Scratch Wiki!"
  },
  {
    "id": 716329614277066800,
    "stamp": "2023-05-03T20:19:15.000Z",
    "headline": "New Scratch Design Studio!",
    "url": "https://scratch.mit.edu/studios/33288318",
    "image": "https://64.media.tumblr.com/b88ede539189dba2642eff1d1c3644ab/4e1e501ba96b00a2-32/s540x810/885c784a15122d9e67ba8bb15c76a8bad7d654db.png",
    "copy": "Abracadabra! In this Scratch Design Studio, we’re diving into the world of magic tricks and you’re invited!"
  },
  {
    "id": 713214585544605700,
    "stamp": "2023-03-30T11:07:12.000Z",
    "headline": "Wiki Wednesday",
    "url": "https://scratch.mit.edu/discuss/topic/680451/",
    "image": "https://64.media.tumblr.com/9a68a4b4b0a7d129dfa331eaf0b1f6c7/34ca3856b703e8e2-88/s540x810/9b848a214f0fc00145f40e2a345d61f6440b8eaa.png",
    "copy": "Check out the new Wiki Wednesday forum post, a news series highlighting the Scratch Wiki!"
  },
  {
    "id": 711684772803510300,
    "stamp": "2023-03-13T13:51:29.000Z",
    "headline": "New Scratch Design Studio!",
    "url": "https://scratch.mit.edu/studios/32985694",
    "image": "https://64.media.tumblr.com/b88ede539189dba2642eff1d1c3644ab/9b856d63b7b74208-52/s540x810/280744d465fd1985941d76c0856075d06ec09efe.png",
    "copy": "From bizarre dreams, to ones that are funny and make no sense, dreams can be a great source of inspiration. This Scratch Design Studio invites you to create a project about dreams!"
  },
  {
    "id": 710793701354537000,
    "stamp": "2023-03-03T17:48:17.000Z",
    "headline": "Maintenance Work",
    "url": "https://scratch.mit.edu/discuss/topic/667012/",
    "image": "https://64.media.tumblr.com/c83273b16d28ab82f5c0b34b23346115/c9329c8e01f4f8fe-dc/s540x810/74d6fd926b83c9c47314b7ac207e961b5f8e6389.png",
    "copy": "We are doing some maintenance work that will impact the homepage of Scratch. See here for more information."
  },
  {
    "id": 710146771937083400,
    "stamp": "2023-02-24T14:25:37.000Z",
    "headline": "Wiki Wednesday!",
    "url": "https://scratch.mit.edu/discuss/topic/665327/",
    "image": "https://64.media.tumblr.com/9a68a4b4b0a7d129dfa331eaf0b1f6c7/53cb42a61b5b6bae-f5/s540x810/96d90d74c763986b2f6a7cad48846bca5308dbb2.png",
    "copy": "Check out the new Wiki Wednesday forum post, a news series highlighting the Scratch Wiki!"
  },
  {
    "id": 707603663086026800,
    "stamp": "2023-01-27T12:43:59.000Z",
    "headline": "New Scratch Design Studio!",
    "url": "https://scratch.mit.edu/studios/32745737",
    "image": "https://64.media.tumblr.com/b88ede539189dba2642eff1d1c3644ab/ee6ac9aa614da60f-14/s540x810/4c340334de694c8288b57e49f5bc7ef2c741d3e8.png",
    "copy": "From furniture learning to dance to water bottles making friends, this Scratch Design Studio invites you to explore the possibilities of inanimate objects coming to life!"
  },
  {
    "id": 707595162497531900,
    "stamp": "2023-01-27T10:28:52.000Z",
    "headline": "Wiki Wednesday!",
    "url": "https://scratch.mit.edu/discuss/topic/658664/",
    "image": "https://64.media.tumblr.com/9a68a4b4b0a7d129dfa331eaf0b1f6c7/5fee7e18bea6e2a1-85/s540x810/1a7814c99445db70be8cb0e9867edc5b1b3b217b.png",
    "copy": "Check out the new Wiki Wednesday forum post, a news series highlighting the Scratch Wiki!"
  },
  {
    "id": 706794214677184500,
    "stamp": "2023-01-18T14:18:09.000Z",
    "headline": "Activity Swap! 2023 Show-and-Tell",
    "url": "https://scratch.mit.edu/studios/32721811",
    "image": "https://64.media.tumblr.com/c83273b16d28ab82f5c0b34b23346115/7766ba05477f70e2-ff/s540x810/f02a184a10e01baa807f7a3f8db1a2ec98347756.png",
    "copy": "Week two of Activity Swap has officially begun, and this week is show-and-tell! See the studio for more information…"
  },
  {
    "id": 704895751312064500,
    "stamp": "2022-12-28T15:22:53.000Z",
    "headline": "Wiki Wednesday!",
    "url": "https://scratch.mit.edu/discuss/topic/652266/",
    "image": "https://64.media.tumblr.com/9a68a4b4b0a7d129dfa331eaf0b1f6c7/92681075590f51d7-bc/s540x810/fb89ca10938c447fe84e58e7c3a50a566b033158.png",
    "copy": "Check out the new Wiki Wednesday forum post, a news series highlighting the Scratch Wiki!"
  },
  {
    "id": 704074245909758000,
    "stamp": "2022-12-19T13:45:25.000Z",
    "headline": "New Scratch Design Studio!",
    "url": "https://scratch.mit.edu/studios/32552757",
    "image": "https://64.media.tumblr.com/b88ede539189dba2642eff1d1c3644ab/bc46daa788638fb1-a4/s540x810/37b7379e19ce0a3eccef385db3ce8c42c4f475e0.png",
    "copy": "From the discovery of water on a planet to a new star or cure of a disease, this Scratch Design Studio invites you to create projects around scientific discoveries!"
  },
  {
    "id": 702992144847224800,
    "stamp": "2022-12-07T15:05:53.000Z",
    "headline": "2022: A Scratch Year in Review!",
    "url": "https://scratch.mit.edu/studios/32526281",
    "image": "https://64.media.tumblr.com/c83273b16d28ab82f5c0b34b23346115/eac1b9b166ce26a5-2e/s540x810/636950e7c31876151a609f85f66934b159cdf0f7.png",
    "copy": "2022 is coming to an end and we invite you to create a project to share moments from the year you would like to remember. See the studio for more information!"
  }
]
```