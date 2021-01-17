---
layout: post
title:      "In This Case, Failure Means Success"
date:       2021-01-17 21:15:12 +0000
permalink:  in_this_case_failure_means_success
---


Despite the profundity you may expect from a blog post with that title, I'm not here to spill any philosophical tea. What I'm really after is exploring this confounding error message that I kept seeing in the Chrome developers console throughout the development of my latest app:

![X](https://imgur.com/GSeTAP5)

```
Fetch failed loading: DELETE "http://localhostL3000/care_events/30".
```


When I first saw this error, I thought, "Hmm, I guess this fetch call isn't working. Maybe I provided the wrong API endpoint?" But when I popped over to the Rails console, I found this:

![X](https://imgur.com/a/Tca7Wud)

So the item I was trying to delete was successfully destroyed. That's good news. In fact, this means that the error is insignificant and we *could* just ignore it. But I don't like errors showing up when everything is actually behaving well. I find them annoying, and if I get in the weeds with other errors, they can distract me from important information in the console.

Before I go further, I should mention that this seems to be an issue in Chrome specifically, and I can't speak to the behavior of other browsers.

Some initial Googling revealed that these messages are a default setting in Chrome, and they can be turned off. If you are in the Chrome developers console, click on the gear (console settings) and uncheck `Log XLMHttpRequests`. 

This wasn't the solution I wanted though. I like having that information available to me in case there really is an issue with a fetch request in the future. So the Googling continued. 

The first important thing I learned was that Chrome doesn't like an empty response body. When it gets an empty response from the server, such as in the event of a DELETE or PATCH request, it thinks something went wrong. Nothing will actually go wrong, but you'll end up with that pesky error. 

The issue is when Rails doesn't have a `return` in a controller action, it very kindly sends a `204 No Content` status code to the browser, as you can see in the screenshot above. That makes sense, right? There was nothing to send back, so it doesn't send anything except a descriptive status code. But Chrome still isn't a fan of the empty response, despite the elucidating status code. 

After exploring a few options, I only found one that reliably removed the erroneous code. Rendering something at the end of the `destroy` controller action. At first, I just rendered the instance represented by the instance variable that was set to be destroyed. But I didn't like the logic of this. After all, it doesn't exist anymore. And why send unnecessary information over to the client? 

After some trial and error with such things as `head` and empty JSON objects (with no success from either of them), I found what I think to be an elegantly concise solution. 

```rb
def destroy
     @care_event.destroy
     render status: :ok
end
```

This `:ok` (aka 200, aka success) status code indicates that the request has succeeded, which in this instance means that the Care Event has been deleted from the database successfully. 

And voila! No more distracting error messages in the Chrome console! Now, when an XLMHttpRequest says that it failed, I know that by failure it doesn't really mean success. 
