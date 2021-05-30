---
layout:     post
title:      "Handling Authorization When Sending FormData"
date:       2021-05-23 10:00:00 +0000
permalink:  formdata_fetch_headers
---

I recently decided to polish up a project from a couple of months ago so that I could get it deployed. When I originally set up the API, I had future users in mind and configured Devise to handle authentication. I didn’t flesh that out on the frontend though, as it wasn’t a priority at the time. So I’ve been working on getting that set up, and I ran into a slight issue.

Because my application gives users the ability to upload photos of their plants, I’m using FormData to handle those image blob files in my fetch requests. As I went about adding Authorization headers to each of my fetch requests, I hit a problem. In my fetch requests, I had to include `"Content-Type": "application/json"` in order for the request to work. How was I to indicate that the Authorization was to be processed as JSON and the FormData as a file? Setting the `Content-Type` to `application/json` meant getting the following error from the API: `Error occurred while parsing request parameters. Contents: WebKitFormBoundary...`.

![error](https://i.imgur.com/rtRfgxD.png)

This error tells me that there is trouble parsing the request, which is usually caused by a mismatch in the information that is being sent by the (in this case) `PUT` request on the client side and the way it is interpreted by the server. 

At first, I thought that I would have to find a way to essentially send two headers in the same fetch request, one for the content type of the Authorization, and one for the content type of the FormData. But of course that didn’t pan out. Then I tried making the `Content-Type` `multipart/form-data` to account for the blob that I was sending with the request, but again, no luck.

Finally, I discovered that including `Content-Type` in the header was actually unnecessary. If I didn’t include it, the request went through just fine. So then I wondered, do the `GET` requests actually need `Content-Type` to be defined? And the answer to that is yes, they do. This is because the `GET` requests are actually carrying information to the API, namely a session token. The API needs to know how to interpret this information.

So, the final solution is to include the following header in all requests that don’t handle FormData (any that are in JSON format):

![GET request](https://i.imgur.com/5DfqgxX.png)

And requests that handle FormData should use this header:

![PUT request](https://i.imgur.com/8iYqDNw.png)

If you are having a similar issue, I hope this helped you solve it! 
