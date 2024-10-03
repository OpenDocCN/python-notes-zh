# PyCon US 2022 - P16：Talk - A. Jesse Jiryu Davis_ Why Should Async Get All The Love   Advanced Contro - VikingDen7 - BV1f8411Y7cP

 Hello everyone， welcome。 The next talk will be starting soon。 Let's prepare ourselves。



![](img/c4ef1fa886775c408a105c6ab11f8e26_1.png)

![](img/c4ef1fa886775c408a105c6ab11f8e26_2.png)

 to hear Jesse and why should a sink get all the love？ Once controlled flow with threats。

 I couldn't agree more。 Jesse， please welcome with a warm applause。 Thanks a lot。

 AsyncIO is really hip。 And not just asyncIO。 Twisted tornado。 Those are the older async， frameworks。

 Curio and trio， the new async frameworks。 They're all super cool。 They're hyped and， deservedly so。

 They are amazing。 I'm a big fan。 I spent a lot of time in the early days。

 contributing to tornado and to asyncIO。 My very first PyCon talk eight years ago in Montreal。

 was what is async？ How does it work and when should you use it？ I was one of the early boosters。

 of async。 AsyncIO introduced a lot of Pythonistas to some advanced control flow techniques。

 like tasks and futures， chaining， asyncIO gather。 All of this was really awesome and it was very。

 exciting。 But in the excitement， there was a message that was lost which is that all。

 this was already possible with threads too。 Compared to asyncIO， threads seem hopelessly outdated。

 The cool kids will laugh at you if， you use threads。

 Now when should you use either threads or asyncIO？ Threads and asyncIO， they。

 are two ways to do concurrency。 Now concurrency is not parallelism。 Parallelism is when your。



![](img/c4ef1fa886775c408a105c6ab11f8e26_4.png)

 app is actually doing multiple things on multiple CPUs at once。

 And Python mostly can't do parallelism， because of the global interpreter lock。

 You can understand the gill with this phrase which， is short enough to read on your hand。

 One thread runs Python while others sleep or await， IO。

 And if you want to learn more about the gill， I give a PyCon talk a few years ago at， this link。

 So threads and asyncIO， they are kind of even here。 Neither threads nor asyncIO。

 tasks can use multiple CPUs。 And just so you know that I know what you are all thinking。

 let's do a quick aside about multiprocessing which is that if you really need parallelism。

 you should use multiprocessing。 But it's not the panacea that it's made out to be。 It is。

 the only way for standard Python to use multiple cores but coordinating and exchanging data。

 among processes has a lot of hidden complexity。 So beware。



![](img/c4ef1fa886775c408a105c6ab11f8e26_6.png)

 So what this talk is actually about is concurrency。 And concurrency is dealing with events in。

 partial order。 That means you are waiting for several things to happen but you don't。

 know what order they will happen in。 The main example by far of concurrency is waiting。

 for data on network connections at once。 Some network needs concurrency。 Basically all network。

 servers need concurrency， sometimes extremely high levels of concurrency。 And we can use。

 threads or kind of async framework like asyncIO as our method of supporting concurrency。

 So which one should you use？ Let's start with asyncIO's advantage。 Very， very high concurrency。

 programs are more efficient with asyncIO when it comes to memory。 Here's a chart of two programs。

 The one in blue is making lots of threads。 The one in， red lots of asyncIO tasks。

 The red programs start using more memory just because it imported， asyncIO。

 But that doesn't really matter。 What matters is that as you add more concurrent， things。

 threads or tasks， the memory usage for the asyncIO program grows more slowly。

 And I'll share links to details about this benchmark at the end of the talk。

 From the previous chart， we saw that threads take about 10K each。 Python threads just don't。

 take that much memory。 More than a few hundred threads is impractical。 But below that， they。

 really work quite nicely。 If you recall the problems with large numbers of threads that。

 David Beasley pointed out many years ago， those were in Python 2。 Those have been fixed。

 In comparison， in asyncIO， each task takes about two K-grams。 So it is more efficient。

 AsyncIO is more efficient for huge numbers of mostly idle connections where it's not。

 actually using the processor all that much for each one。 Is asyncIO faster than threads？ No。

 As CalPetersen wrote， "Sadly， async is not go faster stripes for the Python interpreter。

 Under realistic conditions， asynchronous web frameworks are slightly worse throughput and。

 much worse latency variance。" The standard library is slower than a lot of。

 multi-threaded frameworks。 That's just because asyncIO executes lots of Python to process each。

 event。 Generally， frameworks are faster the more that they are implemented in C。 Some。

 of the newer async frameworks that have lots of C and particularly those based on UV loop。

 are quite fast。 But they're not faster than multi-threading。 Generally， tail latency。

 still seems to be a problem with them。 I'm not going to necessarily say that all async。

 frameworks are slower than all multi-threaded frameworks。 I think what I can say without。

 getting into trouble is asyncIO isn't faster。 It's efficient， again， only for huge numbers。

 of mostly idle connections and only for the data。 What about compatibility？

 Does asyncIO work with the tools that you're already using？ Well， what web frameworks do you use？

 Here are the most popular web frameworks last year。

 The total is more than 100% because people could choose multiple。 The flask and Django。

 the most popular ones are multi-threaded frameworks。 They don't， for the most part。

 work with asyncIO。 Only three of these are asyncIO compatible。 So if you want to use asyncIO。

 that probably， means that you're going to have to rewrite large portions of your app。

 AsyncIO is incompatible， with the most popular frameworks。

 How tricky is it to write correct concurrent code with threads versus asyncIO？ Let's make。

 a function to something。 It will increment a global counter。 And we can run it on two threads。

 at once。 Will counter always eventually equal two？ No， unfortunately it won't because plus。

 equals is not atomic。 It's got several substeps。 It has to load the value from the global add。

 one to it and save it back。 And if two threads interleave during this process， one of the。

 updates can be lost and the final value might only be one， not two。

 We need to protect plus equals with a lock。 I think that doing this correctly， it's kind， of tricky。

 This is a lot of code and a subtle gotcha for a very simple task。 This blog post。

 from 2014 talks about this trickiness problem。 It's by glyph， the author of the twisted async。

 framework。 It's pretty old， but it's still my favorite argument about this topic。 Glyph， writes。

 as we know， threads are a bad idea for most purposes。 I disagree with Glyph。 This， is my point。

 He says threads make local reasoning difficult and local reasoning is perhaps the。

 most important thing in software development。 Glyph's point is that we should use async。

 not because it's faster than threads and not even because it's more efficient in memory。

 but rather because it's less tricky。 Let's see this in action。 Here's our counter value。

 incrementer， but now it's a co-routine。 It's async def。 We'll run it on two tasks at once。

 You can tell just by looking at the code where it might interleave with another co-routine。

 Anywhere that there's an await expression， it couldn't leave。 That means anywhere that， there isn't。

 it can't。 That means that plus equals， it's atomic because there's no plus， equal。

 there's no await in plus equals。 So we don't need a lock。 This means async。io is。

 better because it's less tricky than multi-threading。 So in summary， speed， threads are at least。

 just fast。 Compatibility， threads work with Flask， Django， without rewriting your app for async。io。

 In summary， async。io efficiently waits on huge numbers of mostly idle network connections。

 and trickiness。 Async。io is less error prone than threads。 But my point is that it doesn't。

 need to be this way。 I think it's time to take another look at threads。 All along it's。

 been possible to write elegant， correct concurrent code with threads。 It doesn't have to be tricky。

 and complicated。 Threads are actually cool。 So now we're getting into the heart of this， talk。

 how to write less tricky code with threads。 To begin， let's see how to use threads， with futures。

 Threads had futures first before async。io。 Futures let us express control flows。

 that you'd struggle to write with the old locks and condition variables。 Confusingly， async。io。

 has a different future class from threads。 But I've never had to use both in the same program。

 at once， so it's fine。 Here's our previous code example with the lock that protects the， counter。

 We can use futures to make it better。 First， let's add this。 We want to know what。

 the counter value is at the very end。 And then let's rewrite this with views。 So the first。

 thing we do is we import from concurrent futures。 This is where all the cool thread stuff lives。

 It's been here since Python 3。2， so a while now。 Now our do something function。 It takes。

 a future as an argument and it sets the result to one。 This is called resolving the future。

 And one here is just how much to add to the counter。 We'll run do something on two threads。

 and we'll pass in the two futures as arguments。 And then we will wait for the futures to be。

 resolved， calling future。result， waits for some other thread to call set result。 Once。

 both of these futures are resolved， we sum the results and that's our counter。 This code。

 isn't really much of an improvement yet， but I'm just showing you how futures work。

 We're going to get there。 Here's a better example。 Let's use a thread pool executor。

 This is a thread pool that runs things on threads and reuses threads efficiently。

 Now our do something， function。 It doesn't take a future and resolve it。 It just returns one。

 And we'll call it using， executor。submit that starts the function running very soon on a background thread。

 And it returns， a future which is resolved when that function returns。

 The rest of our code here is the， same as before。 We wait for the results of the two futures and sum them。

 This is looking， much better than old fashioned code using a lock。

 but there's more room for improvement。 Executor。map。 It's like the built-in map。

 but it's concurrent。 It runs the function， on each of the arguments all at the same time。

 So we'll give it range of two to undo something， twice concurrently。

 We don't actually need the input arguments， so we'll just use a dummy， argument here。

 There's no more explicit futures here， no explicit threads。 It's all hidden。

 inside the map implementation。 I think this looks really nice and it's not error prone， at all。

 What about more complex workflows？ See， you want to make some French press coffee。

 Like I did this morning。 I brought a hand grinder with me to Salt Lake City， so grinding the。

 coffee takes a little bit of time。 And then I heated the water。 I put them both in the。

 press to brew and four minutes later I drank it。 Obviously this is not very efficient。

 I should be heating the water and grinding the coffee concurrently， so it should really。

 look like this。 So how do we code this kind of workflow with threads？ Let's make a thread。

 pool executor。 Then there's functions to heat the water and grind the coffee。 And then this。

 brew function， it takes two futures and waits until both have been completed。 And then it。

 waits four minutes for the coffee to brew。 So we can use the executor， the coffee， heat。

 the water and grind the coffee concurrently。 And then call brew with those two futures。

 And once it's done we can drink the coffee。 So far so good。 Let's add some more steps。

 to this workflow and see how modern thread programming handles the extra complexity。

 So there's a quick step after heating the water。 You have to pour it into the press and。

 there's a step after grinding the coffee。 You have to pour that into the press。 So these。

 can be finished in either order， but we want to do the red step as soon as it's blue step。

 is completed。 So we want to do two things as their prerequisites are done。 Here's how， to code this。

 So now heat water and grind coffee， they've got return values， they produce， some sort of product。

 And our brew function now， we're going to use as completed。 This。

 is also in concurrent futures in the standard library。 And it returns the futures in the。

 order that they are completed。 So if the water is heated first， we handle that first， or if。

 the coffee is ground first， we handle that first。 Once both are done， then we wait four， minutes。

 And the rest of the code is as before。 Imagine if you had to write this using。

 locks and condition variables to signal when each of these steps was done and to trigger。

 the next step， this would be a nightmare。 But using concurrent futures， this is actually。

 really slick。 Of course， it's not really modern code yet， because we don't have any。

 type annotations。 Here I've annotated the return types of heat water and grind coffee。 And if。

 you have a future that resolves to a certain type， you just subscripted like this。 So this。

 will be a future string。 And then the type system knows that when you call future。result。

 that will be a string， or a well-thrown exception。 Let's take another look at this workflow。 What。

 if things get even more complex？ What if this workflow is one component of like a whole。

 afternoon that I'm spending？ So this is just the coffee workflow。 I've also got my chores， workflow。

 So first I'm going to make and drink coffee， and then that gives me the motivation， to do my chores。

 And of course， I'm listening to a podcast concurrently the entire time that， I'm doing this。

 How would we handle a complex set of workflows like this with modern threading？

 The nice way to structure this is with a with statement。 So it's start a block like with。

 thread pull executor and run a function on that executor。 And you can start an inner。

 executor using another with statement。 When you leave the block， either normal or with。

 an exception， it joins and shuts down the executor automatically。 So all threads begin。

 within that block must finish one way or another before you can leave the block。 And you can。

 have multiple inner blocks within that outer block。 This style is called structured concurrency。

 And it's been popularized in a few languages and frameworks。 Python people have mostly。

 learned about it from Nathaniel Smith's Trio async framework。 And task groups are going。

 to be useful for this with async。io in the standard library in Python 3。11。 Unfortunately。

 you can't really do this with threads fully。 The kind of definition of structured concurrency。

 is that if one of the tasks in a block fails， then all of the other threads that that block。

 started or will start should be canceled very soon。 And unfortunately， we can't really。

 do that with threads because threads are not as good at cancellation as async。io。

 I'm going to show you a handwritten solution。 You'd have to do something like this with， your app。

 First， you want to make a custom exception type thread canceled error。 I'm。

 going to call it inherit from base exceptions。 So it's kind of hard to catch。 And then you。

 create a token， which is initially not canceled。 And it's got a method for checking if it's。

 canceled。 Now in our do something function， we have to very frequently check if the token。

 is canceled or not。 And you can't forget to check。 Otherwise， the whole thing won't work。

 The rest of the code creates a token and a thread pull executor starts the function on。

 a thread passing in the token。 A second later， it marks the token canceled。 And then sometime。

 soon after that， the thread should kill itself。 We can wait for that by waiting on future。result。

 which will then throw the canceled error。 Python doesn't control the thread scheduler。

 the way that it controls the async。io event loop。 So cancellation with threads just， it。

 can't ever be as good as with async。io。 There are some better ideas out there like this one。

 from Nathaniel Smith。 I'm curious if anybody's got a pep in progress to improve thread。

 But enough for the bad news about threads， let's get back to the good news。 Here's a real。

 life example that I coded a few months ago。 So I made a toy Paxos implementation。 Paxos。

 is a protocol for servers to work together for fault tolerance。 So we've got a group of。

 servers here host 01 and 02， and they all talk to each other。 But how does each server know。

 who all of the members of the group are？ Let's give them a config file with the list， of servers。

 So here it is。 But now， how does each server know which one in the list it， is？

 This is actually surprisingly hard in cloud deployments or even in a data center。

 Every server usually has a couple of IP addresses， a couple of DNS names。 It's not actually that。

 easy to know。 If you've got a DNS name， is this my name or not？ Just calling gethost。

 name won't give you the information you need。 So the solution to this， it's kind of amazing。

 First of all， every server when it starts up generates a unique ID for itself。 Next， each。

 server sends a request to all the servers in the list asking them for IDs。 That includes。

 asking itself for its own ID， but it doesn't know that because it doesn't know which one， it is。

 So here's host 0， requesting all of the server's IDs。 The other servers are doing， this too。

 Host 0 and host 1， they respond with different IDs。 And host 0 also responds。

 to itself with its own ID。 It sees that they are equal so it knows， aha， that's me， I'm， host 0。

 This is actually how MongoDB Replica sets and lots of other distributed systems， solve this problem。

 But it's complicated because a server can process any other requests。

 until it knows a list entry corresponds to itself。 Also， servers can start in any order。

 So this is a complex workflow。 How are we going to make this clean and nice with threads？

 Here's the server code。 First thing， we create a unique ID。 I'm going to use Flask for the。

 server because I love Flask， it's the most popular。 And to create this self-future which。

 will be resolved once I've found myself。 Here's a flat， flat， flat， flat， flat， flat， flask。

 root to return the server ID to anybody who requests it。 And here's some client request， handler。

 We can't handle any client requests until we know who we are。 So we have to call， self-future。

result。 That'll wait for self-future to be resolved。 And then once it's resolved， this handler。

 this 。result call will return right away。 When the server starts up， it loads， up its config。

 creates an executor。 And it runs Flask in the background on a thread。 And。

 that means that the main thread is able to keep doing stuff。 And what is it doing？ It's。

 launching the search for itself。 So here's the search loop。 First we note the time。 And。

 then while it's been less than 60 seconds and we haven't found a self， we will continue。

 We will search every server name in the list。 And for each one of those， we'll use the requests。

 client library to request that server's unique ID。 That means that we'll hit this request。

 handler either on this server or another server， but we don't know yet。 If the server IDs， match。

 then we found ourselves。 So that means that we can resolve the future。 And that will。

 unblock any threads that are waiting to process client requests。 Or we might throw an exception。

 We could throw an exception because we're trying to contact another server and it's not。

 started yet。 Servers can start in any order。 We might even get an exception trying to contact。

 ourselves because we're starting Flask on a thread in the background and it can take any。

 amount of time to get started。 So we have to be prepared for that also to fail if we。

 don't wait long enough。 So if there's any failure， if we haven't fed ourselves， then。

 we sleep for a second and then keep trying。 If we do find ourselves， then we reach the， end。

 And we wait for the app done future to be resolved。 We got that from executor submit， up above。

 So this means there were letting Flask run in the background until it gets control。

 C signal and decides to quit。 This is pretty clean and clear， right？ We had。

 a complex problem statement and we used executors and futures to come up with a very straightforward。

 and legible way of expressing this with threads。 We didn't have to use async。io and that means。

 that we could use Flask and we could use requests。 If we'd used async。io， we would not have been。

 able to use Flask and requests。 We would have had to use excellent alternatives but not。

 the ones that we were used to。 So threads are cool。 Don't let the async。io。

 kids make you feel like a nerd。 Threads are a better choice than async。io for most concurrent。

 programs。 They're at least as fast as async。io。 They're compatible with all the popular frameworks。

 and with the techniques that we just look at using executors and futures。 It's possible。

 to write clean， elegant concurrent code。 Thanks。 [ Applause ]。



![](img/c4ef1fa886775c408a105c6ab11f8e26_8.png)

![](img/c4ef1fa886775c408a105c6ab11f8e26_9.png)