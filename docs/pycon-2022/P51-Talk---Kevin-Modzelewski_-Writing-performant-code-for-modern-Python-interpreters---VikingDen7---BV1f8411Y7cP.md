# P51：Talk - Kevin Modzelewski_ Writing performant code for modern Python interpreters - VikingDen7 - BV1f8411Y7cP

 Welcome， everyone。 I have the pleasure of introducing Kevin。
![](img/5165d7baa6c620f50fac7a33077665ca_1.png)

 Motszelski from Anaconda and he will be giving the talk on writing。 performance code for modern Python interpreters and he will take the questions at the end of the talk。 \>\> Hi， everyone。 I condensed the title of the talk a little bit。 So it's now how to write fast modern Python code。 And my name is Kevin， Motszelski。

 I'm an employee at Anaconda where I work on the piston optimized Python interpreter。 And today I want to focus on the word， modern。 There's a lot of Python tips out there。 There's another talk across the hall that's also about Python performance。 And this talk。 I wanted to do this talk because there's a lot of work that's currently going into optimizing Python that has downstream effects for what you as a Python programmer can think about to make your code even faster。

 So some of the tips that have been around for a while are not quite as useful anymore。 And there's some new ones that if you get onto these new systems。 you might want to start thinking about。 So the structure of the talk is first I'm going to talk a little bit about what makes Python slow。 what we're doing to speed it up， and then spend the rest of the talk going into specific examples of how those optimizations affect you as a programmer。

 Why Python is slow is a bit of a divisive topic。 Everyone seems to give different reasons。 This is my personal take on it。 The most common reason is that Python is interpreted。 interpreted languages are slow， so Python is slow。 But in my personal measurements on web servers。 the interpretation overhead is about 10% of the time。 So it's significant。 People don't want it。

 But it doesn't explain why Python can be 10 to 100 times slower than C。 For that you need to go to a different set of things which I'm generically calling dynamic behavior。 Python is a very dynamic language as we know。 And I'm not going to list all the ways it can be dynamic。 but I'm just going to call out a couple today。 So the first is maybe the most obvious way of dynamic。

 which is you don't write types before variables。 So the interpreter doesn't know what types any of the variables are。 And yes， there are static annotations， but do you really want the interpreter to crash if your annotation is wrong？

 Those aren't used to optimize performance yet。 So this is slow because it means that any time you want to do anything in Python。 you have to check what is the type of this object， how do I do the operation I want on this object？

 The second thing that I call slow here is dynamic variable lookups。 And this is something that we might not even think about because it doesn't seem like it should have to happen sometimes。 But say you have a print statement in print， hello world or whatever。 Print isn't a special word in Python。 Print is just a function。 And when the interpreter sees print。

 it says， okay， let's look up what this function is。 and it does this whole lookup mechanism to see where that is。 In particular， it asks。 did anyone in this module override what the print function is？

 And it has to do that every single time you want to print something。 every single time you want to take the length of something， cast something to an integer。 All of these words require expensive lookups。 And then the third thing I'm pointing out here are dynamic attributes that even within a single class。 in general， don't know in advance what attributes exist on that class。

 Which means that you need a dynamic representation of the attributes。 which are pretty fast for what they are in Python。 but they're still much slower than static languages。 So with that little primer on where we started。 there's three projects that are sort of coming out in various forums either last year or this year。

 The project I work on is piston。 It's a fork of CPython being run out of Anaconda。 There's the faster CPython project， which is working directly inside CPython。 and all that work is going to start showing up in 3。11 in October。 and that's being run out of Microsoft。 And there's Cinder， which is out of Instagram。

 and is also a fork of CPython。 And these are all available right now in various forums。 So this is going to be the controversial slide of my talk， which is why it's not filled in。 because I'm going to go step by step with a lot of disclaimers。 This is going to be the set of projects in this space， and my benchmark numbers for them。

 So I think I'm going to get a lot of flack for this slide， so that's why I'm disclaiming it a lot。 The first controversial thing is even choosing a set of benchmarks for analyzing performance。 There's a very common semi-standard set of benchmarks called Pi Performance。 And it's nice in a lot of ways。 It's like well established。

 A lot of people present their numbers for it。 I personally think that it tends to overstate performance benefits。 and so I tend to like to look at more application code。 So I wrote a flask benchmark as well。 Flask is a web server for Python， and maybe a web application library。 And it's one of the simpler ones， and I chose a simple one so that I could get more projects working with it。

 The next controversial thing is the selection of a baseline to measure everything against。 I picked Python 3。8 because that's what piston is based on。 And specifically I picked the Ubuntu build of Python 3。8。 I didn't know this coming into this。 but actually different builds of Python can be pretty different speeds。

 So like the same version of Python， but built in different ways， will be different speeds。 So the Ubuntu build is a pretty fast build。 I believe the Mac and Windows builds are slow builds。 So anyway， this is a Python 3。8 Ubuntu build。 And we're measuring relative numbers here。 so it's the same speed as itself。 The next controversial thing is I get to list my project first。

 So piston， we show improvements on both of these benchmarks。 And you can see the relative improvement is quite different between these two benchmarks。 So it is not getting into which is a more accurate。 but it is important to choose which benchmarks you use that are actually representative of your programs。

 So we'll get very different numbers either way。 The next is Python 3。11， alpha 7。 which includes most of the faster sleep Python work。 This came out， I think， earlier in April。 And they also show good improvements on both of these numbers。 This is controversial because they say a different number for their pipe performance numbers。

 I don't know exactly where the difference is， but they say 25%。 But when I measured it， I got 15%。 Then their cinder， they don't have releases， so I just grabbed their GitHub and built it。 And very oddly， it was quite a bit slower than standard CPython。 So I put question marks here because I don't really believe these are real numbers。

 They're using it internally。 And I think they're smart enough to not use something that's slower。 I don't know what's going on here。 I think they didn't open source all of it。 I'm not sure。 Now。 this is going to be a little controversial。 I put pipy on here。 This is a little bit more of an established player in the Python performance space。

 But they make a very different set of trade-offs， which I think show up in these numbers， which。 first they can't run PyPerformance， so they don't support all of the dependencies of PyPerformance。 And then they're slower on this web serving benchmark。 And the controversial thing here is I did not put a question mark after their flask。

 number because it is in line with other numbers I've seen of PyPy。 And then I also took the time to benchmark pigeon。 I think it's a little bit less well-known。 but I still wanted to know how it did。 And I don't know what happened here， but pigeon was 1。000 times slower。 So that gives us a double question mark。

 I assume that that's not the behavior that they get， but I didn't have time to sort all。 these things out before this talk， unfortunately。 So this is toward the state of Python optimizers that aim to support all of Python。 There's lots and lots of Python performance tools， but a lot of other ones fill more。 in nice positions。 So in terms of going back to what makes Python slow。

 we can start talking about what， these projects do for those things that I mentioned are slow。 The first is interpretation overhead， which is kind of just gone now。 That a lot of these projects。 such as piston and cinder， add JIT compilers and JIT stands， for just in time。 So it means that instead of compiling your code during your development process， we will。

 compile your code as it's running and convert your Python code into assemblies of instructions。 that is your Python code。 And so this sort of definitely gets rid of interpretation overhead。 I'm not going to really talk about this today because it's really cool at a technical level。 and 10% is really nice to get。 It's not something that really you as a programmer can affect that much。

 You just kind of get it。 You're not going to really improve or diminish it in any way。 So I'm not going to talk about it for this talk。 What I'm going to spend the rest of the talk talking about are the dynamic features。 I listed a bunch of projects and all these projects do a bunch of things。 But if I was going to make a sweeping generalization。

 I would say the sort of bread and butter technique。 that all these projects are doing in many different areas is to use the combination of。 two theories。 The first is that most code does not use the full dynamic power that it could at any point。 in time。 And the second idea is that we can quickly check if code is using the dynamic power that。

 it could。 And so this lets us say that we can very quickly check to see that nothing strange is happening。 right here and we can do something fast instead。 And this is pretty much the source of most of the speedups I showed you earlier。 So this is quite effective。 And this sounds great。 You know。 Python has dynamic features but you're not paying for them if you're not using， them anymore。

 But if you kind of reverse that statement， it kind of says that you are paying for dynamic。 features that you do use now。 That before they were kind of free because you were paying for them whether or not you。 use them so it didn't matter if you use them。 But now because you no longer pay for them when you're not using them。 it is something， that you as a programmer could think about and speed up your program even more。

 To be clear， you don't need to。 Your stuff will be faster no matter what。 But if you want to get the very best performance out of these new systems， thinking about these。 things will make your code even faster。 All right， so the rest of my talk is going into examples。 And the first one is that global variable and I suppose built in variable case that I talked， about。

 So say you have a print statement or you're printing out a bunch of things or using when。 to get the links of things and you're looking up that name a whole bunch of times。 In Python。 there's a， in the implementation of Python， there's a very quick way to check。 where any global variables assigned since the last time I did this lookup。

 So you can very quickly check where any global variables assigned and if not， then we know。 that the global lookup resolves to the same thing it did last time。 And I should be clear here about what it means to assign to a global variable。 I have two snippets of code here。 The one on the left assigns to the global variable。

 It takes a new value and assigns it to the name of the global variable。 The one on the right。 I would call that mutating a global variable and the one on the right。 does not affect what I'm talking about。 That's fine。 But the one on the left， that assignment。 L equals new list， will slow down the following， print statement。

 I put together a benchmark and the numbers in this table are the time it takes to look。 up a global variable。 And the two columns are， I did a first benchmark where we're sometimes also assigning to global。 variables in a second benchmark where we're never assigning to the global variables after。 initialization。 And you can see what I mean by you're not paying for dynamic features in Python 3。

8 because， these are the same speed。 It's doing the same amount of work。 It's doing the full dynamic work each time regardless of whether this optimization could。 be applied。 The story is very different with these modern implementations that was pissed in especially。 it's six times faster in the not updated case。 I wouldn't take these numbers too literally。

 This table I think is going to evolve very rapidly。 I think I was surprised a little by these numbers。 I think the faster C Python people have some ideas from it。 So the exact numbers are going to change。 But I think the general conclusion that the no reassignments case is almost always going。

 to be faster than the reassignments case is we'll hold up over time。 This leads to a pretty simple tip that I'm giving out which is try not to reassign your。 global variables。 You might have performance might not be your primary concern but if you're considering。 performance assigning to global variables will slow things down。

 And if you still want global mutable state then store it within an object as an attribute。 on an object or in a dictionary or something like that。 The next set of dynamic behavior are attributes that I talked about before。 As I said you generally don't know what sets of attributes are going to be on an object。

 and this means that we use dictionaries in general。 There's lots of special cases in Python but in general Python objects are backed by dictionaries。 otherwise known as hash tables。 Python dictionaries are very fast as dictionaries come but they're still much slower than say。 in C where it's just a direct pointer look up in a single memory load。

 So even though each individual access is not that slow because attribute access is such。 a common operation a large part of the runtime does end up doing this。 So we're going to apply the same optimization to this which is we're going to assume that。 you're not using the full dynamic power that you're not having your objects of different。

 shapes and changing the shapes all the time。 And we're going to say I'm saying this a little vaguely because the exact technical restrictions。 are a little bit involved but at a high level we say that if a repeated look up looks the。 same as it did the previous time then we can execute it the same as we did the last time。 And there's all these cases and all these special cases and different things that that。

 can mean and it's actually quite complicated but at a high level that's what we do。 And as I said there's a bunch of different things that lead to these technical restrictions。 but I'm going to call it two as things that you might run into that could affect this。 The first is what I'm calling different shapes so to snip it on the left we have two objects。

 of this class and they have different attributes on them。 This forces a less efficient representation of the class's objects because we can no longer。 say all the objects of this class have a single shape。 So once you do this。 this snip it on the left， accessing attributes on those objects will。

 be slow for the rest of your program。 The other case that it hurts is what I'm calling type mutated which is when you change。 attributes on the class of an object。 You can see here in the second case we have an object。 we send an attribute on it and， then we change an attribute on the class。 And the reason this is really difficult for performance is the class has a lot of ways。

 that it can intercept attribute lookups on its objects。 And generally they're not used but so we have a very fast way of saying did the class decide， sorry。 We can say we know in the past the class did not intercept them and nothing changed on。 the class so now it's still not intercepting them。

 If you change something on the class we no longer know if you might have changed something that。 could now intercept attribute accesses。 So we have to do the expensive check。 I made a benchmark and the third column is what I call the happy case where all the technical。 restrictions are met and the fast path can be taken。

 And you can see from this example that again in Python 3。8 you're not really paying your。 cost for your objects being different shapes， you're doing a full hash table look up no matter。 what and it's the same cost。 You already were in Python 3。8 I think it goes quite a bit back beyond that you were。

 paying a cost for updating your classes because that does invalidate a bunch of caching。 And now in the optimized interpreters the happy case gets way faster but the other cases。 not so much。 And again the exact squares that are good are going to change over time but the general。 idea here is that having your objects of different shapes or changing the types will。

 generally tend to have the effect of slowing down your attribute lookups。 So the tip here is basically just don't do either of those things if you can avoid it。 especially the shape one。 In looking at some code I've seen code that doesn't has objects of different shapes and。 they were doing that I think to save memory by not assigning variables they didn't need。

 but that's also not a great tip anymore because by having different shapes you're using this。 less efficient representation and using more memory in general。 So in general you want to set the same attributes in the same order。 I'm not going to get it into it in this talk but if you know what slots are those are now。

 the fastest way to do attributes in Python they don't guarantee good performance but。 they give you the best chance they resolve a bunch of the technical aspects that I didn't。 really go into in this example。 There's a special case of attribute lookups which is where you look up in an attribute。 and then you immediately call that attribute object。

 I'm calling this method calls and so that's sort of a two-step process you look up the。 attribute you take that object and then you call that object and there's this common。 piece of advice that if you're doing that a lot in a loop you should try to do the attribute。 lookup only once outside the loop and then call the object inside the loop and that looks。

 something like this where you might say we want to append a bunch of things to a list。 let's forget about list comprehensions and whatever else and just say we want to append。 a bunch of things we're going to cache this list out of pen method and just call that。 attribute inside the loop inside the for loop。 And doing a benchmark in Python 3。

8 this is decent advice if performance is what you're。 looking for this does improve performance by about 66% in this case。 The problem now in 2022 with these smart optimizers is the optimizers want to see more of your。 code at once to optimize more of it especially in this particular case that this particular。

 case is very special of fetching an attribute and calling it and there are a lot of special。 optimizations just for that case but by caching the method and separating them those optimizations。 will no longer apply and now with these modern Python implementations caching this method actually。 slows down your code by a fair amount。 So the situation is getting pretty complicated so to be clear this is just for functions on。

 built-in types like list if we look at Python types the numbers are a little bit different。 it was a little bit less of an improvement before versus the built-in type but the improvement。 is even smaller now as basically as attribute accesses get faster the benefits of this approach。 gets smaller。 I looked at another case which is not exactly the same but looks very similar which is where。

 you're calling functions of modules and I picked the function that I could find that。 does the least amount of work I think math。square root there's a square root instruction so this。 is a single instruction everything else is overhead so this is the maximum amount of。 improvement that you'll ever see from this case。 In Python 3。

8 it was pretty big and in these new implementations it is considerably smaller。 maybe you consider 15% to be enough but it is quite a bit smaller than before。 As I said math。square root was picked because it is the fastest function I could find。 If we look at a more typical function say OS。path。join the improvement was much smaller。

 before and again the improvement has decreased a lot。 Now you're talking about a 。4 to 2% improvement by doing this。 I shouldn't use four different cases there's different numbers for them sometimes it helps。 a decent amount sometimes it helps a tiny amount sometimes it hurts。 What is the takeaway？

 What should you do？ My personal advice now is just don't cash methods anymore。 I think it's not worth the mental overhead I don't think it's worth the readability。 hit I think the improvement will get smaller and smaller as the implementations get smarter。 and smarter。 I think you don't have to rewrite it if you already did it but I think in general this。

 is something that we can leave behind as the implementations get smarter。 In this side I'm calling out a couple other dynamic features I'm not going to go into。 them a lot but the point I want to make here is these unlike the ones I talked about before。 these were already expensive before but now these particular features are getting even。

 more expensive because they inhibit other optimizations。 So not just are they expensive when you use them but they might affect other parts of your。 code that can no longer be optimized。 In particular a problem that we need to solve as a community is attaching a profiler to this。 optimized code at least in piston I don't know about faster C Python and cinder will。

 tend to just disable almost all of the optimizations。 So the code that you end up profiling might look very different than the code that you。 meant to profile with optimizations on。 And this is in general a hard problem and we might need a new profiling API I don't know。 exactly what it will take but this is an unfortunate part of the situation。

 The last thing I wanted to talk about is the situation of C extensions versus pure Python。 that generally C extensions are thought of while either as bindings to another language。 or as a way to speed up Python code。 A common piece of advice is use Python converted to a C extension stuff like that but this situation。 is getting pretty murky now because all of the optimizations that I've talked about today。

 they only currently only apply to Python code。 So this means that C extensions do certain set of optimizations。 The Python interpreter does a different set of optimizations and is very dependent on your。 code which set of optimizations helps your code more。 And unfortunately it's very hard to give a good rubric for this case you definitely should。

 do it and see this case you should definitely do it in Python。 But to illustrate this I did I took the benchmark from before the attribute lookup benchmark。 If you remember this was this 18。4 nanoseconds was the amount of time it took before。 And I used Python to convert this benchmark to a C extension and then I ran it in Python， 3。8。

 And indeed it does make it a fair bit faster and so that advice was good before that converting。 it to a C extension is good。 But if you remember the side of the other numbers the other implementations sped up the。 benchmark much more than Python did。 And then also the optimizations that these implementations do don't help Python at all。 So Python could adopt all of these same optimizations。

 I don't think there's a technical reason that they couldn't。 I think it's just I think it will probably happen over time but they currently don't。 So this means that if you're particularly hitting the optimizations I've talked about。 today you might actually be better in Python。 As I said there's not a really clear cut rubric for when it's better one or the other。

 If I had to say I would say that object oriented code is going to be helped a lot more by the。 new interpreters and numeric code is going to still stay best in C code。 But this is something that you're going to have to verify for yourself because as I said。 it's fairly complicated at this point in time。 Which means me to my last point which is that unfortunately there's not a lot of help that。

 you're going to get with these kinds of issues。 You know I'm trying to give you the best tips I can now but these things are changing rapidly。 There's all these corner cases and there aren't tools that will say hey this was slow because。 you did this over here and so this optimization was turned off。 That stuff doesn't exist yet。 So your only method of telling how to optimize your code is you're going to have to benchmark。

 it and see what works and what doesn't。 So to wrap up my talk as I said in the beginning the general idea is that we're trying to make。 Python programmers not pay for dynamic features they're not using。 And this is great but it adds this new complexity that you get rewarded if you are if you do。 think about these dynamic features and trying to not use them。 I put the GitHub links up on there。

 You can find all these projects they're already available in different forms。 And I believe that these are also the best ways to get in touch with the relevant teams。 And so I work on piston if you want to reach out to me online you can go to that the piston。 GitHub page and then I will also be hanging around here after this talk。 Thank you。 [Applause]。

 Thank you Kevin。
![](img/5165d7baa6c620f50fac7a33077665ca_3.png)

 I would now like to invite questions from the audience。 Do you think the rest extensions will be any different than the C extensions given like。
![](img/5165d7baa6c620f50fac7a33077665ca_5.png)

 cryptography starting to use rest libraries underneath？ I don't think so。 I think the only thing might be I think these optimizations are probably too much work for。 individual C extension writers or rust extension writers to do。 If I were to guess at how they would start appearing in C extensions it would be that。

 a intermediary tool like Python would adopt them。 So in that sense unless Python can generate rust extensions it might be a little bit slower。 going to rust but at a technical sense there wouldn't be any difference。 Would it make sense to move your dynamic features in a different file or a different。 class and the not so dynamic ones somewhere else can separate the concerns or give the。

 optimizes more way to do so just move your everything is dynamic to one place and not。 the other place instead of mixing them。 Would it help？

 I'm not sure exactly what you're proposing but I think the general idea is very much， happening。 We're not doing it in piss-in because we don't want people to change their code but。 cinder has this thing called static Python that might be similar to what you're talking。 about where you sort of commit to not using certain dynamic features and then the compiler。

 is able to speed it up even more。 Is that what you're talking about？

 If you use one of the dynamic features that you kind of instead of one class you have two。 classes one that use dynamic features the other than not that you can speed up the one。 that doesn't。 Would it help？ If you can do this easily if it just fits your if you have a design decision to make and。 it wouldn't be any more work just to split in two classes for instance。

 Yeah I guess that could work if you have some type that you need to update a lot then。 I guess you could have a different type with instances that you look up things a lot。 I think you probably have some trade-off with readability and maintainability but for performance。 that I think that could work for sure。 What is your thoughts on optimizations like using my pi c and stuff like that？

 Like passing your programs into my pi c。 Yeah I'm not super familiar with my pi c in general but as I kind of alluded to earlier。 in the talk there are a lot of tools that there's sort of a spectrum of how much Python。 versus how fast they are and so the more Python you support the less fast you can go and if。 you support less Python you can go way faster and so it's kind of nice that there are options。

 all along that trade-off curve that you can use。 I assume that my pi c has some sort of issue with supporting everything and that's why。 we don't just run everything through my pi c all the time but that's a little bit of speculation。 on my part。 So I was wondering as far as typing is concerned as things get more static I know in some languages。 like in Scala where types are sort of pseudo dynamic the type inference engine can get。

 pretty slow when things are sort of missing。 I was wondering if that's something you know have you all seen any sort of performance。 issues in terms of the performance or you know doing type inference on the fly？

 I would say that we do very in piston talking just about piston now we do very lightweight。 profiling which is basically for each bytecode what did we see happen at that bytecode and。 it is quite cheap to do that there's no cross function analyzing whole program analysis anything。 like that。 I don't know for the larger projects in terms of like actually the type infincers or the。

 static Python project that I mentioned but for us it's not really an issue。 Anyone else？

 If you have any other questions I would suggest talking to the speaker after either outside。 this space。 Great， thanks。 Thank you。 [Applause]， (clapping)。