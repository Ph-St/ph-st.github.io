# ICYMI (Philosophy)

## Documentation

This is a description of the [Mastodon](https://joinmastodon.org) bot @icymi_philosophy@botsin.space. The bot aims to aid discovery within the philosophy community on mastodon. It is currently in an early stage of development and written in Python using the [mastodon.py](https://github.com/halcy/Mastodon.py) wrapper. I took over many ideas from [@icymi_law@esq.social](https://icymilaw.org/).

Here is a list of the principal steps the bot runs through every two hours:

1. It reads in a list of all accounts it is following. 
2. It combines the lists (I refer to this combines list as the bot's FOLLOWEES).
3. It reads in its home timeline of the last 24 hours (or max. 400 posts, which is currently the maximum timeline buffer)
4. It removes all it's own posts and boosts from the timeline.
5. It removes all posts and boosts by accounts that have either "nobot" or "noindex" in their bio.
6. It separates all posts into two lists: (a) Posts and (b) Boosts. Posts that it received by following the Hashtag #philosophy go with the boosts.
7. It follows every post in the Post-set (a) back to the originating server and counts reblogs, favourites, and replies.
8. It follows every post in the Reblog-set (a) back to the originating server and counts reblogs, favourites, and replies.
9. If the reblog or favourite comes from a FOLLOWEE account, it's being counted twice.
10. It stores 12 posts in a database:
    - (1) Four posts in the Post-set with most reblogs plus favourites, if two posts have the same counts, the post with more reblogs is selected. 
    - (2) The post in the Post-set with most replies. The bot also blacklists the entire context of this post so that the same thread isn't boosted multiple times.
    - (3) The post in the Post-set with most favourites. 
    - (4) Two posts in the Boost-set with most reblogs plus favourites, if two posts have the same counts, the post with more reblogs is selected. 
    - (5) Two posts in the Boost-set with most reblogs by FOLLOWEES.
    - (6) Two posts in the Boost-set with most favourites by FOLLOWEES.
11. Every 17 minutes, the bot boosts one of those 12 selected posts and removes it from the database.
12. On Sunday, 1 PM CET the bot reads the list of philosophers on Mastodon maintained by [Cédric Eyssette](https://eyssette.github.io/Mastodon-Philosophy/) as well as the one maintained by [Kelly Truelove](https://truesciphi.org/phi_mas.html) and follows everyone from these lists it is not already following.


## TODO

1. Introduce a weighting mechanism such that accounts with many followers are counted less.
2. Split the 24 hours haul into two chunks in order to circumvent the 400 post limit.
3. Distinguish long threads from posts with many replies.
4. Automatically add everyone on the fediphilosophy instance to the FOLLOWEEs. Currently not possible due to API version of that instance.
5. Check posts for the most common links and post a list of those links once per day.
6. Publish most common hashtags
7. Respect NoBot and NoIndex info also when automatically following accounts
