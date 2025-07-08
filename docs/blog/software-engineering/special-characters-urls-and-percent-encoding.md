# Special characters, URLs and %-encoding

> Originally published: 10 November 2016

Handling special characters with %-encoding is a rather...tricky thing to do.

Well, it isn't really. The actual application of it is relatively simple. You just reference the relevant
library/package and apply whatever the standard %-encoding function is to your URL.
Simple, right?

Wrong.  
The wider implementation of handling special characters when building HTTP requests is quite fiddly and often
implemented badly.  
(You can find my suggested implemention at the bottom of the page)

If you wanted to provide an interface/service/adapter/bit-of-code for the rest of your application to perform HTTP
requests, for example:    
`http://harmelodic.com/api/services/testing.html?username=nedflanders&password=howdy+neighbour`  
What do you do with your special characters?

Well, the natural thing to do would be to %-encode them, but that puts you in a tricky situation with lovely (
*horrible*), wonderful (*awful*) things called *reserved characters*.

Reserved characters can be some of the most frustrating things for people to use and/or not use when handling HTTP
requests.  
The idea of them, is that a select few characters are reserved in the HTTP standard for delimiting parts of your URL.   
They are as follows:

`!` `#` `$` `&` `'` `(` `)` `*` `+` `,` `/` `:` `;` `=` `?` `@` `[` `]`

The most used ones like `/`, `?`, `=` and `&` actually make sense as being reserved characters:  
`/` is used for delimiting when defining where a resource is (the `/api/services/testing.html` bit).  
`?` is used to define the start of the query string (the `?username=nedflanders&password=howdy+neighbour` bit)  
`&` is used to delimit each query string parameter (and its associated value) on the query string.  
`=` is used to delimit between a query string parameter and it's associated value (e.g `username=nedflanders`).

That's okay but what about something like a `+`?  `+` isn't used for any hugely important logic in a HTTP request like
`/` or `?` is. `+` is used to represent a space character. That's pretty okay I guess, you can neatly replace each space
that occurs with a `+` and you'll all dandy!  
That's really simple! There's no problems with that, yes?

Well...no.

What if you have a URL with a `+` in a query string value? That `+` could be pretty important. It could be a character
in an email address or an API key. If your 3rd party server reads that `+` symbol as a space character when it's not
supposed to be a space character, then it can screw up your data and that's something we **DO NOT** want.  
So what's the work around?

You %-encode it!

But that leads to a further problem with our initial scenario.  
We said:

> What if you wanted to provide an interface/service/adapter/bit-of-code for the rest of your applicaiton to perform
> HTTP requests.

This is a perfectly valid scenario. You want to abstract your code for performing HTTP requests from your application so
that your application uses a standard HTTP process. Moreover, abstracting this code means that you can remove any
context that may have been required in certain scenarios and instead just handle the HTTP request for the caller.

The problem that we have with this scenario, is what the definitive answer is to the following question:

What if whatever is calling your code, provided you with a URL with `+` symbol in it. Do you handle it as a space or as
a `+`?

If it's meant to be a space, then you should leave it as a `+` and the normal, underlying HTTP process will handle that
correctly.  
If it's meant to be a `+`, then you should %-encode it to retain data integrity!  
But how do you tell?

The answer is: You can't, without context.

The whole point of abstracting your code is to remove context, especially *this* sort of context. You are intentionally
removing the necessity for whatever is calling your code to provide any underlying HTTP logic.

Which means we need to implement this abstraction with a better standard. Here's the one that makes the most sense to
me:

If whatever is calling your code, requires a special character to be in the URL (whether it be a reserved character or
not) they should provide it to your service as the literal special character only and nothing else. That means:  
No %-encoding,  
No `+` symbols that are supposed to mean spaces.  
None of that stuff.

This way, your abstracted code has complete control over everything to do with the HTTP process, including encoding and
handling special characters properly.

Sounds simple and logical, right? Why am I even on about this?  
Because I've been in too many situations where people get this horrendously wrong and, to be frank, it's really bugging
me.

Keep your code clean.

---

## Final note on implementation:

***Don't*** (for heavens sake) simplify your HTTP code down to providing a single static function that takes an entire
URL (including query string parameters) and %-encode that.

The problem with implementing it this way is:  
If whatever is calling your code, provides a query string parameter (for example, `username`) with a value that contains
a reserved character like `&` or `=` or something, then you're going to get into a very tricky situation where, without
context, you don't actually know what is valid data, and what are valid reserved characters.

A good way to implement this standard would be to have a class (or object) that performs the HTTP code. This way,
whatever calls your code has to instantiate this class as a variable and then use *Setter* methods/functions available
to this variable to set up the URL, Port, URI and each query string parameter (and anything else you need to set up).  
This means you can handle the %-encoding for each method and handle all of the actual HTTP logic within. Better yet, it
means that whatever calls your code, doesn't provide HTTP delimiters `?`, `=` and `&` which they could mistakenly
encode, thinking its helping when it's not.
