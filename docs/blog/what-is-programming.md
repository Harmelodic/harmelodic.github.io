# What is Programming

> Originally published: 23 September 2016

You're probably reading this because you've recently thought something along the lines of "This coding or programming
malarkey is getting to be quite popular, I should give it a go...or at least look into it and see if it's for me!"

Well, you've just made your first step into the world of programming, so congratulations! Give yourself a pat on the
back!  
I'm not actually being sarcastic here, a lot of people find it difficult to understand the world of technology past a
smart-phone or laptop etc., so well done, you've done really well to get this far.

But now you want to know more about programming or coding.  
(Quick note, it doesn't really matter which term you use, neither makes you sound any more or less cool, just go with
what you feel comfortable with, I tend to interchange depending on the context).

## What actually *is* programming?

Good Question! Programming is simply:  
Writing instructions (or "code") for a computer to do something, *usually* so that a human doesn't have to.

## What is code?

Code is a set of instructions written for a computer to perform (or "execute").

## What's a programming language?

A programming language is a language for developers to use to write their instructions ("code") for computers.

Sadly, you can't just write a load of English or French or German in a text file and expect a computer to understand it
all as instructions, we have to be a bit more specific because computers are dumb (seriously!).

## What programming languages do computers understand?

Aha! There's the million pound question! There is in fact, technically, only one answer to that question:

Binary (or as some people call it: "Machine Code").

Binary is simply a language made up of '1's and '0's. It's that simple. That's all computers can understand because
computers are stupid; The quicker you get that fixed in your mind, the better!

## So, *why* do other programming languages exist when computers only understand Binary?

The simple answer to that is:  
Humans don't understand Binary very well and developers don't like programming in it.

It's a pain to try to develop a mobile app in binary or a website in binary. It's difficult, so we don't do it and
***that's*** where other languages come in:  
To make it easier for developers to write code.

## So, *how* does coding in these other programming languages work if computers only understand Binary?

Well, let's do a quick history lesson that explains it all. Grab a drink, get a little comfy because this stuff is going
to take a few paragraphs (but is still quite interesting).

*Note:* I'm only going to cover the important stuff and add in a few interesting facts but if you want to read further
into anything, feel free!

Some clever people back in the mid-late 20th Century sat down and came up with an idea to let developers not have to
write in Binary:  
What if developers could write their code in an understandable way (i.e. looked like English) and then get something
else to convert all that code in '1's and '0's? Wouldn't that be so much easier than faffing on writing in Binary?  
The idea worked and the method of converting code into '1's and '0's became known as: *Compiling*. People even made
computer programs called *Compilers* to do the *compiling* really quickly for developers.

Compiling became hugely popular but it was still early days for programming languages and there wasn't really any
standard language(s) for developers to use. There were loads of languages hanging around the place, each designed for a
different use (though usually some form of mathematical or scientific purpose).

Then a *REALLY* clever chap called Dennis Ritchie came along. He designed and developed a programming language called
*C*.  
*C* was designed to be a sort of "general purpose" programming language for developers to use that could also compile
into Binary really efficiently and ***HOLY CRAP*** it became popular.  
*C* is one of *the most* widely used programming languages of all time.  
To give you some sort of scope to that statement; you're probably running a Windows, Mac or Linux machine right now,
right? Well, those Operating Systems are written in *C*, *even* the modern versions are!

But *C* was only the beginning.  
Soon more standardized programming languages began to appear: *C++*, *Java*, *C#*, *Python*, *Objective-C*, *Swift*,
*JavaScript*, *PHP*.  
Some simpler to code in but made slower programs, some more specialized for specific things than others but all
standardized and *a lot* easier to code in than Binary.

## So what language should I learn?

Another great question and one that only **you** can answer, but I can provide some help:

If you are only *just* starting out at programming and/or only want to learn the basics, I would recommend learning a
rather popular simple language called *Python*.  
[Codecademy](https://www.codecademy.com) has a great, completely free tutorial on learning Python that I would
definitely recommend (Fun fact: It's where I started programming!).

If you're wanting to get into Web Development, I'd recommend learning *JavaScript* (and maybe *PHP*).

If you want to make iOS apps, you'll want to learn *Objective-C* or *Swift*.

If you want to make Android apps, you'll want to learn *Java*.

If you want to make games, you'll probably want to look into learning *C++*.

If you want to make developing software for servers, corporations, etc., you'll want to look into learning *Java* and/or
*C#* and/or *C++*.

If you want to develop Operating Systems and similar software, you'll want to learn *C* and *C++*.

And that's it, go have fu...oh, you have another question?

## WAIT! What the hell are *Interpreters*, *SDKs* and *Runtime Environments*?

Ah...yes! How could I forget?! It's important stuff after all!  
Let's tackle each of those subjects individually.

### Interpreters

Interpreters are kind of like compilers except with one fundamental difference:  
Where compilers take ALL of your code and compiles it *before* you run your program; an interpreter runs your program
and then when it needs to execute some code, it compiles that bit of code and then executes it.

This means Interpreters allow you to instantly execute your code without having to wait for a compiler to compile all of
your code all at once but at the sacrifice of running your program slower because it has to compile the code as it goes
along.  
This is great for simple programs and programs that don't need to run really quickly.

Code written in some languages, such as JavaScript and Python, can *only* be executed using an interpreter (i.e. you
*can't* compile it all at once).

### SDKs

An SDK is a Software Development Kit.

Most programming languages are maintained and developed by a Software Company or Foundation. These organizations
understand that developing for a programming language is hard on it's own, so they often provide a neat little package
of development tools; such as pre-built code, Frameworks, Runtime Environments and more.

These packages are called Software Development Kits and if you can get your hands on one for the language you're wanting
to develop with, it's highly recommended you get it and use it.

### Runtime Environments

With "low-level" (i.e. nearer to Binary) programming languages like *C* and *C++* where a developer compiles their code
and executes it, the developer has to take into account details about the machine/system the code will be executed on.
For example, the developer will have to add support for every Operating System (etc.) their program might come into
contact with.  
This can be a good thing but sometimes it's not the case.

That's where a *Runtime Environment* comes in.  
It is essentially an Operating System for programs coded in a particular language.  
This provides an extra layer between the machines Operating System and the program so that the code becomes *platform
independent*. Which is a fancy term meaning "will run on pretty much any operating system".

This means a developer can write a bunch of code and it will run on Windows, Mac, Linux etc. without any need to change
the code itself as long as the Runtime Environment is installed.

The downside is that the program will run slower than it would if it was coded in a language that didn't run code in a
Runtime Environment.  
This is because the code has to essentially compile/be executed twice, once from the developers code to the Runtime
Environment and once from the Runtime Environment to the System.

Code written in some programming languages like Java, Python and JavaScript (Node.js), *require* a Runtime Environment
to be able to be executed.
