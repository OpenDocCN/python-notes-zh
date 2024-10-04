# P22：Talk - Benjamin _Zags_ Zagorsky_ Handling Timezones in Python - VikingDen7 - BV1f8411Y7cP

 And， you know， we do， you know， front end， through back end， databases， integrations， all。
![](img/da32327101e4d6f29aad8f16815471db_1.png)

 that stuff。 The reason I'm giving this talk， well， there's two。 First。 a primary back end language is Python。 We do a lot of work in Python and particular Python Django。

 So I've seen a lot of Python used in different contexts in a lot of different industries。 And the second， one of the joys of doing software consulting is I get to see the same problems。

 over and over again in a number of different contexts and come up with solutions that work。 across all of them。 And I will tell you one of the problems that comes up over and over again is time zones。

 So that's why I'm giving this talk。 Why are you here？ Let's start with a seemingly simple question。 What is today's date？ If you look at your phone， it will tell you that correctly here is today's date of the。

 29th。 But if you ask somebody in Tokyo， right now， they will tell you correctly that today's。 date is the 30th。 So already， with some seemingly simple question。

 we've uncovered something incredibly complicated。 And that complicated thing is that whenever you are working with dates or times。 you are， working with time zones。 Whether you think you are or not， and if you don't think you are。

 oh， you're in trouble。 That's how you get time zone bugs。 So I'm here to tell you how to think about time zones and avoid those nasty time zone， bugs。 Now。

 there might be some of you in the audience thinking， OK， but what if all my users are in。 one time zone？ I don't care。 But you still care because time zones aren't just about users being in multiple different。

 time zones。 It's also about the rules of how time behaves in a time zone。 things like daylight savings， time。 And even if the Sunshine Protection Act currently up for consideration by Congress。

 which will， end daylight savings time by making it permanent， it's a little weird。 Even if that passes， as software engineers， we are cursed with daylight savings time forever。

 because of historic time stamps-- oh， yeah， and Europe still exists。 They still do daylight savings time， even if we stop。

 So having correct time zone support also gives you daylight saving time support， all。 these other weird temporal anomalies。 All of that is bundled into this umbrella of time zones。

 And you want to have that in your code base from the start so that when you do have to。 deal with historic time stamps， or you do start having users in other time zones， or。

 daylight savings time happens， which it does， you don't get burned。 This is also the time for this talk。 It's the time because Python is changing or already has changed。

 Python 3。9， Python finally has standard library support for all time zones。 Before that。 it had support for one UTC。 Now with Python 3。9， the zone info module adds support for all time zones。

 and the community， is shifting。 Django， for example。 in Django 4 is replacing PyTZ with zone info as the default time zone， provider。

 And if you're using PyTZ， you should make that same change。 I'll tell you why in a bit。 So here's what we're going to go through。 First， what not to do。 As I go through that， think。

 do I do this anyway， code base， make notes。 Then， we're going to talk about what to do high level。 how to think about time zones well。 And then we're going to get low level。

 We're going to actually get into code recipes for how to handle the common problems that。 show up when working with time zones， those things that show up again and again in every， industry。

 And then we'll take a few minutes to the end。 Anyone using Django。 Django's got some extra utils for handling time zones that I'll run， you through。 So what not to do？

 The language in this section is going to be intentionally extreme。 The reason I'm going to be extreme is not to say never do these， but because these are。

 all things that don't do what the average developer expects。 And so if you do them。 not only should you know what you're doing， but you should comment。

 your code so that the next person knows what you did and doesn't copy the stuff you did。 in the wrong context and introduce time zone bugs。 So number one， don't use naive date times。 Ever。

 What is a naive day time？ A day time in Python is a seven tuple， your month day， hour， minute。 second， and then it， has an optional TZ info attribute for time zone information。

 Now it's optional because， well， as I just said， Python didn't have full time zone support， until 3。9， so it couldn't exactly require you to pass time zone information in。

 But that leaves us in a really nasty situation where the default way to construct a daytime。 is without time zone information and that's not what you want。 And a where daytime。

 one that has time zone information， represents what you think a daytime， represents。 which is an actual point in time past， present， or future。

 A naive day time on the other hand represents the idea of a time， a template， if you will。 This naive day time represents the idea of noon on New Year's Day 2022。

 And that actually represents dozens of different points in time。 A full 26 hours of ambiguity are represented by that naive day time。

 Time zones range from UTC minus 12 to plus 14。 So if you're constructing a daytime and what you are trying to represent is a point in time。 which is probably what you're doing if you're constructing a daytime， please attach a time。

 zone to it。 You'll regret it if you don't。 Now that's simple， but what about libraries？

 I assume you use them。 There's a whole bunch of libraries。 I'm not going to cover all of them。 but what I am going to cover is the two biggest perils， in the Python standard library。

 Date time now。 If you call date time now in your code with no arguments， you have time zone books。 The reason is two-fold。 Date time now does not attach a time zone。 It gives you a naive day time。

 so that's already bad， as I hopefully just convinced you。 And also。 the return is dependent on system time。 So this means you can run the same code at the same instant。

 Locally and on a server get totally different results。 This is unintentional。 non-deterministic behavior。 That is a very bad feature or a very bad bug to have in your code。

 really an anti-feature。 And it's a particularly pernicious kind of bug because it's one that doesn't show up。 in local testing。 You run your code locally。 It's perfect。 You push it to the server。

 Everything's ruined。 Now you might think， OK， I'll use daytime UTC now， right？

 UTC fixes a lot of problems。 Now at least we've solved the problem of we don't get different results in different。 time zones。 We see now， again， as part of the history。

 that function is old enough that it returns a naive， day time。 So you know it was UTC when you constructed it。 But as soon as you go hand it to somebody else。

 you put it in a database or you convert， it to an ISO string and send it through an API。 you have lost the time zone information， and instead handed them 26 hours of ambiguity。

 So don't use naive day times。 That's the big takeaway here。 Next up。 don't use dates without a time zone。 Now hopefully there is at least someone in the audience thinking。

 but ZAGs， dates don't， have time zones。 There's no TZ info attribute on dates and you would be technically correct but have missed。 the bigger picture。 If what a date represents in your code is an actual 24-hour interval past。

 present， or， future， you're talking about actual points on the timeline and you need to have a time。 zone。 You need to have a time zone in mind that you're constructing that date in or else you're。

 going to be getting the wrong date in some cases。 So for example， if you call date。today。 you have time zone bugs。 The reason that date。today is nasty is what it does。

 It takes the current system time， pulls out the date。 And so you can run the same code at the same instant locally and on a server and get results。

 a day apart。 And this is the worst kind of bug。 Not only does this bug show up。 not show up in local development， only shows up when， you push it to the server。

 but this bug only shows up outside of business hours。 This is a bug that shows up late at night for US developers or early in the morning for。

 European developers。 And so you won't catch it。 You won't catch it in QA。 but your users or your overnight jobs， they absolutely will， catch it。

 If you write a form validation saying， you know， the date must not be in the future and。 you use date。today， that user in Tokyo who's trying to fill out the current date， your。

 website's broken for them。 So the solution to this is kind of in the title。 When you're constructing a date， if what you're talking about is an interval of time。

 that actually has or will happen， have a time zone in mind。 Number three。 don't do duration arithmetic outside of UTC。 So duration arithmetic is if we're taking two date times and we're saying。

 well， what's， the difference in time between the two？

 So if we're taking one date time and saying add some number of hours to it。 Now this is a very specific date time that I have in this example。 130 in the morning， March 13。

 US Eastern， 30 minutes before daylight savings time。 This equals itself converted to UTC。 That is correct。 Converting a time from one time onto another。

 they still fundamentally represent the same， point in time。 If we add two hours to both sides。 they're no longer equal。 So for anyone， Matthew in the audience， what I just said， Python does。

 You can run this。 A equals B。 A plus 2 does not equal B plus 2。 That should terrify you。 That should tell you this code isn't doing what you expect。 And it's not。

 When you add a time delta to a date time in Python， what Python is doing is it's saying。 please take the hour hand on the clock and move it forward that much and then do some。

 date rollover。 And then once we're done with that， we can reexamine time zones。 And so what that means is that we haven't actually added two hours of duration。

 We've just changed the clock by two hours， but the UTC office has changed。 And so adding two hours in Python to 130 in the morning in US Eastern on this date gives。

 us a result only one hour later。 The fix for this， again in the title。 do duration arithmetic in UTC。 Because UTC doesn't have these pesky temporal discontinuities。

 Next up， don't use PyTC。 That's a really flagrant statement right there。 This is not to say the entire PyTC library is bad， but there are aspects of it that are。

 very dangerous。 PyTC is a great library last decade。 PyTC has been around since 2004。 and I really appreciate its contribution to adding date， time support to Python。

 But it's so old that it's not compatible with the directions Python has gone in in terms。 of how time zones should work。 So for example， if we take this actually normal day time。

 there's no magic on this one。 There's no daylight saving time or anything like this。 Totally normal day time in April of this year。 And we attach the PyTC time zone of US Eastern to this at time of construction。

 We get a UTC offset of four hours and 56 minutes。 That is not correct。 The correct number is minus four hours。 And that number has not been correct for America。

 New York since 1883。 That's what I mean by incompatibilities with the Python standard library。 The solution to this， very simple， the Python standard library now has time zone support。

 It's called zone info。 You should use it。 The last one before we get into what to actually do is don't replace the time zone info on。 an aware date time。 Now this is specifically for aware date times。

 An aware date time is one that already has time zone information。 The reason you shouldn't do this is as is broadly the theme here。 It doesn't do what you expect。

 What I would expect replacing the time zone on a aware date time would do would be please。 convert this same point in time but just give it to me in a different time zone。

 That's not what happens。 What happens is you take the date， you take the time。 you pick them up off the page， you， go put them in a different time zone。

 So what that's actually doing， right， the numbers of the daytime aren't changing but。 the time represented by it is changing by many hours。 And so replacing the time zone info。

 what you're saying， this is actually in a different， time zone， whoops。 and this represents in all likelihood a different point in time。

 So probably not what you want to do， almost certainly not。 If what you want to do is convert a daytime to another time zone， use the function has， time zone。

 All right， so let's talk about how to do it right。 I've said UTC a lot already。 What is UTC？ Well。 first and foremost is a bad acronym。 What they did was they took the English， they took the French。

 they had the same letters， in a different order， the compromise was no one wins。 So if you want。 UTC stands for utterly terrible compromise。 But bad acronyms aside， it's an amazing standard。

 First of all， no temporal discontinuities。 Ahem， leap seconds。 Oh， sorry。 All right。 we're ignoring leap seconds because Python does。 Python actually ignores them。

 leaves it to network time protocol to sort them out。 So we're not talking about those。 And if we aren't talking about those， UTC does not have any of those temporal discontinuities。

 Daily saving time， what's that？ Pacific islands， switching sides of the international date line。 don't care。 So this is a great place to work in because we can actually do duration arithmetic safely。

 Next up， all other time zones are defined in relation to UTC。 And this one I don't have an asterisk on。 As far as I can tell， that is true。

 There's some weird offsets out there。 There's some half hour offsets， some 15 minute offsets。 But all of them are still defined in relation to UTC。

 There is still some offset depending on the time of year。 So UTC is a great central place to work when you're trying to get， you know， you can convert。

 to and from other time zones very easily because UTC is the time zone by which all other time。 zones are measured。 And then finally， it's the official time zone of the international space station and。

 space is cool。 This diagram is the most important slide in this talk。 Don't worry about getting it all at once。 Okay， we're going to zoom in on various pieces of it when we go into code recipes。

 But we are also going to take a little bit of time here and explore this。 So right here in the middle， we have this beautiful green bubble。

 This is the idyllic paradise of UTC。 In the time zone safe zone of UTC。 we don't care about temporal discontinuities。 We don't care about there even being more than one time zone。

 If we have time zone aware UTC day times， we can do duration arithmetic on them all day。 pun intended。 We can put them in time zone aware database storage and back out。

 We can send them to other systems as ISO strings。 And if we get the current time in UTC。 we're still in this beautiful paradise and where， time zones don't exist。

 And we could stay here forever if it weren't for one pesky little thing， the user。 Software would be a lot easier without them。 So let's deal with users。 Okay。

 so we've got user input， right？ Users have time zones。 In order to deal with users。 we actually need to know what time zone a user is talking about。

 So we need to either have a time zone attribute on the user or have the user provide us the。 time zone they're talking about when we get whatever they're giving us。

 So if we take the user input of a year， a month a day， a time， and they give us a time zone。 we can attach that time zone to all that other information。

 We can get an aware date time and then as time zone that into UTC。 ISO strings up at the top here。 they're actually easier to deal with because correct ISO strings， have a UTC offset baked in。

 So we actually know how to get that。 We can use stir time or from ISO format。 we can get an aware non- UTC daytime as time's， on that into UTC。 And going back the other way。

 as time zone does double duty。 That can also get us to a user's local time zone， a local daytime。 And from that， we can turn that into a string or we can get those dates that you probably。

 really want to use at some point。 And that would be it except for this one little arrow over here which is calendar arithmetic。 So instead of saying I want to add or subtract durations， saying what I want to do is change。

 the date by sum amount， that you should do on dates。 If you do calendar arithmetic。 you say I want to change my date by some number of days。

 As a duration in UTC and then convert to a local time zone， all of those temporal discontinuities。 now show up as bugs in the math you just did。 So if we're talking about changing time。

 that needs to happen in UTC。 If we're talking about editing the date， do that on a date。 I mentioned already to use zone info， Python standard library， especially instead of PyTC。

 That's not to say this is the only option。 There's other options out there。 DateUtil， for example。 is actually compatible with the Python standard library。 But at least in terms of sensible defaults。

 zone info gets us what we want which is add， ability with the rest of the Python standard library。 We can take that same totally normal， no tricks here， totally normal daytime， attach the US。

 Eastern to it and get the correct offset of four hours， the correct at least for this， century。 not for 1883。 Now zone info isn't added until Python 3。9， but you don't have to wait。

 If you're still on older versions of Python， there's a back port， you can pip install it。 add this to your requirements file。 Nice and easy。 You can even do the pre and post Python 3。

9 compatible import statement if you want to， get really fancy and save yourself hassle when you do upgrade。 The one other recommendation on here for compatibility， add the TZ data library。

 The TZ data library is a compilation of all of the time zone information that Python will。 use as a fallback if it can't find the system time zone information files。

 These are the files that keep track of all of the rules of all of the time zones in the。 world of which there's a lot。 That's really helpful for either minimal distributions of Linux or whatever that might not ship with。

 that or Windows where Python has no idea where anything is。 So now we're going to zoom in on pieces of the diagram and we're actually， I mean， there's。

 been some code so far， but this is the killer code。 This is the code you should use in your code base。 So first， getting the current time。

 How do we do it？ What we want is the current time UTC with the times unattached。 Very simple。 All you have to do is actually pass the UTC time zone as an argument to date time now。

 So date time now turns out it is the right function after all。 You just have to use it right。 Compare that to UTC now。 They will give you the same value。

 but this first one will actually attach that time zone。 information so you're not running around with the extremely dangerous naive date times。

 If we want to get the current date， that only makes sense to do in a time zone。 There is no such thing as the current date。 There's only the current date in a time zone。

 Each time now actually takes some other arguments besides UTC。 You can actually give it the time zone that you want and it will get you the current time。

 in that time zone and then you can pull out a date。 So this will safely get you the date in that time zone。

 Let's say we already have some UTC date times and we want to do output。 So we want to get。 turn that into a date。 So here， I mean， as time zone is going to do most of the work。

 As time zone gets us into the user's time zone， we can pull out the date。 No problem。 Similarly。 if we want to output to a string， as time zone， get into the user's time and。

 we can do stir time or whatever we want to pull out a string on that。 Input a little hairier because with input we might be ending up touching naive time zones。

 and naive date times at some point and we want to avoid that。 So if we're handling input。 the best way to do it is actually don't handle a naive date， time。

 So you can build that date time with the time zone in it right away。 You're great。 Well。 you're half great。 Then ask time zone that in UTC and you're great。 If you can't， so for example。

 if you're using stir time and that hands you back a naive， date time。 you do need to attach a time zone to that。 And here's where we get into a little bit of potential trouble because the way to attach。

 a time zone to a naive date time is replacing the TZ info， which if you remember back to。 cardinal sin number five， you don't want to do replaced TZ info on an aware date time。

 It's fine on a naive date time because what you're saying here fundamentally is， I know。 what time zone this is and I'm filling in that information。

 So that's why I have added here this assert statement， assert that the time zone information。 is none。 Please use an actual exception there if you're writing this code。

 But check that this is a naive date time before attaching time zone information to it。 so that you don't accidentally replace time zones on aware date times by accident and。

 then as time zone into UTC。 ISO strings slightly easier because we've got that UTC offset baked in。 So here we can use stir time or from ISO format， get that into an aware date time and as time。

 zone that into UTC。 As long as we don't stop halfway。 the thing that's weird if we stop halfway is that's， not a time zone in that string。

 That's a UTC offset and those are different。 That doesn't tell you US Eastern。 That tells you UTC offset of four hours which you will notice is not the US Eastern time zone。

 So all that tells you is enough information to get to UTC and nothing else。 So don't stop halfway when converting ISO strings。

 Now one of the reasons that we've done all of this work， finally we can do duration math， in UTC。 So here's this pesky example from earlier 30 minutes before daylight saving time in US， Eastern。

 If we add times on that in the UTC， we add two hours and then we add times on the back。 into US Eastern， we get the correct two hours later date time of 4。30 in the morning and notice。

 that 4。30 is two hours after 1。30 because the UTC offset has changed by one hour。 Now here's the other side of it。 If what you want to do is calendar math。

 you need to do that on dates。 If we've got， let's say we've got August 1st here。 If we convert。 we do that in local time， we pull out the date， we want to add 120 days。 Great。

 November 29th that is in fact 120 days later， take my word for it。 But if we do 120 days as a UTC duration， we're going to run into problems because daylight。

 saving time， an extra hour gets added and so our duration doesn't actually cover all of。 that because fundamentally， we're not trying to add 120 days of duration。

 We're not trying to add 120 days times 24 hours times 3600 seconds。 So if we do that。 what we end up with is this thing a day early with 23 hours hanging out。

 and then if you call date on that， you're going to be missing a day due to rounding errors。 So really this guy right here， that's what you actually want to do。 Do the rule here。

 If you've got dates， do date math on dates。 If you want to do time math， do that on UTC date times。 All right， we're going to do a couple things， couple extras for those of you using Django。

 If you're using Django， Django has wonderful time zone support。 Use it。 That's easy to true。 This will happen by default in Django 5。 Please do not wait that long。

 And then Django has a setting time zone and this defines the default time zone for your。 Django project。 If all of your users are in one time zone。

 sure set that to be your user's time zone。 If not。 set it to UTC and then what you need to do is you need to call activate on your。

 user's time zone whenever you're working with that user。 Best way to do this。 store the time zone name on the user object， call this in the middleware。

 So that whenever you're working with that user， right， for the rest of that view or whatever。 for the rest of that view， you've got the user's time zone active。

 And Django will then do magic for you。 Django templates automatically convert to the active time zone。 Whoo！ That's so cool。 So if we've got the US Eastern active。

 we've got UTC date times in our database， we render， that in a template。 Boom。 converted to US Eastern。 Love it。 Django then has a bunch of utilities。

 most of which also work with the active time zone。 The one that doesn't， this is just truly generic。 time zone now。 This is Django's time zone module， by the way， not the date time。time zone module。

 That will give you the current time in UTC with that UTC time zone attached。 Great。 If you want the current date in the active time zone， you can call local date and that。

 will do the work of， you know， putting the time zone into date time now and pulling out。 the date for you。 If you want to convert UTC date times to the user's time zone。

 we've got local date and， local time here， both of which will convert to the active time zone and then local date。 pulls out the date。 Local time leaves it as a date time and you can do whatever you want。

 like call stir time， on it。 And then finally going back in。 So if we're taking user input。 if we get in a naive date time， Django has this handy， function make aware。

 which does exactly what you'd expect。 It turns it into an aware date time。 This is the same thing that I showed you earlier with replacing the time zone info with。

 the added benefit of it will check that this is in fact a naive date time so that you don't。 accidentally replace the time zone on an aware date time。

 And then of course the thing to do with that as time is on it into UTC。 So the summary here。 pretty simple。 Take your input， get that into UTC。

 write all your code against UTC and then when you're。 done then you actually need to get talk to the user again， get that back into the user's， time。

 That's all I've got for you today。 Thank you very much to you all。 I'd also like to extend a thank you to Paul Gansil in addition to authoring the zone info。

 module that got added to the Python standard library。 So wonderful。 We can now all have the benefit of his work。 He also reached out to me and really helped me hammer through some of the thornier examples。

 in this talk。 So really appreciate that。 This is my email right here。 If you've got questions。 I will be sticking around afterwards but anyone on video， if you've， got questions。

 feel free to email me。 If you need contract software work， time's unrelated or otherwise。 happy to help。 So drop me a line。 Thank you。 [APPLAUSE]。



![](img/da32327101e4d6f29aad8f16815471db_3.png)