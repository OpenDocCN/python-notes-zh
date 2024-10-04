# P39：Talk - Henry Fredrick Schreiner III_ Building a binary extension - VikingDen7 - BV1f8411Y7cP

 Good morning everybody。 Thank you so much for being able to join us this morning。 We have。
![](img/738d8639b8493010cb054fb10c6b232c_1.png)

![](img/738d8639b8493010cb054fb10c6b232c_2.png)

 one more talk and then glorious lunch。 I know you're all looking forward to it but hang。 tight because this promises to be really， really cool。 So today we're going to hear from Henry。 Schreiner building a binary extension。 Henry Schreiner is a computational physicist/research。 software engineer in high energy physics at Princeton University。 He specializes in the。

 interface between high performance compiled codes and interactive computation in Python。 in software distribution and in interface design。 He's previously worked on computational。 cosmic ray tomography for archaeology and high performance GPU model fitting。 He's currently。 a member of the IRIS-HEP project developing tools for the next era of the Large Hadron。

 Collider LHC。 Ladies and gentlemen， Henry Schreiner。 Thank you。 So I'm Henry Schreiner。 and I'm working with the IRIS-HEP project which is basically trying to build a set of。 Python tools to replace and augment the existing C++ that we've been using in energy physics。 since the mid-90s。 We actually have a C++ interpreter if you probably don't want to know that。 So。

 we've been building up these tools and one of the things that was really important with。 this is we have very high performance requirements in certain areas。 We have the largest academic。 data set in the world for example。 And so with this it becomes really， really important。 to be able to do some of the things you'll be seeing today。 So I'm going to start with， a question。

 Is Python fast？ So of course this is a completely ambiguous question。 A good， way to start。 But let's just take an example here。 This is an article that came out fairly。 recently within the last two years。 And they had this problem where they're projecting。 a billion cells and they had this Fortran code， 1500 lines， the Fortran code that did this。

 in six hours and 30 minutes。 And finally they decided to rewrite this in Python and when。 they did that it took four minutes。 And the reason for this was really they had this high。 level language。 They had libraries that they could access。 It allowed them to easily explore。 different algorithms。 If you have a billion cells you're doing projections。 You really。

 want to use something like a kd tree。 There's one in scipy so they just use that。 And so。 the key behind this is that you have a library and in that library you have compiled code。 that does the hard work for you and you're still able to stay in Python。 So Python has。 really great performance as long as you can find a library that has your desired algorithm。

 in it like NumPy or Pandas or something like that。 But what do you do if you don't？ What。 do you do if you have some sort of algorithm that's not already coded up somewhere？ How。 do you write that yourself？ And that's what we'll be looking at。 So first possible solution。 to this would be you could just dump Python。 This is the Twitter and random article on。

 the internet solution。 And maybe you're enticed by these other languages。 Is anybody enticed， by C？

 Yeah。 But they offer native performance。 Well， Python though is really easy to learn。 It's very quick to write and it has this massive ecosystem。 I would say each of these three。 things are equally important and incredibly important。 So you'll see and this is where。 we'll get with the final solution。 You might already guess that by the title of the talk。

 So you can split the driver code and that's the stuff that usually takes the most time。 to work on and that's what you're manipulating working with。 That can still be in Python and。 then you can just move the performance critical code to something else and work together。 Before we go on though， there's a few other possible solutions。 One is a number。 This。

 is just in time compiler for Python。 If this is just as fast or faster than any other solution。 you can come up with most of the time。 But it does have some downsides。 You are going。 to have to do the just in time compiling on your on the device。 It's a somewhat heavy， dependency。 There's plus sides and downsides。 But if you just want to make it faster， this。

 might be the first thing to try。 This would be the first thing I would try。 Another solution。 is you can just make Python itself faster。 There's a variety of different packages and。 projects that do this。 You have PyPy for quite a while。 PyCon is a bit newer one。 And then。 there's even forks of CPython to try to make it faster。 And CPython itself is actually。

 interested in getting faster now too。 These sorts of things are in the say 5x performance， range。 Maybe five times faster。 At least for CPython。 But they don't really make heavy。 numeric code faster because that's already sits in a compiled language。 It doesn't need。 to be made faster。 You may need more algorithms。 And of course the solution we'll be looking。

 at today is this one。 You can precompile。 So you can write code using the CPython API。 or you can have maybe code sitting in an interface somewhere。 And then that can be compiled into。 an extension。 And you can then distribute that。 And it turns out that a lot of things。 you use every day are actually done for those things。 And this works very， very well。 So first。

 let's go into a little bit of what a binary extension is。 If you're using Python now， you。 probably think in terms of you have some code you're writing and then you're using libraries。 And if you were to write your own library， you probably start with this first one， this。 side on the left here。 You put all your library code in Python。 Well that code in the library。

 can also be compiled code。 It can be a binary extension。 And the first one is really easy。 But the next one traditionally has been viewed as complex。 The examples tend to be rather poor。 Tinn to be things that a really large library like NumPy would do。 And there are lots of。 details in building these and distributing them that for example you need to compile。

 on every different platform you support that sort of thing。 But you can achieve really。 high performance。 And one of my goals in this talk will be to show you that this is actually。 not that hard to do。 You can do this yourself。 And the tooling around this and all that has。 become very， very good。 And it's really not as complex or as hard as it used to be。 Okay。

 So here's an example of what of wheels。 So I start my example with wheels for showing。 you something that's not a wheel。 That's an S-dist。 And that thing has a setup。py or。 a pipe project at Tomo or some collection of tools that allow a back end to build that。 Okay。 So that's where your source code sits。 Then you have this。 This is a pure Python wheel。

 This is something usually that just contains Python。 You'll see the name there。 It has。 the implementation API and platform。 And that's just Py3。 None。 Okay。 And this doesn't have。 compiled extensions。 And then you have wheels that look like this。 These are the ones that。 have binaries in them。 And you see you have the implementation， the API。 Those have actually。

 been same most of the time in modern Python。 And then you have a platform which is your。 operating system and your architecture。 Okay。 And Pip will actually do the smart thing。 It'll pick the most specific ones。 So in my Py's case， they actually have a pure Python。 wheel and they compile。 So that way if you're not on one of these systems， it will fall， back。

 But most of you will probably be on one of those systems。 There's only a few cases。 where you end up falling back。 So things like NumPy are shipped。 There's no pure Python。 version of NumPy and they still ship quite well with this。 Okay。 And we'll be looking。 at how to make this third one today。 So I've already gone over one of the two main reasons。

 to compile。 You can get performance。 It's also useful for code reuse。 You can， there's。 already great libraries out in these other languages。 You can just use those in your compile extensions。 You don't have to rewrite them。 Or maybe you want to write something that works in both， languages。

 So this is just a collection of some different libraries that support or that， ship binary wheels。 Things like PyTorch and MyPy， Pandas， NumPy。 And you'll see non-scientific。 numeric things in there as well。 You see UV loop and twisted。 Web sockets。 Okay。 So we'll。 start this talk also with a few disclaimers。 This will be a bias talk。 I'll be showing。

 you things I work on。 But I like those projects enough to join them。 And those are the things。 I know the most about。 And we'll also be looking at good practices。 Maybe they'll be best practices。 But this is definitely not the only way you do things。 Okay。 So you might be now saying， to me。 well， but it's hard。 Compiling， compiling code is hard。 It requires that you do a lot。

 more than just shipping a Python extension。 That's true。 But let's divide this problem。 up into three stages。 You've already sort of seen them。 First you have the actual writing。 of whatever you're interested in。 So that could be something that's written in C++ or。 it could be something that is in something that's like my Pis C。 So you write something。

 that looks like Python and you still want to compile it。 And then once you have something。 you want to compile， then -- and notice I'm skipping things like just here。 Because those。 are not -- those don't fall into this area。 And then you have a build system。 This is something。 that takes that code and produces the binaries。 Traditionally， this has been set up tools。

 and disutills。 But this is starting to become a more interesting space and a little less。 tied to just that one specific view。 You'll see that again in a minute。 And then you need。 a tool to do the wheel building。 Because once you have these things you need to produce。 wheels that are redistributed that can go out to all the different platforms。 And we'll。

 be focusing primarily on these -- this path through here。 There's lots of different paths。 we could take。 But this is the one I'll be focusing on today。 But it's -- I like this。 because it's a mix and match。 You may find some other binding tool or coding tool better， for you。 but you might still want to build wheels。 That sort of thing。 Okay。 So starting。

 with this bindings and coatings， there's sort of two choices here。 You can access existing。 compiled code。 So some of the tools sort of focus on that。 How do you get C++ or Go or。 Rust or something out？ And there's also from scratch code which is code you want to write。 and you want to make it faster。 You're not trying to reuse an existing algorithm。 And。

 you probably want something that looks like Python。 So， Python and my Pis C， things like。 that tend to be in this sort of this category。 And you might even be able to make your extension。 optional a bit easier with this one because it may be even the same code。 So starting。 with this idea of interfacing the existing library where you're not actually writing， the binding。

 you have this sort of this side。 That's how you would load a library from C， types。 And then you have to build that shared object library somewhere and you have to do。 all of the work yourself。 And then Python also provides -- C Python provides this way。 to write your own extensions。 And that would be writing this。 Now， most of the time you。

 should probably should not be writing this by hand。 But I'm just sort of illustrating。 what you would need， what all these binding tools are actually doing behind the scenes。 They're writing stuff that looks like this。 And there's only sort of two important things， here。 And it's this -- the fact that you have this name that's dependent on the name。

 of your module and that's how it knows to look up what it has and then you list what， you have。 Staying on the C interface side， let's just briefly mention a couple options， there。 C types is built in。 It's very simple， but you're really -- it's up to you to get。 everything right yourself。 So it's probably a good idea to try to wrap this in something。

 so that you have some sort of nice interface for your users。 And -- because it's not going。 to protect you if you do anything wrong。 This is a good way to get SEGFOLTS in C Python。 If you make a mistake here。 And CFFI is a tool heavily for that -- I think came out。 of the PIPI there。 They built sort of this tool that reads in the C headers。 It's a library。

 you get on -- on PIPI。 PIPI。 You know I do that。 And you can sort of generate some of。
![](img/738d8639b8493010cb054fb10c6b232c_4.png)

 this automatically and get some extra safety there。 Going back to the sort of traditional。 ways to do this， Numba actually also has the ability， besides being a jet。 I remember I。 said it would not be in this area， but it also has a head of time compile which looks， like this。 Where you can take a little function。 You can put a export on the out and you have。

 to tell it what the module name is。 Remember it has to get that module name in order to。 produce a extension because that's part of the interface。 And you could compile it。 They。 have this extension for just utils that you can just stick in there。 And then you could。 actually just run this and compile this ahead of time。 Ship it as a wheel。 It would not require。

 Numba。 This is very limited， but it's an option。 And I've seen it used at least once in a。 real situation。 Another option is my pi C。 So this really piggybacks on top of modern。 Python typing。 So for example， you see this on the left。 You see a little piece of code。 It has type hints。 It's valid Python。 I can run this with Python。 You see Python fib。py。

 Or I can compile it with my pi C。 And then I can run it and it will be 10 times faster。 Because it's actually compiling the function in the middle。 For comparison， if you did this。 with the JET， this would be 35 times faster。 You can't do it with the head of time compile。 because recursive is not supported in that head of time。 Compile that you saw。 Another。

 popular option is fast not quite Python。 And that's Python。 This has been around for a very。 long time。 It's a very good tool for doing this。 You can see some code over on the right。 This is not probably what you weren't used to seeing with the Python code， but this is。 actually valid Python and Python code。 There is an interface to do that。 There's also sort。

 of this weird hybrid between C and Python that they provide。 So it also has a custom， language。 That's what you see a bit more often。 And there's the code for running it， at the bottom。 And that's nine times faster。 So it's a little bit slower than the my pi。 C example for this little piece。 And it does have -- Python does have some downsides。 It。

 can actually bind C and C++， but it wasn't really built for that。 It will be very verbose。 if you try to do that。 And it's got a few caveats and drawbacks。 But it's really good。 It was really designed sort of to allow you to write Python and then make it much faster。 and compile it。 It works well for that。 Okay。 And then pi by 11 is a header only P or C++。

 interface。 And just to unpack that a bit， it means that it's really trivial to add it。 to a C++ project。 There are no special build requirements。 It's just C++ 11 or newer。 And。 there's no precompile phase。 You don't have to siphonize or use one of these other sort。 of tools to prepare it。 It's just straight C++。 And you can kind of think of it as if。

 it was a C++ API for Python itself。 And it was really designed just to do the binding。 It was not designed to do these other things。 So here's a little example of what it looks。 like to use。 Pi by 11。 You include it。 You have your C or C++ code that you're interested。 in binding。 And then you create a module。 Remember， it has to get the name of that module， somehow。

 So this is how you're telling it what the name of the module is so it can generate。 the correct entry point。 And then you just take your--the M is the module object in Pi， by 11。 And you just can define an add。 And it will actually infer the signature and things。 like that from the thing that you pass in。 And you can actually compile this yourself， if you want。

 And this would just work as is。 There's lots of great features of Pi by 11。 And I list a few of them here。 I won't go into them in extensive detail。 But it's quite powerful。 And it does--even supports WebAssembly with PiDye now。 And it has a variety of different。 things that it does。 There is one more I want to go into before we sort of dive in a little。

 bit deeper。 And there is a sort of sequel to Pi by 11 called NanoBind， written by the， same author。 And this is C++ 17 only。 And it's Python 3。8 only。 It's built on top of， the fast call API。 Very similar API to Pi by 11 intentionally meant to be more limited。 And some of these ideas are being backported to Pi by 11 as well。 And I have some plots。

 here that show that it is much quicker to compile。 It was sort of designed with compilation。 time in mind because header only C++ libraries are really slow to compile。 The binaries are。 smaller。 Do keep in mind this is for a very basic example basically like a class and a， class。 So if your binary size is dominated by your binding tool， then you're probably。

 not compiling very much。 But smaller binary size and the runtime performance is faster。 Say if you're looping over something， Pi by 11 is a bit slow because it has a overload。 dispatch mechanism built in。 Which is very powerful and you shouldn't be looping over。 and calling a function over and over and over in Python probably。 You should try to find。

 some way to wrap the entire thing。 So this is an exciting new project。 It's only， a few months。 a couple months old now I think。 So we're going to dive into an example project。 I'm going to show you every single line of， code you need to create a compile extension for all platforms。 everything。 So I'll have， a reasonable amount of code。

 The sort of point of this is to show you how much code， and maybe point out a few things。 I'm not going to walk through every single line。 And。 I'm going to try to do something that's not completely trivial。 I'm going to try to take。 CLI 11 which is a command line parser library。 My favorite command line parser library。 I also。

 wrote it so my UI。 But my stuff terminal uses it so。 So we're going to make a little wrapped。 version of this from Python。 That's not necessarily a good idea but it's a library I happen to。 know。 It's a good idea to know the library you're trying to wrap so you know how it works so。 you know how to expose it。 And we're just going to do sort of this minimal bit here。 So we。

 want to be able to create an app。 We want to add a flag。 We want to be able to check。 and see if that flag exists。 And we want to make sure that sort of basic nice cities are， there。 So we want to make sure that we can print the app and something nice will print。 out and that we get a keyer or something that depends or will check as a keyer out if we。

 make a mistake。 So this is the entire binding code。 We're going to import pibind 11 and it also has some wrappers， for the standard library things。 And a little bit of setup there。 Here we're creating a， new exception。 That exception is going to be， it's going to come from a keyer。 It'll， make a new armodule。option。

found for us。 And that will automatically wrap the exception。 so we don't need to bother with it down here。 We could。 We could throw a py colon colon。 keyer if we want to do。 But this is more declarative which is nice。 And then we create our app。 And。 then in the app we're going to define a few methods。 And so we're defining each one。 Notice。

 we even have some dunder methods there。 We set define get item and stir。 And then the。 thing on the other side can just be a lambda function。 Sometimes， like there in the middle。 you see that overload cast。 There it's actually -- it's binding to the function itself and it's。 overloaded so you have to pick which one。 Or you can just use a lambda function in C++。

 and just throw that in there and let the compiler figure out which overloaded one you're after。 Or you can do more work if you want。 So it's really quite nice to be able to just use the。 lambda functions there。 And that's the binding code。 Okay。 I said I'd show you every line。 of code so there's the init。py。 And also tests。 You should always test anything。 So let's at。

 least write some tests。 So this is basically the same thing I just showed you。 But in test， form。 Okay。 All right。 We're going to leave that for a minute。 When you go back to the。 yellow slides we'll be back in our example。 Let's talk about build systems。 So there are。 some great examples of pure Python build systems that are now using PEP 6。21。 Things。

 like flit and hatch and others， including actually setup tools now。 But building binary。 your binaries your choices are somewhat limited。 So setup tools and disk utils has a lot of。 drawbacks。 It doesn't it is capable of building a file from C or C++。 But it really wasn't。 extended and doesn't add a lot of things just sort of basic things that most build systems。

 would have like multi threaded builds or partial rebuilds and features of your compiler and。 just all these things that are built into tools like CMake and mason and stuff。 You can't even。 tell it what C++ standard you want to target。 It does actually have native Python support， though。 So this then has been extended by a lot of different packages。 So my PIC extends， this。

 NumPy disk utils。 There's lots of examples of things that then write extensions and work。 with this even though it's you're kind of delving into the internals quite a bit。 And then there。 are a few from scratch。 I don't think actually in scans I'm not sure if that fully counts。 because that's also using at least a little bit of disk utils。 I saw disk utils showing。

 up inside its code base。 But I do want to point it out just because it was one of the。 earliest adopters of PEP 517 that supported compiled builds。 But mason py is out now and。 Motrin is one for Rust。 So we're starting to see some of those。 There's also scikit build。 which is sort of what we'll focus on today。 Right now unfortunately it just wraps set。

 of tools but there is a plan to move and I'll talk a little bit about the plans to try to。 get it over into this sort of， onto that side of the screen。 Python 11 actually has this。 nice extension as well for setup tools。 So you can just grab this set of helpers and then。 you can add the， do a pipeline extension， you can tell it what C++ standard you want。

 and just go with that。 If you have something that's simple you can do that。 So scikit build is a tool from the makers of CMake to allow you to use CMake from Python。 And this started back in 2014。 It was announced at scipy。 It was originally PyCMake and then。 it was renamed a bit later。 And two of our most popular packages are the CMake package， for Python。

 So if you do PEP install CMake you get CMake for pretty much all the binary。 and all these different areas。 It has some really nice tooling。 I helped sort of update。 some of that。 And you can do the same thing with Ninja。 You can PEP install Ninja as well。 And that comes from the scikit build project。 And this is really exciting because it would。

 allow you to use a very powerful existing build system， the most popular C++， C， et cetera。 build system directly from Python。 It has a couple new maintainers that joined in the。 last six months or so。 And it's definitely an exciting example there。 And just also if。 you need a refresher on CMake I wrote a book on CMake so you can go check that out。 Years。

 before this。 Okay。 As long as you use modern CMake it's actually quite nice and you'll， see that。 So there are some plans。 The idea is that we'll try to develop a scikit build。 core that will be a 517 builder avoiding setup tools and distuetills。 That will give us hopefully。 we can sort of give a compatibility layer so the existing users will still work。 We'll。

 also have a way to do a direct build。 And then hopefully a proper setup tools extension。 because currently that's still sort of how you put these things together。 But there might。 actually be some generalization that's happening in the， we're going to talk about this a bit。 in the packaging summit later。 So it might be nice if we could sort of generalize and。

 not write an extension for every single system out there like Hatch， Poetry， etc。 And then。 also we need an extension discovery mechanism。 We might be working with CMake itself to do。 this because it would be really nice if we could just stick this in our Python requires。 Say I want to buy by eleven and then just immediately find it。 Right now you have to。

 do a little bit more work to do that and you'll see in the next page that I just avoid this。 entirely in the example we'll be showing。 And you can read more about that proposal there。 Alright。 so going back to our project。 This is our CMake list。 This is the whole thing。 We're actually just grabbing both PyBind 11 and CLI 11 from GitHub because that way we。

 don't have to deal with the fact that we don't have a nice way to grab where PyBind 11 is。 And we have to do this with CLI 11 anyway because it's not a Python package。 So we're。 just grabbing those。 Then we add a module。 This is added by PyBind 11。 We link them。 We。 tell it we need C++ 14 because we use C++ 14 feature and then there's an install。 And。

 that's the CMake list。 Right now you have to write this setup。py as well。 And this is。 what I mentioned about the wrapper for the regular setup。 And we have to stick this in。 Some of this does actually have to be inside the setup。py and can't be pulled out to set。 up by config。 Not all of it but some of it does。 Okay， so those are really the almost the entire。

 build system。 The only remaining thing is we need to also request a psychic build and。 our build system requires。 Okay。 And we'll actually fill out some more of the Py project， at Tomo。 We're not done with that yet。 But we'll leave it there for now。 The next thing。 we need to do is to redistribute。 So we need to build wheels。 So you could distribute on。

 Conda Forge。 I'm not going to go into that in detail just because it's really mostly automated。 You just have to go out and write a recipe and their CI will build that for you。 And you， know。 a lot of the stuff that we do we really want to make sure we distribute on both。 But。 we'll be focusing on PyPI。 And there we have a variety of different things that we have。

 to deal with。 So on Linux we need to build inside of a controlled Docker image to make。 sure that we're not using something that's not allowed in the mini Linux specification。 You don't want to pull something in that or use the version of Glib C that's too new or。 something like that。 So there's Mini Linux and Mucil Linux images。 And you know， there's。

 also multiple architectures for all of these。 And you really should be running audit wheel。 afterwards to make sure that this is packaged up into a nice mini Linux image。 Alright。 mini Linux wheel。 Back OS you now have to worry about the target version。 You have to。 pick what version you target。 It's really a nice feature of Mac but you also have to。

 think about it。 And you also need to make sure your Python was compiled with whatever that。 target was。 So you really want to use the Python。org Python。 You don't want to grab the。 CI or whatever because that was probably targeting the thing it built on which is probably Mac OS。 10。15 or something like that。 But you really want to use the 10。9 when the target's 10。9。

 from Python。org so that you have your choice of what you want to target。 And then you have。 to worry about cross compiling for universal。 And there's a separate tool there。 Same thing。 again for Windows。 Windows is actually the easiest of these because you can pretty much。 grab Python from anywhere and it'll work。 Please make sure you still distribute 32-bit。

 because you still see some 32-bit pythons floating around。 And even laptop I have one。 And then there's beginning to be ARM support on Windows as well。 And then there's also。 there's beginning to be a there's a very fairly young tool called the VEL wheel that's appearing。 here that also helps you sort of package and make sure anything that you use is bundled。

 into your wheel for you。 So I want to go straight into CI build wheel。 This is a tool to do this。 all of this for you。 It supports all the major CI providers and it runs locally。 It ran locally。 for Linux for a long time but it actually supports Windows and Mac OS locally now too。 And this tool has really taken off in the last couple years。 I joined it I think in 2020。

 And then pushed to get this into the PIPA。 It joined and we have over at least I think we。 have at least 600 users now which include almost everything you saw in that first slide。 like my pie and numpies just has just started has just moved to it。 And so this supports。 sort of all the different possible wheels that you're likely to want。 So you can target。

 any version of Mac OS。 You can do Apple Silicon for all the Python versions that support it。 which is 3。8 and newer。 All the variants of many Linux including the ones that you might。 have to emulate or some CI providers have native runners。 Mucil Linux was added fairly recently。 PIPA supported。 And this will also go through do the repair step I showed you。 It will test。

 your wheels if you tell it what command you need to test it。 It will install it in a brand。 new environment and make sure that your wheel actually does what it says it does and passes。 your tests pulled directly from the wheel you built。 And the defaults are all pinned。 You。 can't unpin those but they're all pinned and updated regularly。 And then there's some new。

 features that have come out recently right after we joined the PIPA。 We had CI Build Wheel， 2。 That include our PIPrygetic。tonal support which you'll be seeing in a minute which I'm。 quite happy about。 And then over the last few months we've been adding even more things like。 the overrides which is powerful but I won't be showing you today。 And as I mentioned the。

 local runs experimental windows arm support and though we don't have any runners for that。 yet in CI。 That is。 And in 2。5 which was released this morning we also have a support。 for the stable ABI。 So you can do a limited API stable ABI build and it will build the。 first wheel and then just test all the rest and not try to build it again because it's。

 already built。 And you can also build directly from an S-test。 You can just grab an S-test， off。 PIPI and then run CI build wheel on it and you can produce your wheels。 And we are。 supporting Tomalib now。 A few tips。 This is how you'd run it locally。 When you do it locally。 you just have to and I'm just using PIPX run there but any way you like to run it I would。

 recommend that way。 You just tell it what platform you want to target。 That uses Docker。 for Linux so that works on your Mac or Windows or whatever you are as long as you can run， Docker。 You can build， you can target Linux but you always have to tell it the platform。 and that way it knows that you intentionally are trying to build wheels because it may take。

 a while。 And then this is a really good tip for keeping it up to date。 We do provide an。 action and this will keep your action up to date。 So going back to our PyProject。Tomalib。 I can now finish showing it to you。 Here we tell CI build what command to run for testing。 That will turn on the testing feature。 And then we also tell it that I need to install。

 with the bracket test when I install the wheel because that's where I put the dependencies。 And then this is how you would do this for adding Apple Silicon。 You have to pick for。 Apple Silicon if you want to use universal wheels which have both architectures put into。 one place or if you just want a native wheel。 And so you just opt in for whichever way you， want。

 And that's really the configuration。 There's a lot more options here。 You can。 customize that however you like。 And you can do it through everything can be done through。 environment variables but this is a lot cleaner and nicer。 And lets you run it locally without。 having to redo all your environment variables。 And there's the PyTest stuff。 Okay， so now。

 I'm going to show you the workflows。 This is the longest piece but I think you'll see。 it's not too bad。 First thing here and I'm doing this in GitHub。 You could do it some。 other system if you wanted。 First we're going to do this whenever we click publish or do。 publish from the GitHub command line whatever way you like to do that or if you want to。

 manually trigger it。 And then because this will probably take a while you're going to。 be building a lot of wheels。 Then we're going to build an Estisk。 So that's you can just use。 build for that。 So we have a tool that can build wheels and Estisk and you can just use。 that to build the Estisk。 And PipEx is built into GitHub actions which is really nice and， Azure。

 So you can just do that one line。 You don't even have to get a version of Python。 You can just use PipEx。 It's a supported platform which is fantastic。 It's actually used internally。 inside CI Build Wheels action。 The action is only about a page long。 All right。 And we。 just added something to Knox that does that too。 Okay。 And then you know when to build， your wheels。

 I assume that you would think this one would be the most complex one and。 it might technically be but it's not too bad。 You mostly just do pipac。 I build wheel and。 then that's a good idea to use an exact version there and then use Dependabout to do that update。 for you。 So every time we release it it will then ask you to update and that way your wheels。

 don't crash because maybe we change something。 And that's about it。 We're running on all。 the platforms。 Okay。 And then finally we， you know， assuming you want to upload this， to PyPI。 you could also just download it from the interface and manually upload it yourself。 But you can。 because it's an artifact， but here's how you would upload it and you want。

 to make sure your wheels and your Estisk are finished before you do that。 And we're done。 This is what comes out。 This is what that produces。 This is every wheel and we put a minimum。 version of 3。7。 So this is every wheel。 It supports 3。6 in New York。 And that's it。 That's。 you can see the Mac OS wheels there。 There's the universal。 There's the one for the， for。

 the Intel architecture。 There's Mini Linux， Miesle Linux。 The two different windows ones。 The PyPI for 3。7， 3。8 and 3。9 over there。 They're all there。 If you're in the back， just the。 biggest point is there's a lot of things here。 And you saw how much work had to go into that。 Not that much。 And this scales。 This is something you can do on a very small little project。

 If you look at the CI build wheel examples page， you'll see small little projects。 You'll。 see medium sized projects and you'll see giant ones。 So this， this scales really， really， well。 Simple enough for small projects and powerful enough for complex ones。 Also， maybe。 now that I've given you this sort of intern to what's possible， maybe you could think。

 outside the box and mix and match these things in unusual ways。 So here's an example of clang。 format wheel。 And so that uses a scikit build which actually triggers LLVM's build。 It uses。 the CI build wheel to then build Python independent binary wheels。 You only need one。 So it just。 picks like 3。8 or something。 And then it makes sure that the final names don't have the 3。8。

 part because it doesn't depend on CPython。 But it does depend on your platform。 And then。 these are like one or two megabyte binaries that sit on PyPI。 There's no binding here。 This is only an entry point。 But this means you can just do PIPX run clang format and。 you can get clang format in a second or two on pretty much any system that you can run， on。

 There's even a pre-commit wheel。 You can run this on pre-commit CI yourself。 Before。 this project came out， I've been fighting Conda Forge to try to get this thing under， I。 think it was 500 megabytes to fit into the， to try to get a Conda environment that would。 fit in under 500 megabytes with clang format in it so that it could run it in pre-commit。

 dot CI because there was a limit。 Maybe it's 250。 I don't know。 And you know this was one。 to two megabytes per wheel。 So this is a really fantastic use I think of some of these things。 we've been seeing。 If you want to just quickly jump in and start working on this， I just want。 to do a little plug for something that I work on。 One of the scikit-hep pages， or products。

 from scikit-hep is this developer pages。 I want to try to do a lightning talk on this。 too because I really like it。 And you can quickly make a package using even our， their。 cookie cutter。 So you can just say PIPX run cookie cutter。 Give it there。 PyBind 11 and。 as of yesterday now， CICIT build are back in choice。 PyBind 11 has been there for， you， know。

 pure PyBind 11 setup tools has been there forever。 But scikit build just was added， yesterday。 So we now have 11 different backends。 A lot of pure Python backends， hatch， poetry， whatever。 you know， whatever you'd like， setup tools， two different setup tools， one for PIP， 6。0 anyway。 But。 and then for compiled ones， we also have a multi-in for Rust。 So if you。

 wanted to start a Rust package， this can get you started too。 And all this is， is tested。 with Knox and GitHub actions。 And we even have this also very new repo review page。 So。 you can just type in the name of a repository and you can type in a branch name， click the。 button and then Piedide will run Python 3。10 in your browser and it will then spit out the。

 what guidelines you're following and what you're not from the， from the developer guidelines。 So that's quite， quite fantastic。 I would highly recommend taking a look at the second， app。org developer page。 So key takeaways here。 The code you just saw is， I've just put that。 into the repository。 If you want to look at it and that's， you can just see exactly， you。

 can see it work because it's there and they get to have actions。 Logs and things are there。 And then if you want to sort of find out a little bit more about this， my blog is at， isinempi。dev。 That includes links to the most important things I think that are here as， well。 There's a link to the， to the， to the， to the， to the， to the developer pages。 And。

 there's links to the Piedide 11 examples and to the scikit build examples。 And Piedide。 Piedide 11 has some very nice examples including just using setup tools， using scikit build。 or wrapping， see， make yourself using setup tools。 And then finally to end with a little。 bit about me， these are some of the things that I work on。 These are， these are the projects。

 that I'm involved with and a member in some， in some form or， or some form or way。 If you， go to。 but most importantly again， my blog link which is actually old there， that will， take you there too。 But isinempi。dev or isinempi。gov。io will take you to my blog。 Okay。 And I guess。 we don't really have any questions。 So thanks for your time。 [applause]， Thank you again。

 We do not have any Q&A at this year's PyCon。 So if you do have any questions， for the speaker。 I encourage you to ask them over here outside of this room。 I also just。 want to take a moment before you leave for lunch to give a nice shout out to our audio。 team and to our transcriptionist for kicking butt today。



![](img/738d8639b8493010cb054fb10c6b232c_6.png)

 [APPLAUSE]。