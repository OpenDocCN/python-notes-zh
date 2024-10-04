# P60：Talk - Moshe Zadka_ Best Practices for Continuous Integration in Python V02 - VikingDen7 - BV1f8411Y7cP

 So I want to cover what are the steps I'm going to go through in this talk。
![](img/0b9fcf17d06e5a70dccca14da249704e_1.png)

 I'll start by diving into what are the functional bits that make up a continuous integration。 system because those are the functional bits that we want to apply the best practices to。 Then before we talk about how to apply those best practices to， we'll talk about why we're。 going to apply the best practices to。 So what actually makes one continuous integration set up good and another bad？

 So what are the criteria we're going to evaluate？ And finally。 after we've all agreed about what bits we can control and what are the targets。 we'll actually come to what I promised I'm going to talk about and explain how to make。 your continuous integration actually good。 So before you even talk about having a continuous integration for a project。

 you usually are， going to have some sort of a local way of building your project。 So。 basically the interpreted language， we don't have a traditional build step， we don't need。 to compile anything， you know， usually。 But we do have steps that are pretty traditional to run and I kind of think of them as the build。 We usually want some kind of linting process。 Pilate， flakeate， sometimes my pie。 Under linting。

 I cover anything that will look at your code but will not run it。 So anything that analyzes your code statically。 Obviously。 the other thing that you can do is run your code and the other thing you can， do is test your code。 Test is anything that runs your code or pieces of your code and looks at the output and sees。

 if the output is okay。 So， we usually do that when we want CI and before we have a CI。 you usually want to already， have some way of running your tests locally。 And we usually have some sort of packaging。 Now， sometimes you'll package it into a wheel or an S-dist but this does not have to be the。 case。 Sometimes you'll want to package it into a PEX or maybe a Debian package or an RPM package。

 or very commonly a Docker container image。 But these three things。 they're not always present but very frequently you'll find that。 most projects will have all of these three。 And before we even run CI。 we want to make sure that the project knows how to lint itself。

 to test itself and to package itself。 So let's assume we already have a project that does that。 Well， we can start doing CI。 And so， I kind of gave the thing away， right？ I said， well， you know。 you run it locally， it's not the CI。 So clearly CI should be something that you run remotely on a server。 Well when you run it locally， the answer to when you run it was either very simple or。

 very complicated。 The simple answer is when you wanted and the complicated answer is human psychology is。 really complicated。 I don't know when you wanted to do it。 But at the end of the day， you know。 it's at least obvious。 When you wanted to do it， you just did it。 When you run it on a server。 the server has to have some sort of well-defined algorithm， when do you run that build。

 And that will have to be defined via code， so that has to be very clear。 In the old old data。 like let's say circa 2006， having a nightly build was considered。 this is where the term continuous integration came for。 You continuously， every night。 every night you run the build and teams did that were， like sophisticated。

 They have an automated build process that runs every night on a server。 That was like the height of sophistication in 2006。 And then it was good， right？

 Because then in the morning you can come in and say， okay， oh， something broke。 It was one of the things that were done during the day。 You should remember that 2006 is a long time ago and most teams were very local and like。 there was a specific definition of time， right？ Like the time zone of the local team。

 So the night is like whenever the team is asleep。 And so they would come in the morning。 see that something broke。 One of the things that happened during the day。 it might be difficult to figure out which， of the things during the day。 So someone said， hmm。 I have an idea。 What if instead of waiting until it's night time。

 as soon as they've emerged to the main， branch， we'll run it。 Then if it's broken。 we'll know that that's probably whatever that merges。 This is like， let's say in 2008。 if you had that， you were like in the top 95th percentile， of development sophistication。 People were like， oh， that's a team I want to join。 Clearly they take their code seriously， right？

 But we keep updating our standards。 And nowadays， we usually think of what we want to run the contingency regression on or。 something I want to think of as suggested patch。 Different systems will have different names for that。 In GitHub， it will be called the pull request。 In GitHub， it will be called the merger request。 If you use review board， it's a review request。 But whatever it is。

 it's the thing that you ask your colleagues to review as people will。 also be the thing that you're going to run the CI before you merge。 So if it's broken。 you don't know that fact after you have merged。 And now you have to fix something that's broken for everybody。 But you know， ahead of time， this is let's say， you know， started kind of being like kind。

 of standard practice， sometime between 2013 to 2015。 And by now。 every reasonable system that you use will have that as a built-in feature。 The situation hasn't stopped。 Right？ This is already kind of 10 to 7 years ago。 There's like more things that can be done now。 People talk about stuff like merge trains。

 something called not-wacket science， which is， a very weird name for something。 All kinds of stuff。 I'm not going to dive too much into that。 Another thing I'm not going to dive too much into that is continuous delivery and deployment。 That's not because these things are not important。 But this talk is limited in time。 If I try to cover everything， it would take way too long。 So in this talk。

 I'm going to focus on these suggested patch builds。 And the reason I'm going to focus on these suggested patch builds is that in a typical， project。 your continuous integration system， CPU cycle for CPU cycle， will be spending most。 of its time in that flow。 Right？ Sure， this will eventually be merged to main。

 and it will run CI on that merge to main。 But usually a patch takes a few iterations。 So on average。 most of the flows that go through your CI are going to be on that suggested， patch build。 Which means that the main thing you're going to want to optimize is that flow。 So I'm going to focus the entire talk on just how to optimize that specific patch build。

 And to come back to like， why are we doing that， we usually run it as an automated gate。 Right？

 We have a human gate。 Why is the human approval process that will usually mark off as approved。 Why is it this code looks good to me？ And the goal of the CI is usually to look for regressions。 Right？ It can't really say if the patch is like accomplishing its goal in the abstract。 Right？

 That's not something you can really implement in a computerized manner。 Right？ What a CI system。 the goal of it in a， one of the suggested patch build is， is this making。 any of our things worse than they were before？ And so this is useful because this means that like anything that has passed this gate will。 at least not make things worse off。 So this is a goal of this suggested system。

 So now we know what it's supposed to do and we know what we're talking about。 So this is what we're going to focus about。 How do you make your CI system be a better automated gate for patch being not regressions？

 So how does it do that？ Well， your CI system is usually made of a coordinator that look at suggested patches。 and decides， okay， that suggested patch has now， I should really do like these ones on， it。 And then the CI runners will actually go through the flow， run your tests， run your linters。 and send the output。 And in most modern frameworks， again， regardless of which ones you use。

 that output will be， consumable via two ways and both of these are important and it's going to be really relevant。 in the， you know， press of the talk。 One is that they're going to be collected live from the runners。 So as the build is running， you can see the output， which means if there's a test failure。 you can see it immediately， but it's not just live， it's also documented。

 It's also retained forever。 Now forever is obviously like somewhat of a nebulous concept exactly when it's backed。 up， what's going to be paid。 But it's usually considered in most systems a reasonably long term place。 right？ You can have a permalink in it that you can link in a discussion and say this build shows。 you that this test did something， right？ So this is kind of an important thing to kind of remember about the build logs in the continuous。

 integration system。 So now that we understand what the continuous integration system is made of and what it's。 supposed to do， let's figure out what we can do to， what kind of continuous integration。 systems are better than others， right？ What do we make it good， right？

 Even if we don't know how to make it good， what we make people say this is a good system。 you have improved the situation。 Well， there's a few different pretty orthogonal criteria that you can apply。 So one criterion that you can apply is accuracy， right？

 We said that its goal is to say does the patch introduce a regression？

 So accuracy means that the answer is correct。 If it says that the patch introduces a regression。 it introduces a regression and if it says that， the patch does not introduce a regression。 the patch does not introduce a regression。 Any time that it gives the wrong answer。 that's an accurate answer， that's an incorrect answer。

 Clearly the perfect continuous integration system is 100% accurate。 A system that is more accurate is better than a system that is less accurate。 So this is a criteria。 right？ This is something that is good to have。 But another orthogonal criterion is actionability。 Again， CPU cycle for CPU cycle most of the time when you run a CI system， it will say。

 that most of the patches are bad。 That's reasonable， right？ Otherwise。 why would you have a CI system？ If most of the time it would approve。 then you should probably run it less， right？ It's probably doing a lot of redundant work。 So in general， a lot of times patches are bad。

![](img/0b9fcf17d06e5a70dccca14da249704e_3.png)

 That's okay， right？ This is why we have this automated system to give us feedback。 But this means that when the patch is not good， it's really useful if that system gave。 you good information about how to fix it。 Now， the optimum here is something like black。 which actually generates a diff for you。 But that's probably unattainable。

 So what kind of an attainable criterion we can have here？ Well。 the best of all system tells you exactly how to reproduce it locally。 Once you can reproduce it locally， the CI system is not in the picture anymore。 Right now。 how actionable it is is kind of up to your local development environment。

 You can use local debuggers， you can put in print statements。 Whatever used to debug your local code is up to you， right？

 That's already like a language stack issue or a development stack issue， not a CI issue。 So best of all， if a CI system tells you how to reproduce it locally， if for some reason。 it's very hard to reproduce locally， it should probably give you a lot of useful information。 Another， again， orthogonal criterion is how long does it take towards answer， promptness， right？

 Imagine that you had a system that is perfectly accurate and perfectly actionable， meaning。 that it always gives you the correct answer。 And when the answer tells you like this is a bad patch。 like you can either reproduce， it locally， but it takes the whole day to run。 That's not a good system， right？ Even though it's perfect on the first two criteria， right。

 that's not a good system because you， wouldn't be able to develop effectively with it。 So promptness is an orthogonal criterion here。 And the final criterion I'm going to talk about is costs。 Right？ Again， imagine that you had a system that is perfect on all the other three， but costs。 a lot of money。 Well， we live in a capitalist society， right？ We have to pay money for these things。

 Basically， the most money you spend in kind of a modern， kind of flexible， cloudish environment。 is the one-on-the-compute cost。 If you're spending more money than your product brings in。 then this becomes an affordable， system， right？ And that's kind of like the limit case。 Clearly。 in an ideal case， you want to minimize the cost because this is your profit margin。

 on whatever product DCI system supports。 So now that we know what our goals are， finally。 we can talk about how to actually， improve them。 So well， you can improve the accuracy。 You can improve the actionability。 You can improve the promptness。 And you can improve the cost。 So one of the biggest things you can do to improve the accuracy is to use containers。

 And the reason is because remember that we said that CI， the goal here in the gate function。 is to tell you the regression。 Now if you update to a new Python version， right。 even if it exposes a hidden problem， with your code， right。 even if it's a new Python build that uses wider characters and。

 it exposes like some problematic stuff with your code， it's not a regression in your patch。 That patch should not be not allowed to come in。 So you want to use container image that has a specific version of Python and a specific。 build of Python。 And any other non-Python dependencies。 And then you want to pin the image tag。 right？ If you do all of that and then pin to latest， you've undone all the work， the hard work。

 you've done。 Because as soon as a new image is released， you've upgraded to the new image。 And again， you might fail perfectly valid patches。 Now a typical Python program has dependencies。 Even pretty small things can have like 10， in like their recursively closed dependencies。 Medium ones can have 100s。 I've seen definitely ones with like thousands。

 If any of those versions update and that somehow causes a problem， again， it doesn't。 matter if the problem is in the library or in your code。 This is not a problem with a patch。 You should test against the pin dependencies because otherwise you will be giving the wrong， answer。 You will be saying that the patch is wrong but the patch is absolutely fine。

 So you should always test in your CI flow with patch versions against pin dependencies。 Now you don't want to upgrade the pins and you want to upgrade them in a dedicated patch。 And if that patch fails， the CI system has done its job。 That patch will not be allowed to merge。 That patch does indeed introduce a regression and until you fix it， you should not merge， it。

 You should probably fix that at some point but you should follow the usual flow about how。 to fix problematic patches。 There are services that will help you。 The only thing that will help you is to produce your PRs。 That's not nothing but you are still responsible for looking at the failures deciding when to。

 merge and so on。 So whether you use a service or not， you should still go with this flow。 Test quality matters a lot for the CI accuracy。 If you have bad tests。 they're going to reduce your CI quality。 But test quality is its own not half hour talk but at least 10 hour tutorial。 From the CI perspective， what you want to do is make sure that you monitor and improve。

 the test quality。 The CI is in a unique position to look at the test and see the historical data and see。 which tests are good and which aren't。 You want to make sure that you do that。 Again。 I'm not going to cover exactly how to optimize that。 For improving actionability。 the biggest thing you can do is set the verbosity to the highest， level it will go。 Any test runner。

 linked runner that has verbosity options， set them to the maximum it can go。 Because you can filter out stuff but you cannot filter in stuff。 Which means if you had a failure。 especially if it's hard to reproduce failure， you really， want to know what was the problem。 So the higher the verbosity， the better。 You can filter the logs if you really need to。

 A lot of the time failures will be in tests。 Now frameworks like PyTests or Hamcrest will help you to make sure that the assertions。 are more verbose。 But they are there to help you。 They are not there to do your job for you。 You can still use PyTests assertions or Hamcrest assertions and not get verbose test outputs。 if you use them badly。 Make sure you cause your test to fail locally。

 Like the assertion failure actually has enough verbosity if not to fix that。 That is extremely useful。 That is time well spent when you write a test。 One thing that PyTests or any of these frameworks will not help you with is when tests fail not。 because of an assertion failure but because of an exception。 Which means make sure in your code。

 When you raise an exception add a lot of details。 Because those details those frameworks will show you。 But if you don't put those details in they will not be able to show you those details。 So this is really important。 And we talked about how a lot of actionability comes to local reproducibility。 Spue as many details of the environment to the logs。

 You can do it in the beginning of the test one so they will scroll off the live one and。 won't bother anyone。 But when you have a failure and whoever is trying to reproduce it locally is having problems。 they will scroll to the beginning and read carefully about what is different between their。 environment and the CI environment and you don't know which line is going to save them。

 So of course environment variables are really crucial because you don't know which environment。 variable is going to make some library deep deep deep in your code behave slightly differently。 Any details about the platform version of libc version of operating system and so on。 And anything else you can think of just spew your heart out。

 As a corollary you're spewing all these environment variables。 If you put secrets in them you're going to have a bad time with your secret licking。 The solution is not to do all kinds of fancy masking。 The solution is don't put your secret in environment variables。

 The better place to put your secrets every reasonable CI system will give you better place。 to put those secrets in。 Environment variables are really scary because there are other things that will spew environment。 variables so you're not actually helping yourself。 You're just delaying the inevitable secret。 One way to improve partners is to cache aggressively。

 A lot of CI systems will have primitives to cache all kinds of downloads in a local file。 systems that will help you figure those out and start using those。 If you build containers it's kind of annoying to set container layer caching against the， registry。 But if you go through that flow it's going to save you a lot of time because unlike when。

 you build it locally those CI runners are often very similar so they will not have those layers。 cached in。 But very commonly people order their container build files to have the least changing and。 most time consuming layers at top。 So if you do go through this flow you can improve your container build speed a lot。 And have local PyPI container caching proxies that are network wise close to the CI system。

 Another thing you can improve on this is pooling。 Aggressively we use connections， downloads， etc。 Think carefully about how to break up tests that actually improve things。 You can fail fast。 If you use all kinds of libraries to order tests based on likelihood to fails， a couple。 of open solutions I will mention them in my talk notes。 You can also parallelize ones aggressively。

 You can use CI primitives for that or you can use tester primitives for that。 Again。 some configuration may be needed。 It's often worthwhile。 You can mock out those things especially like databases。 You can get something that smokes out the database and doesn't run to a file system。

 You can mock out a file system by mounting something like Tempeface which won't write， to disk。 All kinds of tricks like that。 You can also kill useless ones to improve the cost。 If a committee is added to a patch nobody is going to care about any ones that are in。 the previous patch。 It's annoying to find those ones and kill them but that can improve your cost a lot。

 You can stop runs early。 That's a bit of a trade-off because sometimes you don't get all the failures and you'll。 get to a flow where people do more patches。 You should monitor that and see if it actually improves things。 Even if you do better stabbing and mocking that will take less resources from the environment。 But you'll notice that a lot of these things come with trade-offs。

 You can have better accuracy by writing more tests or making sure that they're really good。 You can focus a lot of actionability but you might lose on promptness。 You can focus a lot on promptness but you might lose out on cost。 You can focus on all of them but you're going to put it out of effort。

 You have to make a decision what you value。 If you don't make a decision you're still making a decision by your actions but it's。 going to be re-litigated every spring。 You're much better off actually deciding what are your values。 Once you have your values in hand， once you know what you care about you can measure。 what you have and now that you know what you value you can see whether your measurement。

 aligns with your values。 And if they don't， now you know what to improve。 The thing that least aligns with your values is exactly how much effort to put in。 So putting that amount of effort and deleting it by how much you want it。 But you also want to repeat this process。 You want to keep thinking about whether your values should change and you want to see whether。

 the situation changes。 Both of these things are likely to change。 That's okay。 This is talk about best practices。 The only way to do that is to practice them。 So I hope that you'll apply it to your favorite CIFlow。 I love talking about this。 I just stood out here and talked to you about this for 30 minutes。

 If you want to continue the conversation， again my website is COVIDism。com， email LinkedIn。 and like 2，000 other ways of reaching out to me out there。 And we also have an open space to borrow it at 11am。 Check the board for what room it is。 And I would love to talk to you all about more about that。 Thank you all。

 And I hope this was a good talk and I hope you enjoy the rest of your conference。 [APPLAUSE]。