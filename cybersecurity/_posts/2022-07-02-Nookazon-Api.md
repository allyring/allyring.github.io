---
layout: post
title:  "Reverse engineering the Nookazon API"
date:   2022-07-02 11:03:00 +0100
category: "Cyber-Security"
---

Recently I was looking at the Nookazon website, and was wondering if they had a public API. After a quick Google search, it turns out that Nookazon does have an REST API that returns JSON data, but that it didn't have any documentation.

That's not to say that it couldn't be used though. The API in question is used by the website to get information about items in their store, as well as other data rendered on the webpage, so I guessed it could be found by doing some digging in the 'network' tab of dev tools, and sure enough I found the endpoint at https://nookazon.com/api/public/.

Most of the API's endpoints requires you to have a Nookazon account, as it relies on an authorization token for all endpoints.

To find the authorisation token, do the following:
1. Sign in to Nookazon and open up your browser's developer tools (F12 menu).
2. Open the 'network' tab in the developer tools, then navigate to any page on Nookazon.
3. Find a request that responds with 'json' data, and open that request.
4. In the 'headers' menu of the request, open up the 'request headers' section, and look for the 'authorization' header.
5. There should be a long string of text with the word 'Bearer' in front of it; the long string of text is your authorisation token.

This token should be treated like a password, and shouldn't be posted publically online.

Of course, this token is pretty much useless without any actual endpoints to use it at. Most of the important endpoints can be found by using the 'network' tab of dev tools with the 'XHR' filter enabled while browsing the site.

Here are a few interesting ones I found:

- `GET https://nookazon.com/api/public/items` - The items endpoint, which is used for searching and retrieving item details.
- `GET https://nookazon.com/api/public/users` - The users endpoint, used for searching for users.
- `GET https://nookazon.com/api/public/accounts` - The accounts endpoint, used to get account information such as linked accounts, username, etc.
- `GET https://nookazon.com/api/public/listings`- The endpoint for getting item listings.
- `GET https://nookazon.com/api/public/items/hot` The endpoint for listing 'hot' items.

All of these endpoints have parameters you can add to filter results or get the specific results for a user etc, so here are a few common ones:

- `size`: int - defines how many results should be returned. Can have a limit, depending on the endpoint.
- `orderBy`: str - How to sort the listings; can be any of the following:  
posted-asc, posted-desc, name-asc, name-desc, active, endtime-asc, or endtime-desc (asc is ascending, desc is descending)

This certainly isn't a comprehensive list, so if someone wants to create a pastebin, or a GitHub gist with any more that you'd like me to link, feel free to email the link to me.