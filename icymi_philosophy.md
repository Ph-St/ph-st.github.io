# ICYMI (Philosophy)

## Documentation

This is a description of the [Mastodon](https://joinmastodon.org) bot @icymi_philosophy@botsin.space. The bot aims to aid discovery within the philosophy community on mastodon. It is currently in an early stage of development. I took over many ideas from [@icymi_law@esq.social](https://icymilaw.org/).

Here is a list of the principal steps the bot runs through every hour:

1. It reads in a list of all accounts it is following. 
2. It bot reads in the list of philosophers on Mastodon maintained by [Cédric Eyssette](https://eyssette.github.io/Mastodon-Philosophy/).
3. It bot combines the lists (I refer to this combines list as the bot's FOLLOWEES).
4. It bot reads in its home timeline of the last 24 hours (or max. 400 posts, which is currently the maximum timeline buffer)
5. It bot removes all it's own posts and boosts from the timeline.
6. It bot removes all posts and boosts by accounts that have either "nobot" or "noindex" in their bio.
7. It separates all posts into two lists: (a) Posts and (b) Boosts. Posts that it received by following the Hashtag #philosophy go with the boosts.
8. It follows every post in the Post-set (a) back to the originating server and counts reblogs, favourites, and replies.
9. It follows every post in the Reblog-set (a) back to the originating server and counts reblogs, favourites, and replies.
10. If the reblog or favourite comes from a FOLLOWEE account, it's being counted twice.
11. It stores 7 posts in a database:
    - (1) The two post in the Post-set with most reblogs plus favourites, if two posts have the same counts, the post with more reblogs is selected. 
    - (2) The post in the Post-set with most replies. The bot also blacklists the entire context of this post so that the same thread isn't boosted multiple times.
    - (3) The post in the Post-set with most favourites. 
    - (4) The post in the Boost-set with most reblogs plus favourites, if two posts have the same counts, the post with more reblogs is selected. 
    - (5) The post in the Boost-set with most reblogs by FOLLOWEES.
    - (6) The post in the Boost-set with most favourites by FOLLOWEES.
12. Every 8 minutes, the bot boosts one of those 7 selected posts and removes it from the database.


## TODO

1. Introduce a weighting mechanism such that accounts with many followers are counted less.
2. Split the 24 hours haul into two chunks in order to circumvent the 400 post limit.
3. Distinguish long threads from posts with many replies.
4. Automatically add everyone on the fediphilosophy instance to the FOLLOWEEs. Currently not possible due to API version of that instance.
5. Check posts for the most common links and post a list of those links once per day.
6. 
