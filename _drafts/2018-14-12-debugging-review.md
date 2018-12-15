---
title: Book Review - Debugging by David J Agans
description: What I learned from this book
categories:
 - book review
tags: debug c
---

# myNotes: Debugging

[Debugging: The 9 Indispensable Rules for Finding Even the Most Elusive Software and Hardware Problems](https://www.amazon.com.mx/Debugging-Indispensable-Software-Hardware-Problems/dp/0814474578) by David J Agans

As this is one of my first 'notes', I will go chapter by chapter sharing the notes that I took while reading the book. I don't know if this is going to be the best way to do it but it will be my starting point.

The author is a seasoned engineer that has debug several products on his career. In his book, he try to pour all his experience in 9 golden rules to debug any issue, it doesn't matter if it is a computer program, and embedded system or a car! The reading is really enjoyable as he exemplifies his rules through 'war stories' and always with a pitch of humor that all engineer will enjoy.

One of the remarkable points that the author does at the beginning of the book is that debugging is different to troubleshooting. He defines troubleshooting as finding what is wrong with a copy of a know-to-be-working product, which wire is broken or what piece requires to be replaced. Troubleshooting is usually performed by support technicians no by designers.

This idea didn't quite fit in my head, as I'm currently working as a Application Engineer that is supposing to be providing technical support to an always working product. This is not at all my 'usual' day, I'm always involved in debug sessions where the product is not working as expected due to a 'creative' approach from a customer o due to an corner case not covered by the validation process. So, I'm not directly involved in the design process but I usually end up debugging not troubleshooting.

When reading a troubleshooting manual, the engineer is getting all the experience of the designers, what they did learn while developing the product, however, this is not intended to help you to learn how to find new issues in a product.

The 'war stories' are a great way to exemplify how the 9 rules can be applied. I wonder if a training on debugging can be entirely be made of 'war stories', and if some company ask their engineers to record their experience so new members of the crew can benefit from what other seasoned engineers went through.

Going back to the rules. Each chapter covers one of the following 9 rules:

* Understand the System
* Make it Fail
* Quit thinking and Look
* Divide and Conquer
* Change one Thing at the Time
* Keep an Audit Trail
* Check the Plug
* Get a Fresh View
* If you Didn't Fix it, It Ain't Fixed

Another great thing about this book is that the author summarize their the content at the end of each chapter.

## Understand the System

This is some straight forward advice. Always start by **reading the manual** is you are new to a system or component. Reading the manual will help you to understand how the system is supposed to work, and its capabilities. If it is the first time that you will be debugging a component, you need to **read the user guide from cover to cover**, as you are new to the material and you don't know where is going to be the silver bullet that is going to solve the issue.

> When I started working as an application engineer, I made the mistake of trusting that all the customer had already read the manual, and that they were trying to achieve something that the component is suppose to do. This was not always the case and many times I had to suggest a different component after learning that what the customer wanted to do wasn't possible with his current selection. After a while, I got more experienced on some of the components offered by the company, so I was able to jump directly into sections of the documentation without the need of read everything from scratch.

If you read the manual you will have a better understanding on how the components of a system interact between each other to generate a result. If the system is a big [black box](https://en.wikipedia.org/wiki/Black_box) for you, you will be available of detect a failure on the system, but not which component is causing the failure. Having a **road map of all the components and interfaces** of the systems allows you to narrow down the blocks where the issue could be happening. If your car is making a noise when it is in movement, it is a good idea to know all the parts of the [drive train](https://www.popularmechanics.com/cars/how-to/a250/1302716/) so you can make a better guess of in which component the noise is coming from.

Reading the manual is not going to do any good if you don't **have a good technical foundation**. For example, if you don't know the difference between [big-endian and little-endian](https://es.wikipedia.org/wiki/Endianness), you will have no clue what to expect after a memory read on a little-endian system. In the same tone, if you are not familiar with a [Avalon Interface Specification](https://www.intel.com/content/dam/www/programmable/us/en/pdfs/literature/manual/mnl_avalon_spec.pdf), you will have a hard time understanding why the writes operations that you are performing are not taking place while the *mgmt_waitrequest* signal is high. This is also true for conversations with peers, if you don't have good foundations, you can spend hours talking with the component guru and be in the same place without idea of what is wrong with the system as you didn't connect all the information that the guru just provided.

Almost all the time reading the manual is not enough to solve an issue. Reading the manual help you to know where to start looking for the issue, and also know what you should expect when the system is working properly. After having a couple of suspects, it is tie to dig in to the issue. To get more information on the failure you need to rely on your **debug tools set**, knowing what tools do you have available and how to use them will ease the debug process as you will be able to get first hand information from the system. Knowing how to use an oscilloscope will be helpful to look for noise on a signal, however, it would not be an advantage if you want to decipher a [I2C bus frame](https://learn.sparkfun.com/tutorials/i2c/all).

Finally, always **look for more details**. If you have a functional description of the component it would be a good idea also to read it, maybe the designers didn't add information into the user guide, or the user guide have a over simplified version of a diagram that you are looking for. Also, reading the documentation is necessary, but sometimes the information on the manual is not accurate and the only way to get straight facts is connecting your debug tool to the system.

Also, you cannot always trust on the information provided by a peer. Maybe one of your fellow engineers misunderstood an user guide, and he is explaining to you something that is not accurate. The same thing applies for example designs or code examples, usually this kind of collaterals were designed by the same team that developed the component, so they know how everything works and will avoid common pitfalls that you are not aware of. also, example design are tested on development environments, that are controlled and use a closed set of tools, there is a great chance that your development system is not the same as the one used to generate the example design, so there is no guarantee that you are going to get the same results.

When in the search for more details, it is also a good idea to [grep](http://tldp.org/LDP/Bash-Beginners-Guide/html/sect_04_02.html) for debug comments, maybe the developer left behind a note on an issue that he wasn't able to reproduce or a note about a conner case that will make the system fail but so rarely that it wasn't worth to fix.

## Make it Fail

* Do it again
* Start at the beginning
* Stimulate the failure
* But don't simulate the failure
* Find the uncontrolled condition that makes it intermittent
* Record everything and find the signature of intermittent bugs
* Don't trust statistics to much
* Know that 'that' can happen
* Never throw away a debugging tool

## Quit thinking and Look
## Divide and Conquer
## Change one Thing at the Time
## Keep an Audit Trail
## Check the Plug
## Get a Fresh View
## If you Didn't Fix it, It Ain't Fixed




> Save for the end
When it took us a long time to find a bug, it was because we had neglected some essential, fundamental rule.
People who excelled at quick debugging inherently understood and applied these rules.
