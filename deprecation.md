## Backstory 

Historically, we've been very courageous in node about seeking out the 
best APIs, even if that means breaking existing programs.  Some of you 
may remember when a "release" was "Ryan pushed something to github", 
and the entire shape of the API was unrecognizable from one day to the 
next. 

As node grew up a little, that changed to "Print a deprecation warning 
for a stable branch, and then throw in the next, and then gone in the 
one after that."  That's served us pretty well.  A lot of features 
were brand new in 0.4, and we didn't get them all right.  The 
transition to 0.6 was rocky, but the freedom to change things was 
important, even given the pain it introduced sometimes for users. 

Almost 2 years ago, we decided to rename the "sys" module to "util", 
and to fold in the "utils" module. (There was a lot of "generic bag of 
stuff" code floating around node's internals at that time.)  But, even 
then, there were a lot of people using sys, and it was deemed too 
agressive to print a warning when it was used. 

In 0.6, "sys" started printing deprecation warnings.  Following the 
established protocol, in 0.7, we made it throw, with the idea that 
we'd remove it in 0.9, as is Our Way. 

As people are moving to 0.8, the most common objection is that 
require("sys") throws.  That's a bit absurd.  Following a policy 
*can't* be more important than breaking peoples' programs. 
https://github.com/joyent/node/issues/3577 

Node is growing up.  It's used by established companies with 
reputations and customers and employees that have real work to do. 
This is a necessary step in our quest for total domination of all the 
world's programming.  Yes, it's an easy change to make.  It's also a 
stupid one to HAVE to make. 


## New Policy 

1. Every module in the docs has a stability index. 
http://nodejs.org/api/documentation.html#documentation_stability_index 
 - "experimental"  It will probably be changed.  Please use and 
provide feedback, so we change it right. 
 - "deprecated"  We reserve the right to remove it if it's in the way, 
but make no promises that it will be removed, ever. 
 - "unstable"  We reserve the right to make changes, but will consider 
it high-cost 
 - "stable"  We will do everything in our power to make this API 
continue to work for the foreseeable future. 
 - "api frozen"  The API will not be changed, even to make additions. 
 - "frozen"  No changes to the code, unless a serious bug is found. 
Code and API are considered "done" and unchanging. 

2. Breaking changes, even to deprecated or unstable APIs, are 
considered costly, and need to pay rent.  Higher-stability APIs are 
more costly to change, but ALL api changes are considered costs.  If 
they don't buy us anything, we don't need to do them. 


## Objections 

No one seems to actually object to sys/util itself, but there are 
three points that were raised about Node's policies that I'd like to 
address: 

### So you're saying node will never change ever again and all APIs  will be consistent forever?  So now Node is PHP/Windows/etc? 

Of course not.  However, from this point forward, we're going to make 
our best effort to continue to support old programs, even if it means 
re-implementing them in terms of new APIs.  If this actually is not 
possible, or causes undue harm or maintenance overhead, then we'll 
reserve the right to warn/throw/remove as we've done. 

We also reserve the right to change semantics in some cases.  There 
was no cleaner way to fix the child process exit/close stuff but to 
just bite the bullet and change it.  That sucked for a lot of people. 
It breaks my heart, but we're not going to reverse on that. 

Most people are pretty understanding if something is actually better. 
But how is spelling it "util" rather than spelling it "sys" really 
THAT much better, so much so that you need to throw an error and make 
my program not work?  Seems a bit like an overreaction. 


### So if someone complains enough and is a Big App for Enterprise Business, you're going to just reverse your decision?  WTF? 

Of course not.  But we will do our best not to break *anyone's* 
programs unless we have an extremely compelling reason to do so. 

Also, we are planning on keeping master in a working state throughout 
the development of 0.9.  If you're moving to v0.8 now, maybe also test 
on master from time to time.  Tell us when something impacts you or 
stops working. 

Deprecation reserves the *right* to remove something in the future, it 
is not a contract that it definitely *will* be removed. 


### What about the cluster "death" vs "exit" event name?  What about domains? 

Experimental APIs are clearly labelled in the documentation.  **They 
will almost certainly change.**  If you would like to help inform that 
change, please use them and let us know what parts you like, and what 
parts are confusing or lacking. 

We are not committing to always support every API we release, forever. 
 We are committing to considering breaking changes as a high-cost 
activity.  Some high-cost things are worth it.  Even some superficial 
things.  The cost of changing experimental APIs is a bit lower, since 
we've communicated with the stability index that changes are planned. 

For example, the cluster even name should really be "exit", because 
that's consistent with the ChildProcess and process event names.  The 
consistency is important there, because it's actually the same sort of 
"thing" (ie, a javascript object representing a specific operating 
system process) and it's confusing to have different names for it. 
Once we get some more feedback about 0.8's cluster, we will probably 
move it to "Unstable" or even "Stable" in a future release, at which 
point, you WILL have the assurance that programs using it will not be 
broken unless absolutely necessary. 
