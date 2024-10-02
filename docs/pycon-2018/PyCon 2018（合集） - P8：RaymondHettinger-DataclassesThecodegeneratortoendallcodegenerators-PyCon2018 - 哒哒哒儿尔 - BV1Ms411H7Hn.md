# PyCon 2018（合集） - P8：RaymondHettinger-DataclassesThecodegeneratortoendallcodegenerators-PyCon2018 - 哒哒哒儿尔 - BV1Ms411H7Hn

 Alright everyone， let's try and pack in， defrag yourselves if you can。

 save some room on the end if possible。 It's going to be standing room only。 Alright。

 let's get started with the talk。 I'd like to introduce to you， Raven Headinger。

 Let's give him a round of applause。

![](img/f533ae2bed1736499c0d9d25630db316_1.png)

![](img/f533ae2bed1736499c0d9d25630db316_2.png)

![](img/f533ae2bed1736499c0d9d25630db316_3.png)

 It's Finks for Slides。 It's awesome。 It's the fastest， easiest way to make slides。 So hello。

 my name is Eric Smith， it's nice to meet you。 Usually when I stand up and talk。

 I talk about something I wrote。 But now I'm talking about something that other people wrote and did a fantastic job at。

 And I get to be the vehicle who hopefully does justice to their creation。

 I prepared the talk as a "What is this all about？" Something for us to think about together。

 What does this mean for us？ What does it do？ So I'd like this to be a thinking exercise for us as we look at it。

 I'd like it to be a tutorial and I'd like my slides for you to be a recipe book of how to use it to a great effect。

 So without further ado， let's see anything on this page。 Interesting。 No， no， no， no， no， no， no。

 Okay， on to interesting things。

![](img/f533ae2bed1736499c0d9d25630db316_5.png)

 I'll start with the make file。

![](img/f533ae2bed1736499c0d9d25630db316_7.png)

![](img/f533ae2bed1736499c0d9d25630db316_8.png)

 [laughter]， Introduction。 There we go。

![](img/f533ae2bed1736499c0d9d25630db316_10.png)

 Rebuilding slides on stage。 All right。 So the title of the talk is the Code Generator to Endol- Code Generators。

 What does the Code Generator do for you？ You give it a list of specifications and it writes code for you。

 Is that good？ If your specifications are good and the code it generates is what you want。

 then it's good。 If your specifications are bad， probably not going to work out so well。

 If the Code Generator does something different than what you expect it's going to do， not so well。

 So when you work with Code Generators， your goal is to be in harmony with the Code Generator and think about its worldview。

 because it's going to write code on your behalf。 In theory， you're unit testing everything you do。

 but people tend to miss a fair number of cases that are covered well by this tool。

 So it helps to really become one with this tool。 One of the things I'd like to name tuples is they have a near zero learning curve。

 On the other hand， they had very limited capabilities。 So this is exactly the opposite situation。

 The data classes module is the amount of code is several times larger than that for named tuples。

 But it provides a much richer set of functionality。

 which means it's kind of an entire ecosystem of tools that are worked together in tools and possibilities。

 One of the things that I really like were some excellent design choices were made so that right out of the box。

 if you use it in the simplest possible way， it mostly does for you what you want it to do most of the time。

 But then it grows with you as you start to have more exotic needs。

 So why do we want a Code Generator？ Because it can save you time。

 the time you would have spent writing that code is paid back right away。 Also。

 Code Generators reduce wordiness。 A lot of Python classes， when people first learn Python。

 they feel like there's a lot of boilerplate in there。 Writing the repper， writing the dunder in it。

 writing a dunder EQ， writing a dunder hash， etc。 And that feeling of boilerplate never really goes away。

 You get used to knocking out that code quickly， but you feel like that code isn't contributing the expression of your business logic。

 The idea of this Code Generator is to get you past that step。

 Next question is what are data classes for？ It depends on who you ask。 There are two world views。

 and of those two world views， the people who see it one way don't tend to see it another。

 Those people who think data classes are primarily about data， which means I need a holder for data。

 I need an aggregate data structure， something like a struct in another language。

 If that is your world view， data classes support that world view。 They are data holders。

 But there's another world view that says I spend a lot of time writing classes putting interesting functionality in it。

 and I'd like the boilerplate to go away so I could focus on the business logic。

 If you think of it as a tool to help you write classes and you're going to write lots of methods anyway。

 it just takes out the boilerplate for you， data classes are for you as well。

 The word data in data classes has to do with it being a data holder。

 and the classes part has to do with it as a class generator。 It is both things。

 and it means different things to different people at different times。

 I'd like this to be a joint thinking exercise together for us to explore this。

 and instead of me just putting in one way give you information。

 I would like you to assess each piece as it comes to you。 Here's what to think about。

 One of the things you want to learn is how to use the code generator。 Also。

 when you use a code generator， the most critical thing to know is what code it writes for you。

 One of the things that I really liked about name tuples， one of what I thought was a best feature。

 was it had a verbose option， and when you used a name tuple it showed you the code that it wrote for you。

 A benefit for that is no one ever had a question what code was written for me， ever。

 On the other hand， no one has ever said anything nice to me about that feature ever。

 Eric didn't copy that into data classes， and I took it out in Python 3。7。

 so you no longer get to see the code that's generated unless you come to this talk。

 If you don't say that you like something it might disappear someday。 Alright。

 so what code does it write for you？ And the next question， even more important。

 once you know what code it writes to you， is that what you really wanted？ Sometimes yes。

 sometimes no。 This is a somewhat opinionated data structure。

 and it has some opinions that I really like， but they may not reflect your worldview at all。

 And then any tool you essentially have to ask the question。

 is the time spent invested learning the tool， teaching other people to use it and to read it。

 is that worth the time that it saved you？ Some tools have an increase in complexity。

 some have a net decrease in complexity， and like a rainbow， each person sees their own rainbow。

 And whether this tool is beneficial for you or not， very much depends on your point of view。

 One challenge for me is I train lots of people， enormous numbers of people to use Python。

 And these folks benefit a great deal by having the classes completely spelled out so that they can learn how Python operates。

 the language teaches you what it does。 On the other hand， when they need to do it on their own。

 they often don't do it well and leave parts out。 So if I introduce data classes to them。

 on the one hand， the likelihood of them doing it correctly the first time is very high。

 On the other hand， the likelihood of them understanding what happens behind the scene goes down。

 Imagine a world where you have used Python classes and have never written a dunder in it yourself。

 never written a dunder hash， never written a dunder repper。

 How would you even begin to comprehend this tool？ It is possible in an organization that if you adopt this tool wholesale。

 all of the examples in the code won't have dunder in it's dunder reppers， dunder hash。

 and so there's nothing to teach the users about what it does。 On the other hand。

 education problems are problems that are easy to solve。 Have a tech talk for lunch。

 take a look at this tutorial and it will show you the code。

 So if the engineering problem is we don't know something， the solution is just teaching it。

 But I can't see in advance how it's going to work out in your world。 And also。

 we have this realization that while it's nice to have a code generator saving its time writing code。

 we spend more time reading and debugging code than we spend writing code。

 So a question you want to ask yourself is this improve your debugging experience or does it make it worse？

 On the one hand， any tool that introduces an abstraction makes debugging worse because it hides something behind the abstraction。

 On the other hand， if it automatically does a lot of things correctly for you and it's a well tested tool。

 as this one is， usually your bug is somewhere else。

 so it precludes those bugs being there to begin with。 But again。

 whether this is a net win for you or not really depends on your world view and what you're doing with it at the time。

 So these are the questions you should ask yourself as it's coming up。

 So I'm going to do something terrible here which is to quickly recount the history beginning up。

 going up to the creation of data classes。 I've got Eric。

 the creator of the tool here to correct me on any of this。

 but this is a dangerous undertaking because the last time I did it。

 I left someone out and they were quite angry about it。 So they are now included in this one。

 That said， history is quite difficult because there are very few new ideas under the sun。

 Every time we think we've done something original。

 you don't have to look too far back in time to where someone else had the same problem and came up with the solution。

 This might be the first time all the pieces were put together。 So in a very brief history of time。

 probably omitting some people， in the beginning， there were dicks， tuples and handwritten classes。

 We used Dickshires for lookups， tuples for structures and handwritten classes when we wanted more functionality and the world was just fine。

 Later， we added a code generator for you， named tuples。

 It wrote some code for you that essentially its goal was to take a tuple and add names to the fields。

 To this end， it succeeded。 However， if you wanted something else。

 like not having a tuple underneath the hood， of course it didn't work too well for you because it's a named tuple。

 If you hate tuples， then you're not going to like named tuple as well。 Tuples being un-mutable。

 being un-packable， being hashable might be things that you want or might not。 Later。

 we got something called type simple namespace， which is just a very quick way to make a data holder。

 As far as being a code generator， it generates very， very little for you。

 but a lot of people really like it because it's so lightweight。

 On the opposite end of the lightweight scale， we had ORMs， things like Django， SQL， Alchemy， PWE。

 As far as I know， these were the tools that pioneered using field specifications to generate more complex data structures behind the scene。

 I know ORM being something， you make a class with field specifications and it generates the database behind the scenes for you。

 That's fantastic。 Another tool that is very old and famous in the Python world is the Traitlets module。

 Traitlets was all about validating data as it came in。 Traitlets was well-loved。

 not as popular as some of these new tools， but it was very good at its job。

 It pioneered the idea of using class variables for specifications of what should go in data or whatnot。

 In the past few versions of Python， we've gone through this evolution of adding type annotations to the language。

 First， we just had a notation for attaching it directly to a function or a class。 In it was inert。

 It didn't do anything。 Later， we got the typing module and third-party tools like MyPy that could scan all of the types and perform static typing checking for you。

 Then Quito and Lisa Roach worked together on a PEP to create a new and clean syntax for it。



![](img/f533ae2bed1736499c0d9d25630db316_12.png)

 As of Python 3。6， you can just write visitor count in to essentially declare without creating this variable just declaring its data type。



![](img/f533ae2bed1736499c0d9d25630db316_14.png)

 That was a beautiful notation。 There was also a third-party module library called Adters。

 I am not a user of the Adters module， but I know it's very popular and it was an inspiration for data classes。

 I put all of these things together。 It was time to put in some standard library support and Eric foolishly volunteered。

 He thought， "How hard could it be？ Did you end up putting a little effort into a Derek？"。

 Over 100 hours？ Over 200 hours？ Over 200 hours？ And he's not done yet。

 I've developed a number of collections classes before and I know they live with you forever。

 You will have a lifetime of feature requests for this for everything you decided not to do。

 Someone will request it。 For every design decision where you went left。

 someone will argue that you should have gone right。

 There will be bugs in there that you cannot have imagined at this phase。 And five years from now。

 somebody will go， "It's too slow。 We need to write the whole thing in C。

 Someone will protest that it uses the exact inside。"， And in fact， it does。

 And you will never get any love for it except for right now。 We love you for what you did。

 It was really a magnificent thing。 [applause]， I do have some good news for you based on my history in Python。

 Every year that I contributed something useful to the language， Quito came to talk to me and said。

 "I really liked it。"， He said， "I'll double your salary。" And for 18 years。

 every year he's doubled my salary。 [laughter]， Being an open source contributor pays really。

 really well not。

![](img/f533ae2bed1736499c0d9d25630db316_16.png)

 Okay。 The goals of this， according to the PEP， data classes are roughly， very roughly。



![](img/f533ae2bed1736499c0d9d25630db316_18.png)

 a mutable name tuple with defaults。 There's more to it than that。

 but if you need to file it away in your law of mind with one little phrase， "What is it？

" that's an answer。 It built on the success of the adders package。

 It writes boilerplate code for you， and it provides an elegant syntax for data holders。 Okay。

 I'm done。 Any questions？ Oh， would you like to see it？ All right。



![](img/f533ae2bed1736499c0d9d25630db316_20.png)

 I'm kind of section comparing it with named tuples。



![](img/f533ae2bed1736499c0d9d25630db316_22.png)

 I would like this to have been a talk that was only about data classes and not named tuples。

 but named tuples is what people know。 And the first question that comes to mind when you use data classes is。

 "What's different？" And， "What's the same？" So the goal of this is to answer that。

 and the goal is to hit， "What is the common case？"， The simplest case of using the data classes。

 So without further ado， here is a data class。

![](img/f533ae2bed1736499c0d9d25630db316_24.png)

![](img/f533ae2bed1736499c0d9d25630db316_25.png)

 and the goal is to store a color in the HSL system， storing the hue， the saturation， and lightness。



![](img/f533ae2bed1736499c0d9d25630db316_27.png)

 He used the number of degrees on the color wheel。 Saturation and lightness are typically expressed as a floating point number percentage of a saturation。

 So the way you make that using a data class is you use a class decorator at data class。

 You write a normal class。 You use that notation created by Lisa and our quido to indicate that。

 hue is an integer。 And saturation is a float。 One really nice thing is you can specify default value for the float。

 Let's compare that to the name tuple version。 You also have to do an import。

 And these three lines are exactly the same as these three lines。

 So the only part that is different is instead of at data class。

 you have an inherit from named tuple。 So do they look very structurally very similar to each other？

 I agree。 One of the questions we are going to ask ourselves is， "Is it easy to use？"。

 This is somewhat beautiful and elegant。 It's a nice way to specify a data holder。



![](img/f533ae2bed1736499c0d9d25630db316_29.png)

 So so far， the only evident distinction is that there's a complete difference in approach。



![](img/f533ae2bed1736499c0d9d25630db316_31.png)

 Data classes use a class decorator and named tuples inherited class that has a different meta class。

 that goes in and fills in all of the values for you。



![](img/f533ae2bed1736499c0d9d25630db316_33.png)

 So how do you work with this data class？

![](img/f533ae2bed1736499c0d9d25630db316_35.png)

 Well， you create an instance of it， much as you would as a name tuple。 Which raised the question？

 I passed in two arguments。 How many arguments are there in the HSL system？ H， SNL？

 It's not intended to be a hard question。 Three。 Okay。 Where did the third one come from？



![](img/f533ae2bed1736499c0d9d25630db316_37.png)

 Not intended to be a hard question。 I'm pointing at the answer。 Okay。 So there was a default value。



![](img/f533ae2bed1736499c0d9d25630db316_39.png)

 So one of the nice features of it is the wrapper came for free。

 And it shows you how it was made and it gives you a self-explanatory look。

 This greatly helps during debugging。 Also， it lets you access the fields by name。

 This is really nice。 If you've ever worked with straight tuples and have to refer to。

 square brackets three or square brackets zero， you'll find that this is a great improvement。

 Can you build one of them off of another？ There's a function called replace that takes in a instance of a data class。

 and lets you replace one or more fields。 This says model off of one color and make another color。

 This color has the same saturation and lightness but a different hue。

 So we're just spinning it around the color wheel。 So we also have the ability to turn it into a regular dictionary。

 This is important。 Conceptually， anything that's a key value store should be easy to turn into a dictionary。

 However， this thing is ordered like a tuple and once in a while you're going to want to unpack it。

 or use it in a tuple circumstance。 This gives you the ability to convert it back into a tuple if you need。

 One of the nice things about data classes is it builds type annotations for you so we can record some information。

 some metadata about the class。 Here's something new。 You can assign a value to it。

 Our data classes by default， mutable or immutable。 Mutable。 Hey。

 who remembers the name of my company？

![](img/f533ae2bed1736499c0d9d25630db316_41.png)

 Mutable minds。 All I need to teach people is to have people in the audience with mutable minds。

 If you have an immutable mind， get out。

![](img/f533ae2bed1736499c0d9d25630db316_43.png)

 And so if I'm going to build a student， I want to use data classes for it because I need them to be mutable。



![](img/f533ae2bed1736499c0d9d25630db316_45.png)

 so I can put in new information。

![](img/f533ae2bed1736499c0d9d25630db316_47.png)

 Now let's see how big it is。 Import sys and we'll see that this instance is 168 bytes。

 I'd also like to use time it to see how fast does it take to access the hue。 By default。

 this does a million iterations so we're looking at 33 nanoseconds here on my 5-year-old machine。



![](img/f533ae2bed1736499c0d9d25630db316_49.png)

![](img/f533ae2bed1736499c0d9d25630db316_50.png)

 Now let's compare that with working with a name tuple。 Notice we make the color the same way。

 Notice it has the same appearance。 Notice that it has the same field name access。

 But now we get to a difference。 One difference is that in name tuples。

 the methods start with an underscore。 I regret this choice。

 I wish I had still used an underscore but a trailing underscore。 But because this has a stretch。

 a strong hint that it's a private method。 It's not private at all。

 It was just put in there to prevent a namespace conflicts with actual fields named a replace。

 So this is not pretty but it does have an advantage。 It's discoverable。

 If you do a dir of the object， you'll see this method。 But if you do a dir of the data class。

 you won't see the functions that operate on it at all。

 This is a normal relationship between objects and our methods and functions。

 So as dict is also a method and here's a difference， it returns an ordered dict。

 We would actually like to switch that to return a regular dict but there's a chance that people rely on it being an ordered dict。

 So I'm going to have to go through a deprecation cycle before I can actually switch this to a regular dict。

 So in this case， data classes will lead us because they don't have any legacy code to model after。

 And if you want to turn something into a tuple， it's the same way as you turn it into a list。

 Its annotations are exactly the same。 Here's a superpower of named tuples that data classes don't have。

 They unpack。 Can you do that with data classes？ The answer is yes but you have to first wrap this in a call to as tuple。

 Which is a quite reasonable thing unless you're going to be doing this a lot。

 If we check the size of this instance， it's significantly smaller， only 72 bytes versus the 168。

 Unfortunately， if we run a timing on it， the name tuple field access is slower by quite a bit。

 61 nanoseconds。 I do have a fix for this but I wasn't able to get it into 3/7。

 Really only 15 minutes？ Oh my goodness， we will speed up。 So I have a fix for this so as of 3/8。

 I can make this access faster than data classes but it's going to take some C code to do it。

 Running through property and item getter is pretty fast but not as fast as this one up here。



![](img/f533ae2bed1736499c0d9d25630db316_52.png)

 So here's the summary of all of the differences。 We have a replace function in a data class。

 It's a replace method。 As dict is a function instead of a method。

 We get a regular dict instead of an ordered dict。 The method versus function again。

 Data classes are immutable。 Name tuples are immutable or frozen。

 Data classes can't be hashed by default。 In general。

 that's a good thing because we don't know in advance whether you're going to put hashable fields inside。

 Whereas name tuples presume that you're putting something hashable inside。 By default。

 they don't iterate which means that you can't unpack them。 By default。

 they have no comparison methods whereas name tuples sort just like tuples。

 They have a different underlying data store。 The data is stored in an instance dict for data classes and it's stored in a tuple for name tuple。

 The size of the data class is more than twice as large。 On the other hand。

 it's more than twice as fast。

![](img/f533ae2bed1736499c0d9d25630db316_54.png)

 If you'd like a quick rundown of all differences， it is on this table。 Alright。

 so let's go look at the code that gets generated。 So here was a common case that we just looked at。



![](img/f533ae2bed1736499c0d9d25630db316_56.png)

![](img/f533ae2bed1736499c0d9d25630db316_57.png)

 This is the code you write。 What do you get for that？ What does it do for you？



![](img/f533ae2bed1736499c0d9d25630db316_59.png)

 Well， as we look below， we'll find the code that it generated。

 And the most important part is these first of this top section up here。



![](img/f533ae2bed1736499c0d9d25630db316_61.png)

 So let's see what we get。 Like name tuples， it gives you a nice doc string。 In fact。

 the same doc string。 I really like that it's under init。

 It has a type annotations built directly inside so it's introspectable。

 It has a nice wrapper and the wrapper is exactly what you would write by a hand except that a lot of people would leave off a dull name。

 This is a case of the generated code being nicer than what most people would write by hand。

 It's incorporating a best practice for you。 The equal method is really interesting。

 What name tuples would do is this comparison here on the second line。

 It's what most classes would do。 This one is somewhat interesting though。

 It checks to see if the other method is of the other object is of the same class。

 So it refuses to compare apples to oranges。 This gives us a much stronger relationship between types。

 It's something that I wish had been done for enums because it is possible now for us to have the enum color red equal one be equal to an enum size small。

 medium and large where small is one。 And of course those things should be non-comparable and so this best practice is built into data classes by default。

 And I think this is a really nice win。 It's something most people wouldn't think to do but it will improve code quality and help you avoid some really hard to find bugs。

 Another thing that people commonly forget is whenever you write an equality method you also need to say something about hashing。

 And so this makes a fairly bold statement。 I'm not hashable at all。

 What would happen if this were left out？ It would hash but it would compare on identity。

 And for a lot of people that is a surprise and it's rarely what you want。

 So once again data classes are making a choice for you that is mostly what you want most of the time whether you knew it or not。

 So I think we can conclude at this point how much work did it save you？

 You wrote four lines of code。 What did it write for you？

 It wrote about nine lines of code or ten lines of code。

 So there's about a six line of code savings but along the way it improved quality by doing things that other people forget。

 By making a tighter checks， making a higher quality repper and adding type annotations which are things that will forget。

 So I think of the win in terms of code saved being very minor here but the win in terms of quality by default is a nice win。



![](img/f533ae2bed1736499c0d9d25630db316_63.png)

 There's also all of this other stuff down here and I'm trying to avoid a pejorative term junk。



![](img/f533ae2bed1736499c0d9d25630db316_65.png)

 It is not junk。 It supports introspection of the class。 But it looks junky in the help。

 So one of the nice things is it records the parameters for you。

 It tells you how the data class was built。 This is really nice。

 Lots of other code generators leave this information out and you have to guess what parameters we used to make them。

 In addition， it has detailed information about each field and including the metadata。

 So if you were writing the code by hand， you would probably have written only about our seven lines of this。

 If you were smart in doing it， you would have written all of these lines。 Almost certainly though。

 you wouldn't have written any of this and this stuff is actually left in your class and when you're handwriting code。

 you would probably have never put these attributes in there。 Interestingly。

 this one actually creates a class variable for you。

 So you now have a class variable and an instance variable that might disagree with each other。

 Not really problematic but different from what we would write by hand。



![](img/f533ae2bed1736499c0d9d25630db316_67.png)

 So the interesting points are it matches our handwritten code for three of the methods。

 The doc string matches what you would write by hand。 Equality comparison does an exact match。

 Head is set to none so you don't get accidental hash ability。

 Generated code includes everything from the original plus the default as a class variable。

 Probably not what you want and some nice introspection whose only downside is it looks a little bit junky in the help。

 And that is the common case。 Who wants uncommon cases？ That's good。

 How many minutes do I have left for uncommon cases？

 So one of the really nice things about data classes that makes them a little bit more complex or more complex than all the other code generators in Python。

 is it allows extensive customization。 So all the things I said it didn't do by default。

 some of them it does but you have to say what you want。



![](img/f533ae2bed1736499c0d9d25630db316_69.png)

 So to extend this example we are going to cover freezing and order。



![](img/f533ae2bed1736499c0d9d25630db316_71.png)

 So data classes are by default mutable。 This is a nice benefit。

 On the other hand you can't use it as a set element or as a dictionary key which often is what we want。

 Also they are not orderable which means that you can't sort this in some nice consistent print order。

 But if you need these all you have to do is say what you want。



![](img/f533ae2bed1736499c0d9d25630db316_73.png)

 So the only change to our previous recipe is saying order equal true and frozen equal true。

 Who thinks it's kind of nice？ You can just say what you want and get it。

 And if you don't say what you want you get what you usually need。 This is a really nice benefit。

 The only problem from this is if you think that the default is something that what it actually is。

 if you're testing you'll learn it the hard way。 On the other hand there's some really nice tool tips that go along with this。



![](img/f533ae2bed1736499c0d9d25630db316_75.png)

 So now let's use that new data class。 It's pretty easy。

 This time I make four colors and we can now do something new with them。



![](img/f533ae2bed1736499c0d9d25630db316_77.png)

 We can sort them。 Previously that wasn't possible。

 Also we can make a set of the colors which eliminates the two duplicate colors the one at 66 degrees and 75% saturation。



![](img/f533ae2bed1736499c0d9d25630db316_79.png)

 These two capabilities were previously not there。 If you need those capabilities are they easy to turn on。



![](img/f533ae2bed1736499c0d9d25630db316_81.png)

![](img/f533ae2bed1736499c0d9d25630db316_82.png)

 Yeah， which is a fairly nice win。 Now back to the question how much work does this save you。

 In this case a lot more。

![](img/f533ae2bed1736499c0d9d25630db316_84.png)

 So there are several comparison methods that get written for you。 These are no fun to write by hand。

 We already had a solution for that problem in Func Tools Total Ordering also implemented as a class decorator。

 However it didn't have this by default this stronger check to make sure that the classes are the same to prevent accidental comparison。

 Another interesting thing is when we made it frozen we wanted to be immutable so you can't write to it or delete an attribute。

 So this is fantastic。 There's a number of ways to achieve that。 One is through a meta class。

 Another is quite common in the Python world。 If you look at the fractions module we make the numerator and denominator un-ritable。

 The way we do so is by using a read-only property。 In this case though we extend set adder。

 Something fairly interesting when you compare these。 The ones here at the top don't call super。



![](img/f533ae2bed1736499c0d9d25630db316_86.png)

 We don't really want them to because a parent class object will return， I forget what it returns。

 none or not implemented or something like that。 So we really don't want to call those there。

 But set adder and a day letter we actually really do mostly want what the parents have。



![](img/f533ae2bed1736499c0d9d25630db316_88.png)

 So if you're attempting to modify one of the fields that is blocked it will go ahead and raise an exception。

 Otherwise it will go ahead and call the parent which means that a class can be partially writeable。

 So this is really nice。 This is beyond the code here is beyond the skill level of most everyday Python programmers so they're getting a really nice win for this。



![](img/f533ae2bed1736499c0d9d25630db316_90.png)

![](img/f533ae2bed1736499c0d9d25630db316_91.png)

 Also it's fairly common for people to be challenged in writing and underlying hash function and by default it includes all of the fields。



![](img/f533ae2bed1736499c0d9d25630db316_93.png)

 So what were the interesting points here？ In a way this is better than Funke Tools Total Ordering because it's type specific。

 We can't call super for the comparison methods because object has ones that we don't want。



![](img/f533ae2bed1736499c0d9d25630db316_95.png)

 The frozen behavior is implemented in an interesting way by extending get adder and day letter。

 This is different from what most people would write by hand like what you see in the fractions module。

 So these extended methods do call super so that we don't accidentally turn off an important behavior in the parent class。

 And the hatch method does write what you match what you would write by hand。

 As a quick comparison to name tuples all of these behaviors come for free because they're inherited for tuple。

 And the ones in tuple are written in C so they're quite a bit faster so you get all of these behaviors。

 Which would make you think that for something that's frozen named tuple might be what you wanted to begin with。

 And I believe that that is mostly true。 However the world's more complex than that。

 Do I want to be mutable or immutable or a mix of both？

 I'll have peanut butter and chocolate and half some races。

 And sometimes there's legitimate use cases for a mix of those。

 What about do I want orderable or not？ Orderable is good if my data is orderable。

 orderable is bad if it's not。 What if I have a mix？

 In that more complex world we have something data classes can do for you that name tuples can't。



![](img/f533ae2bed1736499c0d9d25630db316_97.png)

 Name tuples are somewhat limited in their capability。

 Remember their goal is to do whatever tuples do。 And not to extend that notion they just take tuples add types default values and name via lookups。

 You're kidding me where did it go？

![](img/f533ae2bed1736499c0d9d25630db316_99.png)

 Okay we did type make right？

![](img/f533ae2bed1736499c0d9d25630db316_101.png)

 Alright so here's things that you might want that are outside the defaults。

 You might want to customize a field specification。

 And so in my example we have a I'm trying to pick a real world example。

 We have a employee class that stores an employee ID named gender salary and age。

 We also want to keep a list that grows over time recording everyone who has viewed the employee record so we know who's seen it。

 So that creates a fairly rich problem for us that will exercise all of the powerful capabilities of data classes。

 One thing we want is a field factory。 Instead of a default value of 。

5 we'd like to make a new empty list every time。 This is very easy to do。



![](img/f533ae2bed1736499c0d9d25630db316_103.png)

 We'll just say the default factory is a list。 You use this very much like default Dix。



![](img/f533ae2bed1736499c0d9d25630db316_105.png)

 Also what if you want to add custom methods to the class？

 This is a regular class and so just write your method normally。

 In this case we'll write an access method to record who viewed the data。



![](img/f533ae2bed1736499c0d9d25630db316_107.png)

 Now we'd like this to be hashable so we could use employees in a dictionary。

 However only three of the fields are immutable。 A person's age will change from year to year。

 Hopefully their salary will increase as well。 And so we'd like to exclude those。

 In addition we really don't want the list to be included because it is not hashable。

 If we specify a default factory it's automatically excluded from the hash function。



![](img/f533ae2bed1736499c0d9d25630db316_109.png)

 Also you don't necessarily always want to display all the data in the wrapper。

 In particular the salary information we don't want that to show by default because it's sensitive。

 Likewise the field information will be for all of the people who view the record。

 That might be a rather long list and we don't want it to be part of the wrapper that will get in the way of debugability。

 This is easy to achieve， you just say I want this field to have its wrapper turned off。



![](img/f533ae2bed1736499c0d9d25630db316_111.png)

 Also we want to limit the fields used in comparisons。

 There are some things like complex numbers or functions that aren't orderable。

 And they would raise a type error if we include it so we need to exclude them。



![](img/f533ae2bed1736499c0d9d25630db316_113.png)

 Also we want to include the metadata like that list of accessors。 That is no basis for comparison。



![](img/f533ae2bed1736499c0d9d25630db316_115.png)

 This is also easy to do。 If you don't want comparison turn it off。



![](img/f533ae2bed1736499c0d9d25630db316_117.png)

 Also from the database world we know that we sometimes want data dictionaries to describe our records。

 Mechanisms provided called a metadata parameter and there's a viewer function called fields。



![](img/f533ae2bed1736499c0d9d25630db316_119.png)

 So we attach a metadata that lets us specify in this case the spell I pay people in bitcoins。

 They never know how much they're going to make。 I'll say I'll offer you ten bitcoins for years worth of work。

 And then like am I rich or I am poor？

![](img/f533ae2bed1736499c0d9d25630db316_121.png)

 Well it varies from day to day。 So I've given you a fairly rich set of constraints。



![](img/f533ae2bed1736499c0d9d25630db316_123.png)

 And if I ask you to write that by hand you might ask for a couple hours of time and a couple hours of testing time。



![](img/f533ae2bed1736499c0d9d25630db316_125.png)

 The question is does data classes make it easier for you？ Yes。

 all of those specifications are quite readable。

![](img/f533ae2bed1736499c0d9d25630db316_127.png)

 Let's look at how we do it。 I say I want this data class to be ordered。 This is an interesting name。

 Unsaved Hash。 And I'll let Eric talk to that if you have any questions about it。

 It is a thorny subject。 Roughly it is named so that people realize that they're up to something tricky and don't do anything crazy with it。

 And it says of all of the parameters this is one where you're going to look up what it does before you use it。

 Hence the word unsafe。

![](img/f533ae2bed1736499c0d9d25630db316_129.png)

 It doesn't mean actually unsafe。 It just means more complex than you expected for a slug。



![](img/f533ae2bed1736499c0d9d25630db316_131.png)

 So listing out all of the fields was easy。 Notice how to add a method。

 I just write it like a normal method。 So we want to add a viewer ID。 Oh。

 these three field specifications are straightforward。 The salary， I don't want it to display。

 So I turn that off。 I don't want it included as part of the hash。



![](img/f533ae2bed1736499c0d9d25630db316_133.png)

 And I would like to record that I'm paying people in Bitcoin。 Also。

 age is not part of the hash as well， but it is part of the comparison。



![](img/f533ae2bed1736499c0d9d25630db316_135.png)

 The viewed by list will be every time we create a new instance， it creates a new list for us。

 This is the default factory。 We don't want that to be part of the comparison or part of the repper。

 That is pretty darn easy。 And to me， this is the most impressive example of what data classes can do。

 How many of you like it？

![](img/f533ae2bed1736499c0d9d25630db316_137.png)

 [applause]， Very cool。 So let's just work with that example。



![](img/f533ae2bed1736499c0d9d25630db316_139.png)

 I will put my wife in and rather than give away her age in normal units。

 I'll give it in hexadecimal。 There we go。 I'm already in trouble for missing Mother's Day tomorrow。

 And our wedding anniversary last week。

![](img/f533ae2bed1736499c0d9d25630db316_141.png)

 Not cool。 All right。 Notice that I didn't have to specify the viewers that has created a forest by the factory default。



![](img/f533ae2bed1736499c0d9d25630db316_143.png)

 Okay。 So nothing exciting on the instance creation。 I go ahead and call my methods。

 who's accessing this record。

![](img/f533ae2bed1736499c0d9d25630db316_145.png)

 So we record two viewers。 If I pretty print the viewed by。

 you can now see the two people who accessed the record and when they accessed it。

 What did you know and when did you know it？ Okay。 And we can now sort it。



![](img/f533ae2bed1736499c0d9d25630db316_147.png)

 Why？ Because it's orderable。 But when it orders， it's not going to order on all the fields。

 just the fields that we specified， the ones that make sense。

 And when we look at the wrappers of these， the data like who it's viewed by is excluded from the wrapper。

 as is the salary， per our specs。

![](img/f533ae2bed1736499c0d9d25630db316_149.png)

 So in addition， we can use the employees as dictionary keys so I can assign Rachel to gather all of the requirements and Martin to write all of the tests。



![](img/f533ae2bed1736499c0d9d25630db316_151.png)

![](img/f533ae2bed1736499c0d9d25630db316_152.png)

 And that's possible because we've marked certain fields as hashable。

 Keep in mind not all of the fields were hashable。

![](img/f533ae2bed1736499c0d9d25630db316_154.png)

 And so we needed all of this power。 In addition， the fields functions lets us introspect the field specifications and we can see that the salary field。

 the payment is in Bitcoin。

![](img/f533ae2bed1736499c0d9d25630db316_156.png)

 The code that's generated here is this is very close to what you would write by hand for the init to create a new list。

 The wrappers， what you would write by hand。 The equalities we discussed before is type specific。

 As are the other comparison functions。 What's interesting about the comparison functions is not how they're written。

 It's what they exclude。 They don't include all of the fields。 And likewise。

 the hash doesn't include all of the fields。

![](img/f533ae2bed1736499c0d9d25630db316_158.png)

 So some quick closing thoughts。

![](img/f533ae2bed1736499c0d9d25630db316_160.png)

 And then to defend myself during the question and answer period。

 I've called in for backup and reinforcements。 So if I either tell you any lies or don't know the answer。

 the person who knows the truth and may be willing to share it with you is here。



![](img/f533ae2bed1736499c0d9d25630db316_162.png)

 So I'll invite Eric to come on up and we'll give him a round of applause when he gets here。

 So here are the challenges with using it。 One is it's currently tricky to add slots to a data class。

 And slots is a pretty darn important optimization。

 It'll save you a lot of space and improve the speed of access of the fields。

 So slots works just fine until you have default values。 And for deep reasons， they fight each other。

 This is hard to fix but probably will be fixed。 If we fix it though。

 almost certainly we're going to have to have the class decorator not modify the class in place but make a brand new class。

 This will be irritating at so many levels。 The dunder init that's generated doesn't call super because most of the time that's not what we want。

 But if you incur it from a class who's init does to be to call。

 there's an interesting dunder method called post init that calls what you need。

 There was some debate about whether to include a pre init。

 but that probably won't happen unless somebody asks for it with a compelling idea。

 And that's the compelling use case。 Also， data classes were designed to wrap around existing classes and it works really well with mutable classes。

 But if the immutable， there's an immutable parent。

 we don't have an easy way to override or extend the dunder new method。

 It is really all about things that we're putting the data in。

 So I tried to reinvent named tuple using data classes and it essentially did nothing for me other than create the wrapper and not in the way。

 It's clean away as the named tuple did。 So there are some things that it's not good at。

 Mostly it's good for a lot of things。 Future directions。

 I expected over time people will request things that are in the adders package will come to be present in data classes。

 So at some point we'll probably have data valid or daters。

 At some point we'll probably have better support for slots。

 So I hope you enjoyed that tutorial and walk through。 I tried to give you simple cases。

 slightly improved more common cases。 And then the corner case it said we took a challenging problem that had many specifications to it and boiled it down to something simple。



![](img/f533ae2bed1736499c0d9d25630db316_164.png)

![](img/f533ae2bed1736499c0d9d25630db316_165.png)

 And I hope I've presented a compelling set of use cases in our tutorial for you。 How did I do？

 [Applause]， The challenge for you now is that was a nice round of applause。

 I challenge you to do better。 I'd like you to double that for the person who actually did all the work。

 Eric， do you want to come up with me？ [Applause]， You'll need a microphone。 [Applause]， Eric Smith。

 We have time for about one question。 So make it a good one。 [Laughter]， Hi， Roman。

 Thanks for the talk。 There's one thing I can do if I hand write the Dunder in it。

 I can make arguments keyword only by putting in an asterisk in the function signature。

 Is there any support for that in declarative data class？ I'll say no。

 and then we'll get the right answer。 There's not。 Part of the reason is because it has to be arguments for Dunder in it or the order you do clear it on。

 It doesn't necessarily make sense that you somewhere in the list of fields you'd have to say， "Oh。

 and everything after here is keyword only。" And it also doesn't work well with inheritance。

 where if you inherit from something that had only keyword arguments。

 then everything you had would suddenly have to become keyword argument only。

 Because if you have a data class that inherits from another data class。

 it adds the Dunder in it to the child class。 It just depends all of the fields together on the inherited classes。

 Dunder in it。 So it has to everything in it because it doesn't call it the base and last。

 Executive summary， it's harder than it looks。 It is harder。



![](img/f533ae2bed1736499c0d9d25630db316_167.png)

 All right， let's give them one last round of applause。 [APPLAUSE]。



![](img/f533ae2bed1736499c0d9d25630db316_169.png)

 [BLANK_AUDIO]。