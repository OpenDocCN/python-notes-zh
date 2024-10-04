# P42：Talk - Greg Compestine_ How to Succeed with Python Across the Enterprise - VikingDen7 - BV1f8411Y7cP

 All right， so we're going to introduce the last speaker of our session。



![](img/23eb9d7f75d0dc78c85dba406cbb22b0_1.png)

![](img/23eb9d7f75d0dc78c85dba406cbb22b0_2.png)

 This is Greg Compostin from -- looks like he's from -- oh， he didn't put it up。 Oh， yes。 Bloomberg。

 I could not even read it on the slides。 So Greg is going to give us a talk。

 It's going to be on how to succeed with Python across the enterprise and the subtitle on。

 this one is by trying really， really， really， really hard。 All for you， Greg。 Okay。

 thank you so much for that introduction and I'm surprised with how loud I suddenly， am。

 There is having a little bit of a technical struggle with the display earlier。

 Thank you for being patient with that。 So just a bit about Bloomberg first。

 We celebrated our 40th anniversary just last year。

 We was founded as a pretty small company that provides information to the financial industry。

 And over the years we've grown to be a fairly large and well-established company。

 We have offices all over the world of almost 200 of them。

 They tend to be in real high-priced real estate locations because we're near financial。

 districts and world capitals and things like that。 We have over 20，000 employees at this point。

 Almost 7，000 of them are software engineers。 And quite a few of them are using Python today。

 We've also appeared in television and movies。 So if you have a drama or any kind of television or movie show and it finances part of the story。

 to look authentic， you have to have a Bloomberg terminal on a desk somewhere。

 This is from Billions but our product has also appeared in The Big Short， one of my。

 favorite movies， Silicon Valley and Enterprise。 So for much of its history C++ was the development language of choice at Bloomberg。

 And in the mid 2000s， JavaScript started gaining traction for UI development mostly。

 We did do some server side JavaScript before Node。js was the thing。

 But that aspect of it never really caught on。 Python was starting to make some inroads at around the same time。

 Myself， I began working with Python at another company around 2007。

 Mostly started off doing data massaging。 I did build this one large scale automation of processes that leverage Python。

 And I also wrote this pretty interesting testing framework that isolated some C++ libraries。

 and this really large complicated application and allowed for essentially a level of integration。

 testing that hadn't been possible before that work。 So， well， why Python？

 I mentioned JavaScript was popular but doing some code archaeology in our repos， I found。

 that there's a lot of other languages that the company has investigated or is using to。

 certain extent within the company today。 But none of them have really achieved the level of success that Python has。

 There are three pillars really to how we built up Python to be successful。

 The first was strong community interest。 But this in itself is a necessary but a lot of sufficient condition for success。

 On top of that， we put together organizations that helped us manage communication and coordination。

 throughout the Python community。 And we also organized planned technical support for Python。

 So the community interest started out as it might do at a lot of other companies。

 People were looking at it as this great general purpose tool with the batteries included。

 You could do your data analysis with it。 You could play around with machine learning and other cool stuff。

 And you could stand up web services with it pretty easily。 Unfortunately。

 that wasn't enough at Bloomberg。 The company had a lot of bespoke network and database APIs that were used by most of the。

 applications there。 And so if you do all this work， you're an island unto yourself。

 You really can't communicate the results of your work to other applications within the， company。

 Then as often happens in innovation， an intern project got started。

 And this person took some of the more popular C++ libraries， created a wrapper around them。

 so that they could be embedded into the Python interpreter。

 And suddenly people were able to write real business applications and communicate with。

 other applications in the company。 And things started to take off。 So at about this same time。

 the company embraced this concept of a guild。 We have this nice bureaucrat definition here。

 But essentially a guild is a group of volunteers who are focusing on technology considered。

 important to Bloomberg and working to promote the best use of that technology。

 The Python guild is just one of several guilds。 There's also a database guild， a JavaScript guild。

 a C++ guild， a testing guild。 But our guild is focused on helping to communicate with the internal Python community at Bloomberg。

 They organize meetings。 They send out newsletters。 They contribute to internal projects。

 There's also some open source contributions that go on。

 And early on they're also doing some work to maintain this extended interpreter that I。

 mentioned earlier。 Technical support。 The Python infrastructure team was started in early 2016。

 There was one person in San Francisco。 A few months after it started， I was hired as number two。

 And this is our office。 It's a really nice office。 It's one of the nicer offices I've worked in。

 A lot of the Bloomberg offices are pretty spiff。 I'm fond of this one。 My team。

 we set out some objectives about what we need to do on the technical side。

 We needed to stabilize what was in front of us。 The C++ extensions in this interpreter had some issues。

 We needed to figure out how we were going to move forward with how these libraries have。

 been wrapped。 We needed to figure out how to manage the interpreters and upgrades to them。

 And packaging and deployment was also a bit of a challenge。 And this was just two people。

 And we were also setting out standards about how to use the interpreter in these libraries， as well。

 Trying to document a bunch of stuff。 So when I arrived。

 the community was running ahead of what the guild and infrastructure team， were doing。

 They were downloading just random versions of Python and building up applications out of， it。

 We were creating one-offs of the extended interpreter that the intern had created。

 There was a lot of dependency management issues that were creeping up。

 This was especially problematic in this environment。

 Our infrastructure had been built around this concept of C++ applications which are distributed。

 as big， standalone， binary blobs。 And the notion of shared applications on a server running out of one interpreter set。

 up didn't really play well with that。 The C++ wrapper， it had a lot of drawbacks as well。

 It was popular and a lot of people were using it。 It was a thin wrapper over the C++ code。

 And because of that， a lot of C++ idioms percolated to the top。

 So there was a bunch of classes there that you wouldn't need in a normal Python environment。

 and the naming conventions look like C++ code。 So it looked like you're writing C++ without the curly braces。

 It wasn't that great to look at。 There were also a lot of issues around memory management because writing this kind of code。

 and a normal path for developing a binary extension is you write it with that purpose， in mind。

 Wrapping code that was never intended for that is quite a bit more complicated。

 So the Python community， while it was largely volunteer driven early on， now they're relying。

 more on the guild or they're participating in the guild to help facilitate what's going。

 on in Python。 They're also contributing to what the infrastructure team is doing in various ways。

 And they're standing up their own inner source projects。

 So that's also really pretty interesting to see。 The guild early on had a lot of challenges。

 It was facing。 It was all a volunteer effort。 It was a very small team。

 There was an awful lot to do。 Burnout was an easy thing to face。

 So we changed the guidelines for participation。 It used to be very experienced in the details of Python to join。

 We said， well， all you have to have is an interest in participating。

 It doesn't matter what your skill level is。 There's lots of work here to do。

 We'll find something that's appropriate for you。 Or you can come with your own idea and work on that。

 We adopted this structure of working groups which are kind of like little committees。

 So my team's challenges， we had to get reigned in all these different versions of Python。

 that were cropping up and get proper management of these things for rolling out。

 We also had to smooth out the whole deployment process。

 And to do this all for multiple architectures where compilers and backends and various things。

 didn't really want to play nice with each other or with what we were trying to do。

 So one of the first things we did was just draw some clear lines about what our goals， were。

 We also adopted an intersource model。 So all of our projects are open。

 Communication about what we're doing is very open。

 We are very welcoming of issues and pull requests to the projects that we have。

 And we're constantly engaging conversations with community often in online forums like。

 chat rooms where people come to us with various questions。 In fact。

 I was just dealing with one or like three hours ago。

 So it's a big part of our job is this communication aspect。

 My team focused on using the guild as a representative of the community。

 So we went to them to get feedback on the plans that we had for going forward and the。

 designs that were coming up for the APIs that were going to replace the C++ wrappers that。

 have been created before。 They helped us send out newsletters to update the Bloomberg engineers about what was going。

 on。 They also helped organize internal meetups where we could present the technology we'd。

 been developing。 And we also got their involvement in the online conversation so that we weren't always asking。

 the questions。 Often the guild would step in and help us with those issues。 We also put together。

 got the documentation organized。 Before it was all kind of scattered and hard to find and we kind of centralized it a bit。

 It began with a very simple mission statement and an FAQ and kind of grew from there as。

 we produced packages。 We built a lot of documentation for those as well。

 So there's now like reference manuals for each of the packages we support。

 There's a bunch of how-to guides。 I have no idea what the total size would be if you printed out but I think it would be。

 pretty substantial。 So the other thing is that we needed a plan for how to replace that extended interpreter。

 It was just too ugly to support。 So we decided to provide packages for each of the important C++ libraries that were involved。

 And just dramatically changed the architecture of these things and really reduced the source。

 of maintenance on these。 To start off， we picked one of the most popular packages that was being used in the company。

 or popular C++ libraries really。 And said， "Well， what is the minimum amount of functionality we can ship for this library。

 that will gain attention and be useful for delivering value？"。

 We ran through our designs with the guild and worked hard to come up with something that。

 supported Python idioms， had really good performance because that was important to people and wanted。

 something that was really reliable。 But we rolled out our first release and crickets。

 Nobody wanted to pick it up first。 So that took a bit of effort。 Again。

 we got help from the guild on this。 We had a lot more conversations with the community about what we had when they came to us with。

 problems about the old API。 We would suggest they tried the new one as an alternative。

 Over time we showed that we had a really good track record both on the performance aspects。

 and on reliability which also helped pick up adoption of these things。 So today。

 Python is the first class language within the company。 If you go to our careers site。

 just we have a career site and do a search for Python， it's。

 the most popular entry and job description for languages there。

 It easily numbers what's there for C++ and JavaScript。 When we bring on new hires。

 we start them off with Python as the language for introducing。

 the concepts for how applications are built in the company。

 And there's really no reason to not use it when you're developing a new application except。

 for very special use cases。 We've seen the internal community grow to several thousand。

 We've seen a lot of growth in community driven projects as well。

 And what I like is they've kind of modeled how they do their projects after the way my。

 team does them。 So these projects also come out with good documentation， high reliability。

 good design。 What I thought was really interesting is the authors of the C++ library have also embraced。

 what we've done。 So they're taking our libraries and using them in their own testing。

 They're also using Python to write their own utilities。

 The way the guild is currently structured is we have two co-leads。

 I like to call them the -- or two co-chairs。 I like to call them the Python Devon。

 That sounds kind of open and welcoming。 We have about a dozen working groups staffed by about 20 members。

 Each working group focuses on a specific issue。 There's one working group that was responsible for helping organize all of Bloomberg's participation。

 at this conference。 And I don't know how we could have done all this without them。

 We hold twice monthly meetings and we have a publicly shared agenda。

 So anyone is welcome to attend our meetings。 And often they just learn。

 but sometimes they jump in with good questions or comments。 My team has grown。

 I'm happy to say it's no longer just two people。 Four of my team members are attending Python。

 Mario and Pablo are also presenting。 They're pretty famous within the Python community。

 Putting in and out。 Matt Wozniski is also attending。

 He is one of the contributors to the memory project that we just open sourced。

 Which has gotten a lot of buzz。 We continue to work with the guild。

 In fact two of my team members are guild working group leads。

 And we talk to each other constantly on a kind of a cross consulting basis。

 It's a relationship that works really well for us。 Most of the C++ wrappers。

 the packages that we developed are now in maintenance boat。

 And I'm really happy to say that they very rarely cause any problems within Bloomberg。

 in terms of production problems or just issues in general。 So we achieved quite a bit there。

 A lot of our work now focuses on improving the upgrade cycle for interpreters。

 And getting better development experience in our programmers' hands。

 So it's just hard to cut over to new versions of interpreters especially like bridging from。

 two to three。 So we've done a lot of work to try to help people with that。 And with package changes。

 Our packages are very stable but outside through party packages， not so much。

 And the developer experience side， deployment is still a big issue。

 We've done a lot of work to improve that。 We've got a bunch of best of class diagnostic tools。

 Memory is one of them。 I'm hoping we can open source some of the others。

 We're constantly working on our documentation and also communication。 It's all about communication。

 So for cultivating guilds， like if this is something you want to do in your own company。

 your own enterprise， there are a few things that work。

 You can't expect this to happen strictly in a volunteer after hours basis。

 It has to be something that is considered part of the developer's responsibilities。

 It doesn't necessarily have to be a high priority。 Other things always take priority。

 But time has to be accounted for it on the management side。 My team is globally distributed。

 I'm in San Francisco。 Some of our team is in New York。 The rest is in London， in Europe。

 This is unusual。 This predates the pandemic， in fact。 And it's something that we propose to them。

 a company has a history of co-locating teams。 But they made an exception for us and we've been able to make it work really well。

 Other teams are now picking up our model as well。 So the other thing we're working with on management is improving how the whole upgrade。

 lifecycle works。 A lot of people tend to look at it as a secondary activity。

 It doesn't really improve revenue。 If it isn't broken， why do I have to pay attention to it？

 And so we have to inform people of the risks here and how to plan it out as future activities。

 for projects。 So one of the other challenges that we found is because we are globally distributed。

 not， just as a team but as a company， a lot of our communication takes place in these online。

 forums。 And it's very easy for us to get tired or make a comment that comes off as too crude。

 or sarcastic or rude and cause a bad reaction。 Now Bloomberg prides itself on being a very civil and inclusive community。

 And so occasionally this does happen though where something inappropriate or rude was said。

 and we have to have a conversation。 Often that goes through the guilt and that's all that has to happen。

 But I'm happy to say that's become much less a problem over time。

 I think that people participating in these conversations are sending a good example and。

 keeping the water level up on the conversation。 I mentioned memory earlier。

 This is a fantastic tool for diagnosing what's going on in the memory of your application。

 Not only looks at what's going on in your Python code but it will dig into the binary。

 extensions that you're using regardless of what language it was written and give you。

 a good view of what's happening there。 And I should mention that some other Bloomberg colleagues are talking at PyCon as well。

 There's a great talk later today by Nandita and Sagar about the transition to open source。

 Fred Phillips is talking tomorrow about the import system and Bernat。 He's a great guy。

 He's going to be talking about editable installs。 He's also a big contributor to the talks project。



![](img/23eb9d7f75d0dc78c85dba406cbb22b0_4.png)

 So when I wrote this I thought I would have time for questions。

 As I understand right now questions will be taking place in the open area。

 But thank you all for attending and for waiting a little extra for lunch。 So thank you。 [APPLAUSE]。

