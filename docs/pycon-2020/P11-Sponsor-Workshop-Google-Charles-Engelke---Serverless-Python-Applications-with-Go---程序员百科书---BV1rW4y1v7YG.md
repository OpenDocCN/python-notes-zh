# P11：Sponsor Workshop Google Charles Engelke - Serverless Python Applications with Go - 程序员百科书 - BV1rW4y1v7YG

 Hello。
![](img/d75a7af45610ca4b253c770090edc225_1.png)

 Welcome to our workshop at Virtual PyCon 2020。 This workshop is how to build a distributed serverless application。 I'm Charlie Engelke。 I'm a developer programs engineer for Google Cloud。

 That means I work on samples and examples on how to use various Google Cloud products。 specifically serverless products。 This workshop was built together with Lori White。

 who's a developer advocate also in， Google Cloud working with higher education。 The slides and exercises for this can all be done on your own afterwards， even without。

 following these videos if you're interested。 All of them are available at the website serverlessworkshop。dev。 Feel free to share this with others， even if they're not at Virtual PyCon。

 What we're going to do in this workshop is actually build a loosely coupled， event-driven。 distributed serverless system today。 So you're going to do this on your own laptops。 Get them ready。

 We're going to talk first about what we're going to build。 The idea here is we're going to create something to manage a programming contest。

 We'll talk a little bit about that more in detail。 But the contestants are going to build things that play games。

 And we're going to use a very dumb game because we want to cover the concepts of how to build。 the application and use the environment less on how to create the logic of a good game player。

 But the same idea could be used with other games and with other kinds of things other。 than games and programming contests。 We're going to talk about what serverless is， what it means。

 and why it can be useful， for this。 Describe the problem in a little bit more detail and then talk about what our solution will。 look like， the general architecture， before we get deeper into it and hands-on。

 We'll then give an overview of the tool we're going to use， which is Google Cloud， primarily。 through the Google Cloud console， a website that you'll be able to work with hands-on。

 And then we're going to do the hands-on solutions。 There are three main parts to the programming contest solution。

 And we have a separate hands-on code lab for each of those three parts。 Finally。 we'll finish with a bit of a recap。 So if you're ready， let's go take a look。

 The game that we're going to be having the programming contest players build is a number。 guessing game。 We wanted to keep the simplest possible thing。

 So the game will be asked to guess a number。 It'll be given a lower and upper bound。 And also it'll be given maybe a history of previous guesses。 So the first time around it may ask。

 hey， guess a number from 10 to 50。 And it'll come back and guess， I don't know， 17。 The next time through we'll say guess a number from 10 to 50， you already guessed 17， but。

 the answer is higher than that。 The programming contest solution will come back and say， well。 maybe it's 38 then。 And then the next time through we'll say guess a number from 10 to 50。

 Your previous guesses were 17， but the real answer is higher and 38， but the real answer， is lower。 And we'll keep going until either the game playing program comes up with the correct。

 answer or crashes， fails to answer at all， or in some cases the judging system just gets。 tired of asking。 We've asked you a hundred times to guess a number and you still haven't guessed it。

 We're not going to ask anymore。 The first thing we're going to do here is have you build and deploy the serverless。 guesser， the kind of thing a contestant would do。 Then we're going to look at the overall system on how we actually judge the submissions。

 how， we keep track of standings， how we authenticate users， all of that's going to be built in an。 entire contest judging scoring system。 That's going to involve multiple serverless components that are loosely connected via。

 messaging and we're going to keep persistent information in a no SQL data store。 There'll be code labs for every part of the above。 We're going to do this with serverless computing。

 At this point we always get some people bring up the fact that there are still servers。 That's the case。 Other people like to say there's no such thing as serverless computing。

 it's just other people's， computers。 Well， that's kind of true too but that's the whole point of serverless computing。 There are still servers。 They're not your problem。

 They're the problem of the infrastructure provider you're dealing with。 In this case it's going to be Google Cloud Platform。 You don't have to provision them。

 You don't have to back them up。 You don't have to manage them。 You don't have to monitor them。 You don't have to scale them。 You don't have to do any of that stuff which means going from figuring out what you want。

 the software to do until you've actually got it deployed and running reliably is very。 very quick compared to other solutions。 Not only that。

 a lot of serverless options scale down to zero。 So if you've got something that is often idle or has very bursty usage。 you're only， paying for the actual usage and when it's not being used you're paying little or nothing。

 There are a couple of major different flavors of serverless computing。 A lot of them are based on using containers where you create containers in the serverless。

 platform managing scaling them， launching them as needed， replacing them when they're。 unhealthy and so forth。 The other flavor is fully managed。 You just bring your application code。

 Nothing else。 The environments and all of that are managed by the platform。 Managed serverless computing is what all of the solutions today are going to be built on。

 You're going to be responsible for the application code。 Write your own programs。 The cloud platform is going to handle everything else。

 Keeping an up-to-date programming language environment including all your programming， libraries。 keeping all the tools and APIs you need healthy， scaling your platform up， or down， backing it up。

 All that kind of stuff is going to be handled by the platform。 One thing you need to remember though is that your code may be unloaded， reloaded or。

 loaded in multiple hosts at any time。 One thing that you may be used to doing when you build solutions is keeping track of state。 in memory or on disk。 You're not going to be able to do that with serverless computing because there may be a。

 lot of instances each with their own memory and their own disk and you can't rely on them。 to know what you did on your last time through。 You're going to have to keep any of that information externally to your software。

 You also have to deal with the fact that sometimes when a new machine needs to be launched to。 run your serverless because you're scaling up or scaling from zero， there may be a little。

 bit of startup latency。 Although that's usually not too bad and every serverless platform works all the time to。 reduce it so that it's not going to be a problem in most cases。

 When we're talking about serverless computing， we're talking about software that has some。 common characteristics。 It's got to be stateless， as I already mentioned。

 You're going to need to use an external data store if you need to keep track of anything。 from one invocation of your software to the next。 It tends to consist of a lot of pieces loosely coupled。

 Each piece is going to handle one well-defined problem and then when that problem's done。 it's going to do something that may trigger other pieces to deal with other parts of it。

 You don't have to sit there and think about every single piece you're writing as being。 the end-to-end solution， just dealing with one task。 It's event driven in most cases。

 When something happens like a web request coming in as a very common event， but perhaps。 somebody updates something in storage or sends a message， your software is going to be launched。

 and run to handle that event。 It often relies on asynchronous communications。 Unlike a web request。 where the request comes in， the software deals with it， responds， and， then goes away。

 often a message will come in or an event will happen， the software will， deal with it。 but nobody's waiting for it to finish。 Whatever sent that message just relies on the fact that it'll be handled eventually and。

 when it goes and needs to deal with the result， it'll check to see if it's actually finished。 or not。 It happens asynchronously， which means we've got a lot looser coupling among these components。

 So the problem we're going to solve today is that of a programming contest， the kind。 of thing that a high school or undergraduate program might run。

 A lot of you in your earlier days likely participate in a programming contest。 And the way they work is a bunch of programming problems are written up and they're given to。

 the contestants to create their own solutions。 And the problems tend to be given this source file。 do this kind of calculation or processing， and produce output， which we will then judge。

 We'll create a bunch of， as the judges， we're going to create a bunch of different source， files。 We'll test your software against them and see if you produce the right output。

 Contestants write their own solutions。 They test it with the sample data that's given out in the problem and if they're really。 good， they actually write some additional sample data to test it， not just rely that they work。

 on the simple cases。 And then they turn the solutions in。 In the early days。 that was on physical media like diskets or CDs or memory keys。 Nowadays。

 it's probably going to be through email or uploading into a web form。 Regardless。 the judges get these solutions。 They have to compile and run them against all their different test cases and then mark。

 whether they succeeded or failed or crashed。 And then there's another system that keeps track of all that and scores it based on when。 it was turned in and how many problems were solved correctly， that kind of thing。

 Contestants find out whether their submission worked or not。 If it didn't。 they can go ahead and try again。 If you've ever worked on the judging side of this。

 you know that managing these submissions， is a complete and total mess。 You've got to keep track of what contestants submitted， what solutions and when they were。

 submitted， especially if physical media is involved。 And you've got to avoid malicious code messing up your test beds。 In fact。

 it's not even necessarily malicious code。 It can just be dangerously buggy code that makes your test beds unreliable and the next。 time you go to score a solution， it may fail even though there's nothing wrong with that。

 solution is the previous test has just left your system in a bad state。 And you've also got to deal with different machine configurations。

 The contestants may be running their stuff on different operating systems with different。 versions of your programming language or libraries or even totally different languages。

 You've got to have all those on your own machines and match them up pretty well。 The solution that we're going to use is to say， hey， it's hard to deal with submitted， programs。

 so don't submit programs。 Instead， after the contestant writes their program。 have the contestant provide the infrastructure， that the judge is going to use as well。

 We're going to run the solutions on the contestants own infrastructure。 When we look at how the problems are written， here's an input file， process it and produce。

 an output file。 That's how HTTP request works。 So that's what we're going to do。 The contestants will write their solutions and deploy them to a web server and just tell。

 the judges， here's the URL。 In order to test my assignment。 take your input file and send it as a post request to， my web server。

 My program will run and it'll return as its result， the return page or return data at any。 rate is going to be my solution to the problem。 So if my program doesn't work or messes things up。

 as a contestant， I'm messing up my infrastructure， not the judge's infrastructure。 The judges have a much easier job keeping track of stuff。

 They can also hit that web server multiple times with all sorts of different scenarios。 and different test files。 They probably will want to randomize their test out a little bit so a contestant that。

 can't figure out the solution doesn't end up just hard coding a response given an input， file。 Let's take a look at a high level system diagram。 The contestant is doing their work on their own laptop。

 After they get a solution that they think is correct， they deploy that solution to their。 own web server， which gives them a URL that's available to anybody on the internet。

 They submit that URL to the judging system probably through a web form。 The judging system then talks to that web server and does all the necessary testing and。

 updates its standings and scores。 The contestant at any point can go look at the judging system standings page and see。 the status of all their submissions and how they stand relative to everybody else。

 We're going to build， first of all， the contestant solution。 That's the smallest。 easiest part of this overall problem。 So we're going to get a simple start to serverless computing using what we expect contestants。

 to be able to do。 We're then going to look at the bigger judging system。 which has a lot more concepts that， we need to apply on how we have multiple components interacting with one another to build a good。

 serverless distributed solution。 You've got to ask if you've ever done programming contests whether this is practical。 You've got these contestants busily writing their solutions。

 Can we expect them to manage and deploy their own web servers as well？

 And if managing and deploying their own web servers means they provision and create a machine。 put it on the internet， install web server software， configure each of their solutions。

 is a different path in that web server。 The answer to that is basically no。 Programming contests are fairly short。 Students are focused on just the problems。

 That's not going to work。 But if they're using a lightweight managed serverless platform。 it is very feasible as， we're going to see。 It's not going to be a difficult hurdle for students to pass。

 In fact， the platform we're going to use for the students， for the contestants， is one。 that gives you the shortest possible path from something working on my machine to something。

 deployed and running successfully on the internet。 That's why we're going to start the workshop with that part of the problem。

 The contestant writes and deploys a solution to the web。 Then we're going to get to the bigger system after that。 We're going to use Google Cloud Platform。

 Well， that's who I work for。 That's what I'm most familiar with。 But other Cloud platforms could do all the same kind of thing。

 Any of the major Cloud platforms has similar capabilities to the ones we're going to use。 on Google Cloud。 The steps and details would be quite different。

 The effort involved would be somewhat different， but it still should be doable。 All of our code for this workshop is on GitHub， and we're going to give you the URL in a minute。

 So feel free to fork that repository and adapt it to another platform and let us know。 We'd be very interested。 The resources you're going to need today are just a laptop with an internet connection。

 and a modern web browser。 Everything's going to be done through the web browser。 You're not going to have to install any other software on your local laptop。

 You can use any modern standards based web browser。 Google Chrome， of course， will work。 but so will Firefox， Safari， Edge， and probably， some other browsers I don't even know about that are nevertheless modern and standards based。

 You're also going to need a Google account。 Now if you've got a Google account through your university or work。 what we call a G Suite， account， it may well work。 It can work。

 but the administrator of your overall university or company system can turn off， this capability。 So if you have any issues trying to use your work or school account， instead go ahead and。

 set up a plain vanilla account， Gmail account， and use that for these examples。 The materials are all online。 The slides are at serverlessworkshop。dev/slides。pdf。

 The source codes on GitHub at Google Cloud Platform， serverless game contest， and the， code labs。 step-by-step instructions for each of the three sessions we're going to， go through。

 are also available on serverlessworkshop。dev。 So one thing to remember throughout this is that you can get the latest state of all the。 resources we're working with and share them with others as well if you want at serverlessworkshop。

dev。 We can take a short break right now and when you come back in the next part we're going。 to go over how to get stuff up and running on Google Cloud Platform and then we're going。

 to solve the first part of the problem， the contestant writes a game-playing solution and。 deploys it on the internet using a serverless tool。 Thanks。 Well。

 with that introduction out of the way， let's go ahead and start with the first hands， on piece。 the game player that each contestant will write。 We're going to work with his hands on and all the materials you're going to need are again。

 available online at serverlessworkshop。dev。 In order to do this work we're going to have to build a GCP project。 a Cloud Platform project， and that's because all of the Cloud Platform resources live inside of a project。

 You've got one account and that one Google account can have many projects in it and you。 can share ownership of projects with other Google accounts。

 Resources in the same project usually can interact with one another but you can enable。 resources in different projects to interact and you can also restrict resources in the。

 same project from interacting。 One of the nicest things about Google Cloud projects is that when you're done using a。 set of resources you can just delete the project and all the resources go away there's no chance。

 of getting any future billing showing up by surprise。 Now in the real world in this programming contest each contestant would have their own。

 project and the management system would be a separate project and probably the judging。 programs would be separate projects as well all owned by different entities but to keep。

 things simple we're going to just use one project for everything in the workshop we're。 going to talk about when you might have to do extra work if they were in separate projects。

 Turns out there's not many places where that comes out to be an issue because of how we're。 architecting this system。 We're going to do the work with the Google Cloud developer console that's at cloud。

com。 We're going to go and do that hands-on once we start the actual code lab。 In that console you'll need to build a new project there's a drop-down at the top of。

 the console where you select a new project you click for the selection and click on new。 project rather than selecting an existing one。 The system may already have created a dummy project for you and you can use that if you。

 want。 When you create the new project you give it a name and you can call it anything but its。 ID will probably be based on the name but it won't exactly equal the name unless your。

 name is globally unique across all of the cloud platform。 So if you call your project serverless workshop the project ID is probably going to turn out。

 to be something like serverless workshop some long random number。 I tend to call my project something with my name at the beginning because it's pretty。

 rare so if you call the project angle key serverless workshop well you'd get a randomly。 selected number at the end of it because I've already done that but I was able to get that。

 name and ID coming out to be the same。 One thing you need to be careful of is the project name is going to be in URLs so if you。 want to share somebody share the URL with somebody so that they can actually invoke your。

 program that you're creating you want to create a name that you're not embarrassed to， share。 So we're going to start simple。 This first code lab is going to be building the contestants game player。

 We're going to take the role of a contestant and we're going to write a program that accepts。 a web request representing the current game state and we're going to respond with a single， move。

 We're going to deploy that program to the internet， submit the program for judging by providing。 the URL through a web form and the judging programs are going to invoke our program over。

 and over again。 They're going to invoke our program with the first move and then based on the response they'll。 maybe make another request for the second move and so forth。

 We're going to talk about how all those other parts work later。 So recall our high level system diagram。 We're going to do our work on our own laptop and one as a contestant anyway。

 When we're ready we're going to deploy our program to a web server and then we're going。 to submit the URL to the judging system which will test our program in the web server and。

 eventually update the standings so we can see where we are relative to everybody else。 For this code lab we're going to work with the player。

 The program is deployed to our own web server。 The platform we're going to use is Google Cloud Functions。 This is a managed serverless program where we just provide a program， our source code。

 and there are a variety of languages supported including Python which is what we'll be using， today。 Python is extremely well supported in Cloud Functions and in fact throughout Google Cloud。

 We don't only have to provide our program we also have to say what event should trigger。 our program being run。 In this case it's going to be one web request comes in run our program and then the platforms。

 are responsible for listening for web requests to that URL when one comes in it will trigger。 our program and send the response from our program back to whoever made the original request。

 A big benefit of using this Cloud Function platform is we don't have to do any system。 administration at all。 We just write our program。 The program is going to scale as needed。

 If all of a sudden we have thousands of people wanting to play this game against us our program。 will scale up to handle it。 The more likely situation is our program is going to go idle for a long time where nobody's。

 interacting with it and in that case the platform is going to be able to scale it down to zero。 not incurring costs。 The judging system is going to then play a game against our program by first setting。

 the initial game state to the player and the player is going to look at that and say okay。 I'm going to make my first move and respond with that move。

 The judging system will update the game state maybe make a move of its own if it's a two。 player game and then it will send the new game state for the next move and the player is going。

 to respond with that move。 One after another。 Notice that the player does not have any memory in this case。 The only component here that's keeping track of the history of the game play is in the judging。

 system not the player。 That means our player doesn't have to keep track of who's making the request so when it。 gets one request after another they could be coming from different people or the same person。

 playing a different game our player system doesn't need to know that。 It's told what the game has been so far and asked for what the next move is。

 Let's do an example of that with a game everybody knows tic tach toe。 The initial game state for tic tach toe is an empty board a three by three grid with a。

 bunch of empty squares and we're going to represent that in Jason as a dictionary with。 a field called Mark so far that's an array of all the marks that have been put on the。

 board and they're having been any at the beginning and then what the next mark should。 be your mark when we're talking to the player and the first mark should always be an X when。

 you're playing tic tach toe so the player is going to take that game state and respond。 with its move and pretty much everybody always makes the first move by putting their X in。

 the middle of the grid。 Row two column two our game numbers and get rows and columns one two and three。 The judging system is going to process that move and then make a move of its own so the。

 second request coming from the judging system may be something saying here's an array of。 the mark so far there's an X in row two column two and there's an O in row one column one。

 the upper right hand corner。 It's your turn your mark is an X where you're going to put the X and the player is going。 to respond with a Jason representation of where they want to put that X in this case it's。

 going to be the middle of the top row row one column two and this is going to continue。 until the player can't make a move or until the player wins or until the player completely。

 loses or even crashes。 Any of those outcomes are possible。 One thing we want to look at when we're dealing with a distributed application like what we're。

 building is how tightly are the different components coupled how much do we have to know。 what the opposite part of the system is doing in order to write our part of the system and。

 in this case we have very very little coupling between the player and the judging system。 The only connection between them is that the judging system makes HTTP requests over the。

 public internet to the player so there's no issue of sharing resources dealing with permissions。 just a matter of can I send a request and can I respond to that request。

 That's important because every contestant is building a completely separate player and。 we don't want them to have to coordinate with one another to avoid stepping on each other's。

 toes with resource sharing。 We also don't want them sharing resources with the judging system which could even be。 a security problem。 In general not just for a contest like this but for pretty much any kind of distributed。

 system。 Minimizing coupling between components makes that system design， deployment and maintenance。 a lot more flexible and secure。 We're going to keep that in mind throughout not just this first part。

 Our game is going to be much simpler than tic-tac-toe because we don't want to spend our time talking。 about how the game play should work we want to talk about how a system gets architected。

 So we created what we think is pretty much the simplest possible game。 Well we didn't really create it everybody's done this before。 The guess and number game。

 We give the contestant a minimum a maximum number and a history of guesses so far and。 the contestant comes back and makes a guess a whole number。

 We don't worry about the judging system we just have to write the player but if you want。 after we're done building this and we'll come back to this at that point you can actually。

 submit your solution to our example judging system at serverless workshop demo。appspot。com。 Well the game is going to be played by the judging system sending a web request to our。

 player and the web request is going to be a JSON representation of the state of the game， so far。 Well at the beginning we may say guess a number from one to ten and that would be represented。

 by the JSON object with a minimum of one a maximum of ten and an empty history。 No guesses so far。 The playing system is going to respond with its guess also in JSON。

 For example it may just return six and yes six is the JSON representation of a whole number。 We don't need any quotation marks or curly braces or anything like that just six for our， guess。

 The second time around the judging system is going to say make another guess of a number。 between one and ten you previously guess six but the real answer is higher than that and。

 the game is going to come back and make its next guess。 Ideally it will be seven eight nine or ten but we don't know how smart our player is maybe。

 it'll guess eight thousand。 It doesn't matter it's going to continue until the judging system gets tired of letting。 the player make guesses or until the player guesses a correct number。

 So let's go ahead and build and deploy this solution。 There's a hands-on code lab available again the link is available from serverlessworkshop。dev。

 and the particular link for this is serverlessworkshop。dev/player。 So here is the code lab。 It's a step by step move through how to build a game。

 It explains a background that we've already been covering。 You've got to create a solution called a player as a web service to make one move at a time。

 We explain what the game is。 The game we give examples of the JSON input and the JSON output and describe what we're。 going to build。 We're going to build a game a cloud function that looks at the request to see what the。

 state of the game so far is and makes a guess and return that single integer。 We're going to write this using Python 3。7 although if you're watching this workshop fairly。

 far in the future we may instead be supporting Python 3。8 nothing about what we're going to。 do will really change much at that point and we're going to see how to test that cloud， function。

 In order to do this you're going to need a modern web browser such as Chrome or Firefox。 or Safari or Microsoft Edge any modern web browser can do this work and you're going to。

 have to have some basic understanding of the Python programming language。 So in order to get set up we need to build a GCP project。

 This is the first part of the game playing system you've ever worked on so we're going。 to create a project for it and we're going to use that project for all the other parts， later on。

 You go to console。cloud。google。com。 Let's take a look at that console。 Here I've already created a project called serverless workshop angle key or I could have。

 called it angle key serverless workshop。 You can see the project name and in this case a project ID are the same。 There's also a system generated project number。 We're going to work using the cloud shell which is a command line available through the。

 web browser that runs on a virtual machine uniquely created for you in cloud shell。 You just click this activate cloud shell button。 It takes a minute to start the cloud shell virtual machine。

 There's no charge for this virtual machine。 It's very nice。 It's a Linux command line。 You could do all this on your local machine and Mac windows are Linux but you have to install。

 some software all of which is already installed in cloud shell。 Alright now that we're in cloud shell what are we going to do？

 Well the first thing we're going to do is get the source code。 That's on github and so we need to issue a git clone command。

 And again all the software we need in this case the git clone command is already built。 in to our cloud shell environment。 So let's paste this with control V and it does the clone and if we do an LS we see that。

 it has downloaded all the stuff from that github repository。 We're going to want to take a look at this。 So let's go ahead and open the cloud shell editor。

 The look and feel of this has changed over time and it may change again if you're dealing。 with this a few months down the road but basically you're going to have the same thing where。

 you've got a command line cloud shell and the ability from there to click to open interactive。 easy to use web editor。 The first time you open it can take a minute。

 And here we are we see our editor we can go and look at the player and we've got a variety。 of different python programs that can play the game。

 It's a very very simple program to play a very very simple game。 Okay let's go take a look at the code lab and see what we should be doing next。

 We're going to create test and invoke a cloud function to play the game。 So again we need to go back to the console and we can open another instance so we can。

 not have to go back and forth between different parts of it all the time。 That's something I really prefer to do rather than keep navigating forward and back just。

 open another tab。 So we're going to have the console at console。cloud。google。com。 We need to collect select our project and create a new function by going to the cloud。

 function section of the compute component。 So when we look at the console over here we have a menu icon we click on it and we have。 all the different things we can do in the console broken down into general categories。

 In this case we're looking at the compute category and we want to look at cloud functions。 The first time we ever go to this we may have to wait a minute for it to initialize the environment。

 but fairly soon we're going to have the ability to create a new function。 Okay we've got a form to create the function let's step back to the code lab to see how。

 we should fill this form out。 We need to name the function player we need to decide what kind of trigger we are and。 we're going to need to put in our source code。 So let's take a look at that again。

 Let's call this function player we can call it anything。 I always leave the amount of memory at the default unless I know I need more or less。

 What you pay depends on how long your functions are running times the amount of memory you're。 using。 How we're going to trigger this function what kind of events can cause this function to， run。

 You can see there's quite a variety of different kinds of events that can be handled by functions。 We're going to do the simplest one handle an incoming web request。

 It shows us the URL that our function is going to be advertised at。 We do want to allow unauthenticated invocations。 That's because we want the judging system to be able to ask our function to play a game。

 without having to have an account we've already authorized。 We're going to do our work with the inline editor and we're going to do Python 3。7 and。

 you'll notice it already fills in a sample Python program。 I'm going to expand this editor window so we can see it more completely。

 The sample program has a single function in it called Hello World。 It's invoked with a request object and that is a flask framework request object if you're。

 familiar with the Python flask framework。 That request object has a variety of methods on it one of which is get JSON。 So if the request comes in with a body that is JSON this will let us get a dictionary。

 that is loaded from the JSON object。 If it's got a message in it we go ahead and just return the message。 So this is kind of a weird sample program but it is a decent sample program。

 We're going to completely replace this program with our own。 Let's go back to the tab that has the editor open。 Here is the program skipping all of the comments。

 Let's select it all。 We're going to be able to see it and replace everything in here。 I guess I have to do control V and that's our function。

 We need to import the JSON module because we're actually going to be just working with。 JSON namely for our response。 So our function is called make guess。 It's sent a request object。

 We get the JSON out of it and we look at the field called minimum and that's what we guess。 We're not a very smart playing function。 If you ask me if it's to guess a number between one and ten I'm going to say one。

 If you tell me you previously asked five and it really should be higher than that I'm。 still going to say one。 Nothing ever said we had to be smart just so we have to follow the rules。

 We also need to fill in the requirements。txt which is a list of any libraries that are not。 standard on this platform。 JSON is built in standard so we don't have to put anything in the requirements。

txt file。 We then need to tell it that the function it should invoke because there might be a lot。 of different functions in here。 The function invoked by the platform may call others。

 We need to say which one the platform should invoke and it's going to be make guess。 We can also fill in environment variables a lot of other stuff none of which we particularly。

 need right now。 We can set up firewalling all sorts of interesting things。 However we're just going to go ahead and click create to create our function。

 It comes up we have a little spinner showing that the work is happening that spinner should。 eventually turn into a green check mark。 If instead it turns into a red X you'd be able to look at the log for messages for what。

 went wrong。 For example one thing that happens to me a lot is I use a non-standard Python library。 and forget to include it in the requirements。txt。 It's pretty clear from the error message that that's what I've done。

 While we're waiting for that to finish initializing let's go back and look at our code lab to see。 what we're going to do next。 Next thing we're going to want to do is test the function。

 We're going to test it with an event that asks us to guess a number between 1 and 10。 with no history。 Let's go ahead and select that JSON body that should be sent to it to test。

 Let's see if our console is ready yet。 It is。 It's going green so let's go ahead and click on the name of it and we see that there are。 four different， pardon me， five different tabs here。

 Something in general telling us about how many invocations it's had， none yet。 A page telling us how we trigger it， the URL for it。

 If we open that right now with a web browser we'd get an error message。 In fact let's go ahead and do that。 The reason we got an error message is we did not write our function to handle a regular。

 get with no body。 We wrote our function to deal with a request that includes a JSON body and the web browser。 can't send a JSON body。 We're going to have to test it another way。

 We can look at the source code and if we want to change the source code we're going to have。 to click the edit button which I generally take about five minutes trying to type in here。

 before I realize I've got to click the edit button first。 We can set the permissions。 Maybe this is only useful if we say require authenticated requests only。

 In this case we're not going to do that。 We're going to let anybody make a request。 And finally testing。 We can fill in the JSON object that should be sent as part of that event。

 So we're going to paste what we copied， guess a number from one to ten with no history and。 click test the function。 And the function needs to be initialized since it's never been running presumably and it。

 comes back and makes a guess of one。 It takes a few minutes。 The logs always lag a little bit but eventually log entries of that invocation will show up。

 So we've tested it， seems to work。 Let's test it with a more complicated environment where we actually send it a history。 So we go back in here， paste the more complicated JSON and say test the function and it gives。

 us one again。 The reason the source code is called badplayer。py is it's a very bad player。 Okay。 now we've tested the function。 It seems to be working but we don't really know if it's working when called over the internet。

 We know when it's working when handled in the testing environment provided by the cloud， platform。 So let's go ahead and test it over the internet。 So we're going to use the code editor create a file called game。

json with either of these， JSON objects in it。 So let's go back and look at this editor。 Let's create that game。json and say new file under player and paste in the test environment。

 and it will save automatically although you can do control S to force it to if you're like。 me and wondering is it really saving fast。 And then let's go back to the command line to run our test。

 We see， let's see， we've got to go into the player subdirectory and we see the game。json。 that we just created。 We're going to use a curl command to make a request over the internet to our cloud function。

 So is a general purpose command line tool that can make pretty much any kind of web request。 In this case we're going to make a poster quest。 We're going to send a header that says our poster quest should have a header saying the。

 content type is applications that slash JSON because the data we're sending from the file， game。json is in the JSON format。 We then have got to put the URL of our function in there。

 So let's go take a look right now at the command line。 Place that， whoops。 I didn't copy the command first。 Let's copy that curl command。

 not including the whole placeholder for the URL。 We'll get the real URL in a second。 And the real URL is available from our function console under trigger。

 So let's copy that link address， go back to our cloud shell， paste it， and now we're。 going to make an actual request over the internet to our cloud function。

 And it didn't take very long to come back。 You might not be obvious that it did come back。 but there is the one。 We sent the JSON representation of the integer one。

 which is just the integer one。 No new lines， no character turns， nothing like that。 So curl printed it and then we immediately see the prompt for the second request。 Well。

 it's working over the internet the way we want it to。 Okay let's take a minute to go and look at another way to create a cloud function。

 Instead of creating it through the cloud console GUI， we could create it from the command， line。 And again， that command line could be on our own machine if we had installed the necessary。

 software， but we're going to do it from the command line in the cloud shell。 So we're going to go to the player directory and in order to do the deploy using the deploy。

 using the gcloud command line interface， we're going to have to have the function we're deploying。 called main。py。 Right now that's just the way the gcloud function works。

 It looks for a function called main。py。 So we're going to take our okay player and copy it to main。py and then we're going to， deploy the function with this command。

 So let's copy the command for right now。 Let's go back to our cloud shell and let's copy the okay player to main。py。 And we see there are lots of other programs， but the only programs that are actually being。

 used by the cloud deploy function are main。py and requirements。text。 By the way。 let's take a look at what we're sending for requirements。text。 Nothing at all。

 And let's write our gcloud command。 Before we invoke it， take a look at the pieces of it。 gcloud is the name of the tool that's used for interacting with cloud platform pretty， much overall。

 Functions is the sub command saying we're going to work with functions as opposed to。 all the other kinds of resources available in Google Cloud。 What we're going to deploy。

 we're going to deploy this to a new function called or an。 existing function that we're going to call okay player。 Remember through the UI。

 we created something called just plain player。 This is going to be called okay player。 We specify what runtime we want， namely Python 3。7， how we want it to be triggered with a。

 web request， where the source code is， in this case is going to be in the current directory。 where we're running this command。 What the entry point is， that is when a request comes in。

 what function in this program should， be invoked， we only wrote one function。 but still we have to tell it that the function， is called make guess。

 And we want to allow unauthenticated requests so anybody can play the game against this function。 And it can take a little while to deploy the function。 Once it finishes。

 we can see it in the console。 Let's go look at the function list。 And we can see it's being deployed right now。 After it's deployed。

 we can test it through the console the way we did before， but we'll。 go back to the command line and we're going to test it with the curl command again。

 So while we're waiting， let's recap what we've done。 We created a cloud function。 which was pretty simple to use。 We just wrote Python source code in the console。

 We pasted that into a form and we said， run， deploy this function。 It tells us the URL it's going to be at and we can then invoke it over the web。

 We've just done it with the command line instead of the interactive editor。 And we want to invoke it。 We're going to use the same curl command we used before with one change。

 The URL， if we go look at our functions console for OK player， the trigger is almost exactly。 the same， but the tail part of it is called OK player instead of just player。

 So let's go back and recall that previous command has just changed it to OK player。 And see what it says。 Now this player is a little bit smarter。 So we gave it a history。

 We told it guess it number 21 and 10， but we told it was higher than 5 and lower than， 8。 So let's see what OK player actually comes back and guesses。

 Let me scroll up a little so it's easier to see。 It came back with a 6， which is a legal。 possibly correct guess。 And we could go ahead and make more and more guesses and eventually OK player would win。

 If we want to look at the source code of that， we can go back to the editor and that was， main。py we deployed。 You can see it's much longer source code。

 It gets the minimum value and it goes through the history and it guesses the minimum value。 and every time the history says， hey， it's higher or lower than a number， it either raises。

 or lowers the guess so that it's just barely legal， just inside the range。 So if we wanted to make a guess from 1 to 10， it might take up to 10 moves。

 If we wanted to make a guess between 1 and a trillion， it might take up to a trillion， moves。 It's not a real smart player。 It's just an OK player。 OK， we're done with that cloud function。

 Again， serverlessworkshop。dev has links to all these other code labs as well as the slides。 Let me bring up the slides again。 If after you build your function， you want to try them out。

 go ahead and point your web， browser at serverlessworkshopdemo。appspot。com。 There'll be a form there for you to paste your URL in and a name so it'll stand out。

 in the standings as to what name is actually running this。 And you can submit the player and look at the responses。 So again。

 we have a game where the player does one simple thing， make one move given， the existing game state。 The player doesn't keep track of any of that state， which means a lot of different systems。

 can be playing this game against it at one time without interfering with one another because。 the judging systems are actually keeping the state。

 These are made in response to a web request and the cloud function platform invokes the。 code whenever a request arrives。 We don't have to manage a web server or anything like that。

 Before we go on to the next part， just want to make you aware of a possible gotcha that。 once in a while gets people。 Even though we wrote an entire program。

 I mentioned several times that we need to tell， the cloud platform which method in our program to run。 So one mental model that you could develop in one point I had is that every time a request。

 comes in， the platform runs our program and invokes that function。 But that's not true。 If the program's already been run and is already loaded in a live instance， the cloud platform。

 doesn't start it up again， it just invokes our function。 Which means if we've got global initialization code or global variables from one run to a， next。

 they can interfere with one another。 So be careful of that。 I know somebody who actually tried to create a complicated gaming system that used a binary。

 Python library that had an awful lot of global state in it and was never able to make it。 work as a cloud function because each request interfered with what happened in that global。

 state for other requests。 So again， remember that your code needs to be stateless。 That completes the player， we're now going to move on to the more complicated environment。

 the judging system after a break。 Now that we've built a sample game playing submission， the player。 it's time to look， at the larger， more complex and frankly more interesting aspect of the game playing system。

 Remember， as we go through building the judging system that all the slides and exercises we're。 going to be dealing with are online at serverlessworkshop。dev。

 Recall the high level system diagram of what we're trying to build。 The contestant does the work on their own laptop and when their solution is ready， they。

 deploy it to a web server， a Google Cloud function web server that's under their control。 Once they do that， they have a URL that they submit via a web form。

 The judging system is responsible for testing that game playing program in a variety of scenarios。 and eventually keeping track of the scores， updating the overall standings and making。

 that available for anybody to look at on a web page。 This looks like a monolith。 It's in there just as a box。 Judging system does everything。

 The traditional approach might be to implement this as a single web server application。 What does it do？ Well， it interacts with contestants through web pages。

 It plays games against submitted solutions by making web requests and attracts scores。 in persistent data somehow。 But if we built this as a single monolith。

 we'd be ignoring the fact that there are different。 communities building different pieces of this and that we can make these pieces more。

 loosely coupled and farm out the work more simply。 We don't want to restrict the flexibility in the design and future expansion by making。

 this one thing where if we change anything about the contest， we've got to change the， core system。 Instead， we're going to recognize that the judging components， the things that exercise。

 the submissions are often written by the judges who've written the problem statements， not。 by the people who run the overall contest。 That way， if we want to judge a different game。

 we don't have to rebuild the whole system。 We just have to build the single。 we build a single judging component。 The other problem here is that if we made this a single web system。

 when somebody made a， submission， they'd have to wait for a response to come back until the submission was processed。 meaning all of the sample games were played， which would be a delay unless we ran a more。

 complicated system that handles background processing or concurrency。 All that could be done。 but it's a lot easier to do this in a distributed serverless environment。

 So let's look at what we need to build one thing at a time。 Let's pull out the first piece。 which is something needs to play this game against， the submitted solutions。

 We're going to call this something the questioner。 The questioner is going to go ahead and ask the player， in this case， guess a number。

 And it's going to keep a track of what the guesses were， and it's going to then ask again。 with the updated history over and over again。 And in fact。

 there might be a lot of questioners that are simultaneously hitting this game for。 different situations， different scenarios。 In order to do that。

 the questioner needs to know how to talk to the game player。 It needs to know the URL。 It will then play the game against the player。 And when it's all done， it's going to have a result。

 It needs to know what to do with the result。 So we're going to build this questioner as an independent component。 We're going to give it the URL for the player。 And in terms of what to do with the result。

 we're going to say， hey， when you've got the， result。 put it in a JSON object and post it to a particular address， somebody else will。

 handle keeping track of it。 And again， we're going to punt this problem。 Take it down the road。 Let it be somebody else's issue on how to keep track of the results。

 The questioner is going to just deal with playing the game。 It's going to be easy to run multiple questioners against each submission that way。

 And we're going to use an asynchronous request to trigger the start of play。 So that when it comes time to ask the questioner to play the game， we just send that off and， say。

 please play the game。 When you're all done， here's where to send the result。 We aren't going to wait for you to finish this。 We don't care。 We just trust that once we ask you。

 you'll eventually get the job done。 Once again， we're going to choose Cloud Functions as our platform。 We could use any compute service probably， but Cloud Functions are a good fit because。

 this is a single well-defined task that is triggered by an event， the event being the。 judging system saying， hey， judge this submission。

 Now we're going to do that not by the judging system sending a web request， but by the judging。 system triggering it asynchronously。 Cloud Functions have a lot of different ways you can trigger them。

 What we saw with the player was triggering them with a web request。 But we want to trigger this one on an asynchronous message being sent to it。

 A message being published to a pub/sub topic by the rest of the judging system。 Now what is this pub/sub？ Google pub/sub is a reliable messaging system。

 And really every cloud platform has these kinds of messaging systems。 They're core to distributed computing where when one component needs something else to， be done。

 they don't make a request and wait for it to come back。 They just send a message saying。 do this job， we trust you'll get it done。 The messages that are being sent in Google Pub/Sub belong to topics。

 And when somebody wants to send a message， we say they publish that message to the topic。 So there might be a topic saying， play games and you'd send a message saying， play a game。

 against this URL， sending the results to this other URL。 And the program subscribed to a topic to get all the messages。

 So every one of these questioners would subscribe to that play game topic。 And they get triggered every time anybody publishes a message to it。 This can be one to one。

 one to many， or many to many。 Pub/Sub is asynchronous reliable delivery。 Messages are guaranteed to be delivered to every subscriber。

 But they're only guaranteed to be delivered at least once。 They could be delivered more than once。 In this case， that's not a big problem because it just means we waste our time judging the。

 same problem more than once but in the exact same conditions and presumably get the exact。 same result。 Also the delivery order is not guaranteed which for our purposes doesn't particularly。

 matter。 If people submit three problems and we judge them in a different order from what they're。 submitted， eventually it all comes out in the wash。 It doesn't really make a difference。

 So if we want to trigger this via Pub/Sub， the way it works is there's a topic called， play game。 The judging system when a new submission comes in just publishes a message to that topic。

 And in this case we might have three different questioners that judge the problem under three。 sets of scenarios all subscribed to that topic。 They're all triggered when the message comes in at basically the same time。

 They all talk to the same player but their independent web requests with independent histories。 that each questioner is tracking。 So it doesn't， they don't interfere with one another。

 And eventually when they're all done we've told them hey post adjacent object with your。 results to this URL。 Somebody else will handle it from there on。

 Which leads to the question of why don't we just send HTTP requests。 The reason is basically that they're synchronous。 When you send a request you wait for a response and we don't want to have to wait。

 Now you could say hey， well send a request， we'll have the thing immediately respond but。 then keep working。 But in fact if you're going to use Google Cloud Platform that isn't going to work。

 Because Cloud Platform is intended to run the code to respond to a request。 If you've got a function that is triggered by a web request once it sends the response。

 the platform decides that function's done and it stops giving it any CPU cycles so it。 can't keep running。 Pub/Sub is a synchronous。 When an invoker wants to send a message they give it to Pub/Sub and get it。

 Pub/Sub saying， okay I've got it。 They don't then wait around for Pub/Sub to actually deliver it and for the people that。 it's delivered to to actually execute what you're asking。

 So we're going to start by factoring out the questioners from this system。 And when we look at the overall picture what had been that one monolithic block for a judging。

 system is now several pieces。 We still have some unknown set of components that interact with the contestant。 That the contestant can use a web form to submit a URL and can go look at a web page。

 to see the results and standings。 But what the judging system does with that URL is hey I need somebody to judge this problem。 submission so all it does is publish some message to a topic and one or more questioners。

 subscribe to that topic and play the game。 When they're all done they post the response somewhere presumably that's another part of。 the judging system we're going to have to get to how that works sooner or later。

 The message body that we're going to post these to include the URL of the player。 In other words play a game against the player at this URL。

 It needs to include a URL for where to send the result to。 So when you're all done send the result here。 So we're going to need to keep track of which submission that questioner was testing。

 If you've got five people submitting solutions we don't want to get five results and not。 know which result with what solution。 So we're going to have a random ID that we create called a contest round every time somebody。

 submits a solution and that needs to go in the message as well。 And finally because they're going to send that result back in a URL we want to know that。

 when somebody gives something to us it's somebody we asked to give that to us。 In other words we don't want somebody else able to just spam the result URL with fake， results。

 So we send the message to somebody to a questioner telling that questioner to play a game we're。 going to include a secret just for that one message that we keep track of so when the。

 results come back our system can somehow make sure it's from the person we asked to have。 run it or actually the function we asked to have run it。

 We always want to look at the coupling that we're invoking when we add these greater components。 because every piece of coupling means that a changed one has to be reflected in what's。

 on the other side of that。 There's coupling between the questioner and the player that is the coupling we already。 talked about when we were looking at the player side。 It's web requests and responses。

 Very loose coupling。 For the bigger part of the judging system and the questioner components there's a little。 bit tighter coupling in that they're coupled by sharing a pub sub topic。

 The judging system has to be able to publish messages to that topic and the questioners。 need to be able to subscribe to that topic to receive the messages。

 Fairly loose not quite as loose as just sending web requests。 And finally results have to be sent somewhere。 That coupling is again going to be a web request。

 The questioner is going to send the result by making a web request a post object with。 a JSON object in its payload to the provided URL。 So it's time to build and deploy the solution。

 We have a hand-on code lab at serverlessworkshop。dev/questioner。 So let's go over to that code lab and take a look at it。 We begin with an introduction。

 You can see there's a lot more steps in there where for just the player。 We describe the game that's going to be played。 We've already gone over that。

 The input and the output， the example of how the questioner interacts with the player。 We talked about that in depth when we were building the player。

 And how the questioner is going to be told what to do。 It's going to receive a message with this JSON information。 The URL of the player function。

 Some URL to send the result to。 Unique ID for the contest round and a random unguessable secret for sending that result back。 When it plays the game it will see the result and when it's all done it's going to have。

 to send the result back to the URL it was asked for the result URL。 That when it sends it back it's going to have to identify which questionnaire it is。

 Because we may have asked three different questioners to play the game against a submission。 Those questioners all have different scenarios。 When the results come back we want to know which scenario we're getting the result for。

 So each questioner is going to pick a name for itself so that we can identify which is， which。 The questioner is also going to tell us what contest round they're reporting a result for。

 You're going to have to include that secret so we know that somebody isn't just spamming， us。 That is somebody that received our original message and knows what the secret is。

 And then they're going to have to tell us the result。 In this case one loss tied hung crashed whatever it happens to be。

 And we're interested in how many moves it took。 In this case how many guesses it took to get there。 So in this tutorial you're going to build a computer program that will act as a questioner。

 and it's going to set up the game state， ask the player to make a move， update the game， state。 ask the player to make another move and so forth until there's a result。

 Either the player is finally guess the correct answer or the player just stopped responding。 or we just get tired of asking the player。 Maybe we gave it a thousand guesses and it still hasn't guessed we're just not going。

 to keep asking it。 Whatever that limit is。 And that's up to the questioner how patient it's willing to be。 We're going to invoke this questioner by sending a message to a pub subtopic and it's going。

 to report the result via URL in that message。 So we need a web browser that we've been using up till now and basic knowledge of the Python。 programming language。 To get set up we need to go to the project we've already created in Google Cloud console。

 at console。cloud。google。com and we're going to need to select the project and we're going。 to have to open the cloud shell and fetch our code。

 We did all that when we were dealing with the player we don't have to repeat those steps。 if we've already done the player steps。 Now we're going to trigger this by a message sent to the pub subtopic。

 So before we can write a questioner we have to create the topic that's going to trigger， it。 In the real world the people managing the contest would probably create these topics。

 and then tell everybody to write the questioners。 But the order we're going through we're ready to quit a questioner we need a topic let's。 go ahead and create one。 We need to go to the pub sub section of the cloud console and we're going to select pub。

 sub from the big data section。 So let's go ahead and open a new tab so we don't have to keep going back and forth too。 much。 We already have a tab open for keeping track of the functions and keeping track of the。

 cloud shell。 In this tab we're going to look at the pub subtopics。 So we pull down the menu once the page finishes loading。 And whoops let me try that again。

 And we have to scroll down until we get to the pub sub section which happens to be near。 the bottom under big data。 Pub sub。 If it's the first time we've done it we may have to wait for the pub sub system to initialize。

 but eventually we'll be able to go ahead and create the topic we're going to need by clicking。 create topic。 Let's look again at the code lab to see how we should fill this in。

 We basically just need to give this topic a name we're going to call it play a game and。 tell it whether we're going to manage the keys or Google will manage it。

 The main reason you'd create your own key and provide it which is more work is if you。 want to keep control of that completely in your own company perhaps for security or compliance。

 reasons。 We don't have either of those here。 It's very secure to let Google contain the keys。 So we're going to say play a game。 Notice it creates a complete name that's globally unique for that topic that includes。

 the project it's in as well as the name of the topic and say create topic。 Alright it says a new topic has been created。 We can publish messages to that topic。

 We can trigger cloud functions from that topic etc。 All those things we need to do。 Let's take a look at the code lab to see what we're going to do next。

 We're going to create the first questioner。 So it's going to be creating a cloud function similar to what we did before but we're going。 to see there's going to be an important difference in what triggers it。

 Let's go back to our cloud functions section。 Go to the list of all cloud functions and create a new function。 We need to give it a name。 It doesn't matter other than for us keeping track of stuff。

 We'll call it easy questioner。 This is one that's going to ask it in our case guess a number from 1 to 10。 Nothing terribly hard。 But we're not going to trigger it by a web request。

 We're going to trigger it by a message from cloud pub sub。 If we do that we need to say which topic is going to trigger it and we've only created。

 the one topic。 So it's pretty easy to pick the one we want。 We're going to use the inline editor and we're going to write this in Python。

 And here let's expand this。 Here's an example。 We're going to have a function that is triggered by the platform that's going to send it the。 published event and the context for the event。 In our case we're really only going to care about the event which is a payload the body。

 of the message。 The context is metadata about the event which we do sometimes care about such as when it。 was sent， how long it's been waiting and so forth。

 Let's replace this code with the code we have in our editor under questioners。 Easy questioner。 Main。py。 So here we see the code we want。 Let's go select all the code。 There's a lot more to it。

 And copy it。 And go paste it here。 Replace what's there。 Whoops。 The paste didn't seem to include everything。 Let me try this again。 It chopped off the bottom part。

 Whoops。 Or did it。 Nope it just was part of my scrolling。 Nothing I like better than having two scroll bars that wants to keep track of where I happen， to be。

 Now the other thing we need to include is the requirements。text。 And here we see we're using three modules base64。json and the request module。

 Request is not typically standard in Python but it is standard in the functions framework。 so we're not going to have to include it。 Let's make sure of that by taking a look at the requirements。

text that was put here。 And we see again we don't have any special modules we can leave requirements。text blank。 Okay。 Now we have to say which function to execute。

 Which one is going to be triggered with the message。 And one we're going to trigger is question player。 You can look here and see how that works。

 The events are generally passed in base64 encoding in case they're binary or have other。 data so we generally are going to have to base64 decode it and then we can load that into。

 a dictionary because our message is Jason。 Then we look at get the fields we want out of it。 The player and you result URL。 The contest round in the secret。

 And then we call a function that will play the game and tell us the outcome of the moves。 And then we call a function to report the score。 Those are pretty straightforward。

 Let's create that function。 With luck we put everything in right and that's going to change to a green check mark。 While we're waiting for that let's go look at how it works。

 So again when a message comes in the cloud function framework will invoke the question。 player procedure with the event and the context。 The event has data that is our Jason object。

 It's encoded in base64 by convention so what we do is we take the data of the event， decode， it。 That gives us a byte string。 If we're going to do load it into Jason we've got to convert it into UTF-8 and load that。

 into our message， pull out the fields we need， play the game and report the score。 Okay let's take a look and see if it looks like everything's okay。

 That the function is going to be sitting there ready to run and again the trigger is via a。 topic not via a URL。 We want to test it。 So to test it instead of sending it a web request we're going to just go ahead and use。

 the testing function right now or in fact we're going to go ahead and publish a message。 to the topic。 So we're going to publish a Jason object that tells the URL to play the game against。

 where， to put the results to， the contest round and the secret。 So let's go ahead and put that stuff in properly under Pub/Sub， publish a message。

 Now before we go ahead and say publish it， it says there's no subscriptions but there。 is one it just hasn't been updated。 We do have to put the URL of the actual function to invoke。

 So let's go back to our functions。 Let's look at the list of functions and we're going to go ahead and play it against the。 okay player so let's get its URL。 Copy that address。 The result URL has not been written yet。

 We haven't built the judging system to send the results to so we've set up a simple little。 website that tracks everything that's posted to it and it displays the last 10 or 20 things。

 that have been posted。 So we're going to just have this Jason object posted there and we're going to go then be。 able to look at that website and see what it looked like。

 For the contest round I'm going to call this angle key， PyCon， just something that will help。 me recognize that the result is my result and then some secret。

 Since this isn't going to the real judging system we don't have to include a secret it's。 not going to actually be saved anywhere。 Let's publish it and see what happened。

 Well if we go back to the player function we can look at its logs was it just triggered。 Again these logs made lag by a minute or two。

![](img/d75a7af45610ca4b253c770090edc225_3.png)

 Right now it's 351 and sure enough it was triggered several times which makes sense because it's。 going to say make a guess and then based on that it's going to say okay here your previous。

 guess is make another guess repeated over and over again and it's been triggered many， times。 Let's go take a look at that ECHOER website that we posted our results to。



![](img/d75a7af45610ca4b253c770090edc225_5.png)

 And we see here that at 2251 UTC which is the time that we used for this it received an。 application JSON object that consists of something saying it's the easy questioner that。

 the contest round is for angle key PyCon a contest round identifier the secret and it。 one and seven moves。 So we're able to test things in fairly good isolation that way。

 Now let's create another questioner it's going to be pretty much the exact same questioner。 but with three different values instead of guessing from one to ten it's going to guess。

 from one to it looks like a billion and the target is going to be a million and you're。 going to have to guess it within a hundred guesses。

 Let's go to our functions we need to create a new one let's go back to the functions list。 create a new function called hard questionnaire， here again by a pub subtopic the same topic in Python。

 and let's look at our source code and let's scroll down and grab the source code for the。 hard questioner instead of the easy questioner， go back to our function replace the sample source code with this code and again let's。

 scroll down to make sure we got everything the only difference here is the range of guesses。 other than that it's the same。 Okay the function to invoke is going to be question player create it。

 and once that's ready we can go ahead and test both of them at once by sending one message。 to the topic it should trigger both of these functions let's give it a minute。

 it's basically the same topic as we had before as our function finished deploying not quite。 yet let's start setting up the other it is let's set up the topic and message publish。

 a new message and again we need to replace the player URL with the correct URL jumping。 around quite a bit here let's use the okay player let's see is there a shortcut to get。

 that trigger no so I'm going to drill into it and click the trigger tab put that in my。 topic give it another name so I can keep track of it and publish it let's go take a look。

 at the echo or see if anything new has appeared there yes the easy question has reported the。 result the hard questioner hasn't yet and the reason for that is that the hard questioner。

 will let them do a hundred steps and until the hundred stuff is up it won't give up so。 it'll take a little bit longer and there it is the hard questioner came out and it was。

 failed after a hundred moves it did not guess because it was always guessing the lowest possible。 number and it was going to take a million guesses to get there okay we have now deployed。

 two questioners triggered by the same thing we now can go take a look again at what we've。 completed so we're going to build we've built and deployed the solution now let's take a。

 recap we've built another event driven cloud function it was triggered asynchronously pub。 sub let us send one message that hit many questioners the questioners create the result。

 that need to be saved but they're not responsible for saving it they just send it somewhere somebody。 else's problem again reducing system coupling now dealing with all of that is the remaining。

 part of the judging system and we're going to deal with that in the next step the judging。 system part two after a break welcome back to the workshop for building a distributed。

 serverless application using google cloud platform the first part talked about the background。 for the application the second part of the workshop showed how to build the contestants。

 portion of the application that is a program that played a game that would be judged by the。 system as a whole the third part the previous part to this one actually built the part that。

 judged the submissions now we're going to do the final part which is the administrative。 center of the overall system the thing that manages contestants being able to submit their。

 solution showing standing asking the judging systems to actually do the judging and so forth。 remember you can do all of this on your own later if you want the slides and exercises。

 are all online at the serverless workshop dot dev website that side also includes my email。 address and an email address for sending in requests in general about this workshop well。

 the system so far that we've looked at consists of the contestants part of the system a laptop。 where they do their work at a web server where they deploy their solution and then the judging。

 system the part where the contestants amidst the URL of the website the judging system uses。 the questionnaire that was developed in the last part one or more questioners to actually。

 play the game they report their results back to the judging system which is still kind of。 a black box and contestants can at any time go to that website and view the results and。

 standings we're focusing on the remaining unknown part of the system that question mark box this。 is something we're going to call the manager going forward because it's the administrative。

 center of the whole thing the managers responsible for a few things first of all anybody in the。 contest or probably anybody anywhere can go to that website and they'll look at all the。

 current standings of all the submissions and how people are doing relative to one another。 it's also where the consultants will go and get a web form that they use to submit the。

 URL of their solutions so that they can then be judged and put into the standings the managers。 responsible for asking the questioners to judge the submissions they do that just by publishing。

 a message to a particular pub subtopic and all the different questioners that do the judging。 themselves subscribe to that topic so they're all triggered by that submission and finally。

 when those questioners have finished judging a submission they've got a result they've。 got to do something with it they're told here's a URL please send the results there and somebody。

 will be responsible for it well the manager is the thing that's going to be responsible。 for it so we have to handle receiving those results let's see can we partition this further。

 so far we've really focused heavily on doing divide and conquer so each component we built。 has been pretty small can we make this smaller by breaking it into different pieces that do。

 different things well if we look at it we see that two of these four responsibilities of。 the manager are building a website that interacts with people either showing a person the standings。

 or giving the person a form to submit a request to have a problem judged one of the parts。 accepts the results from questioners that is it needs to interact with software not with。

 people because it's the questioners the software programs that are sending these results in。 so we're going to break it down into those parts we're going to have a web application。

 a standard website that people interact with and we're going to have a web service something。 intended as an API that programs interact with that will accept the results from the。

 questioner software these two parts are only going to be connected by one shared piece of。 infrastructure and that's going to be a database the database it keeps track of all the submissions。

 and all the results because the web application needs to show the stuff from that database。 and it also needs to say there's a new submission in that database and the web service needs。

 to tell the result of every judging run so when we look at the system as a whole and。 see how the different parts of component are connected we see that we have a manager app。

 that's the website that uses a database in order to keep track of all the scores and what's。 been submitted we also see that there's a manager service that's responsible for updating that。

 database with the reported scores from the questioners the questioners themselves are。 invoked by subscribing to a pub subtopic the manager app is responsible for publishing。

 those requests as messages to that topic the questioners interact with the manager service。 simply by sending a web request over the internet and the questioners interact with the players。

 simply by sending a web request over the internet as well the manager service and the players。 themselves don't directly interact so these are the different components there are four。

 parts they're connected by either the internet or sharing a database or getting messages through。 a pub subtopic we're going to add one more piece to this that is optional we're going。

 to cover this at the very end but that is user authentication because our manager app can。 simply let anybody say please run my submission by the way my name is Jay and then that person。

 come back and say well please submit run my new submission but my name now is Mark and。 somebody else can come along and say run my submission but my name is Jay and overwrite。

 the other Jay submissions by having front end user authentication we can make sure that although。 somebody can come in and say whatever nickname they want they can't change it and they can't。

 impersonate somebody else that's a really nice feature called identity aware proxy that we。 can do with very minimal changes to the rest of our application so what have we got to deploy。

 well the two new components the manager application that is a website the manager service which。 is a web service then the database it connects those two pieces we also will at the very end。

 put the identity or proxy in front of the manager app so that requests only reach our website。 if they've already been authenticated and they're going to have a header added to them telling。

 who the authenticated user is so let's go ahead and build and deploy the solution we have a。 hands-on code lab that we're going to go through this and it's available at serverless， workshop。

dev/manager so let's take a look at that we got some background which we've now。 seen in two other code labs explaining the overall problem and describing what the managers。

 are responsible for doing it records the fact that somebody's made a submission sends a。 message to a pub subtopic so that those questioners all judge the submission and then accept the。

 results back from those questioners so it can update its database we're going to build。 the database and we're going to use Firestore which is a no SQL database that also scales。

 to zero just like our various computing solutions do so if nobody's interacting with the database。 you pay for storage but you don't pay for compute time a cloud function that's used for the web。

 service that's going to be triggered by the questioners sending the results into it and。 a web application and we're going to use App Engine for that App Engine is a serverless。

 platform it's actually the first serverless platform that Google Cloud provided and it's。 optimized for creating interactive websites which is exactly what we need and finally we're。

 going to put the identity where proxy in front of that optionally so that requests only reach。 that App Engine web app once they've been authenticated and those requests have the authenticated information。

 added as headers to the request so we're going to see how to do all of those things create。 the Firestore database once again we're going to deploy a cloud function just like we've。

 done in the previous two labs we're going to deploy an App Engine application and we're。 going to see how to restrict access to that application all you're going to need for this。

 is a modern web browser and basic knowledge of the Python programming language so to get。 set up we're going to need to create a project and open the console of that project we've。

 already done that as part of our previous two code labs we're also going to need to get。 the source code which we've already also received so let's go and take a look at our。

 console we see in our console that we have the source code available here the questioners。 the source code for the manager components and so forth and if we want we can go to the。

 terminal and we can run commands there with all of the necessary software already pre-installed。 the first thing we're going to do now is create a database a Firestore database cloud Firestore。

 is a no SQL database that we're going to use for our shared persistent data remember all。 of our computing components are stateless they can't remember any information from one。

 web request or one event to the next so if they want to remember anything they've got。 to store it externally and this is where they're going to store it。

 Firestore works on collections of documents now a document doesn't mean a paper a person。 reads a document in this case just means a structured chunk of data something like a Python。

 dictionary so we're going to have a collection of documents one for each submitted contestants。 request we're going to call those submissions rounds contest rounds the contestant comes。

 and says here's a new submission please judge it that will be a round in our collection。 the round is going to tell us who submitted it it's going to create a unique ID for that。

 round it's going to keep track of the URL that needs to be judged and then it's going to。 have a sub collection of runs every questionnaire that runs this judging round well actually does。

 a judging run is going to report back its scores and those scores are going to be in。 the runs collection each of those is going to say which questioner did this and what。

 the outcome was and how many moves it took。 Turns out that these things are very easy to work with and Python the API is quite simple。 we'll take a look at it when we examine the source code we're going to deploy。

 Now in order to use cloud firestore it's got to be enabled you can enable it with the G。 cloud command line interface or you can simply go into the cloud console let's go ahead and。

 do that go into the cloud console and we want to go to the cloud firestore section so I'm。 going to open a new instance of it in a new tab so I don't have to keep going back and。

 forward and it's going to take a minute for this instance of the console to open up and。 once it does I'm going to use the menu in this upper corner to go to the cloud firestore， section。

 Cloud Firestore is data well data storage so we have here firestore notice that there's。 also file store which is totally different it's got one entire letter difference。

 Now one thing about cloud firestore is it actually has two modes and that's because originally。 there was another known as kill database on Google cloud called data store。

 Now it's still available but it's provided simply by putting a different interface in。 front of firestore so you have to choose one or the other。

 We're going to choose firestore in native mode instead of having it behave like data。 store so select native mode。 This is a choice that we can't go back on we've got to pick it and if we want to use。

 something different we'd have to create a new project。 We now have to say where our database should be stored we can either store it in multi-region。

 with five nine's SLA or a single region with only four nine's SLA。 There's a very high free tier available for firestore it's unlikely we're going to exceed。

 it anyway so even though multi-region would cost more we're probably not going to incur。 any costs anyway so we're going to go ahead and choose the United States multi-region。

 and create that database。 It's going to take a few minutes to go ahead and build nothing will be in it so let's go。 on to the next steps。 We need to create a cloud function that's going to be our web service that will accept。

 results from the questioners and update the firestore database in order to include those， results。 So we're going to go into the cloud section section of the console and create an HTTP triggered。

 web service called manager。 So again let's go over to our console open another instance so we don't have to use the。 back arrow too much。 It takes a minute for this to come up depending on how fast your current internet connection。

 is。 Things are busy right now。 Once it finishes coming up we can go where we've been several times before go into the。 compute section and choose cloud functions which now has some questioners and some players。

 Let's go ahead and create another function。 This one is going to be the manager that receives results。 We're going to trigger it with a web request。 We're going to allow unauthenticated invocations because otherwise we would have to provide credentials。

 to every questioner so that they could send us results。 That might be nice and more secure but it also would be harder to manage and create coupling。

 We're going to have a different way that we keep unauthenticated users from talking to， us。 This is going to be Python again。 Let's make this bigger。

 Then let's look at source code for the manager function。 Scroll down， select it all。 Maybe it into a buffer and replace the current function code with that。

 Again we have the weird outcome that we have nested scroll bars so it's a little tricky。 to make sure everything got copied。 If we take a look at this this is a save result。

 It's set the web request。 It's going to get the JSON information for the result into a dictionary and pull out the。 five pieces of data that are set to us as a result。

 It's then going to look up the contest round that this result is for。 If it doesn't exist if there is no such round it will say a 404 not found。

 Then if it does exist it's going to check that the request includes the secret that。 the manager originally set up for this contest round's reports。

 If the secret doesn't match it's going to return forbidden。 That's how we're going to keep people from sending fake results to our system。

 When we send the message to the questioner we include a secret。 When the questioner reports results it's got to give us the same secret。

 Now notice in order to actually interact with the database the Firestore database it's。 very very simple。 We create a collection object for rounds that either may already exist or may have had to。

 create a new one and then we get the document for the current contest round value that was。 sent to us in the web request。 Now when we get that it may not exist。

 There's a method that tells us that。 But if it does exist we can convert that round to a dictionary and get any field out of it。 and we want to get the secret field to make sure it matches。

 If it does match then we're going to add to the sub collection runs。 So this particular document the contest round can have one or more collections here we have。

 a collection name run and we're going to add a dictionary with the outcome the number of。 moves and what questioner and acknowledge that that succeeded。

 In order to do this we're not only going to have to use the standard JSON module we're。 going to have to use the non-standard Google Cloud Firestore module。

 So that's going to have to be put in the requirements。txt file。 So let's go back to our source code take a look at the necessary requirements。txt and。

 we see we need to use a recent version of the Google Cloud Firestore library。 Okay。 the function that will actually be executed is save result and let's go ahead and create。

 that new function。 While we wait for that check mark to come in or come in as a red X if something goes wrong。 let's see if our Firestore database is finished being set up。 It is。

 We can create our own collections but we'll let the software do that。 So the function is going to be used by sending a post request with a JSON object。

 We looked at that when we actually built the questioners how we send that request and then。 as we showed it's going to extract the data and update the database。

 The Firestore is really easy to work with from Python。 We now want to test the function and we're going to test it by filling in a trigger with。

 a JSON object that represents a request to make a move。 So let's go ahead and copy that JSON。 Go back to our Cloud function which still hasn't quite finished initializing。

 Once it does we can go into the test tab and test it。 Alright。 Click on manager。 Go into the testing tab。 Replace the triggering event with the one from the code lab which says we're reporting。

 results for a contest round called one。 The secret that we were told to use to report it is not very。 The name of the questioner is the program that's actually doing the submission is easy， questioner。

 When it ran this submission one the game in ten moves。 So let's go ahead and test the function and we get a 404 not found because there is no。

 contest round in our database for contest round one。 We asked for that to be judged so we're not going to store any results for it to come， in。

 So let's fix that problem by actually having a contest round in the database for one。 So go to our data firestore say we want to start a collection。

 The collection is going to be called rounds。 The first document is going to be called one and we're going to put fields in it right。 now let's just put in a field for the secret。 Let's say the secret ish。

 I think that is the only field we need。 Let's take a look at the code lab。 Yep， save it。 And we now have a collection with one document for contest round one。

 It has one field in it called secret whose value is shh。 Let's test our function again。 Now we get a 403 forbidden and that's because the secret we're providing not vary doesn't。

 match the secret in the database which is shh。 S with 3H is an explanation point。 So let's go ahead and change that secret to match the one that supposedly would have。

 been given to it and test the function one more time。 And with luck there 201 means created。 It actually supposedly created something。 Let's see if that's telling the truth。

 Let's look at our one document。 Refresh this page because we want to see if there's a sub collection there。 It will often refresh itself but sometimes there's a lag。 So let's force it to refresh。

 And we see that for rounds there is one document called one that has a field called secret。 and his value is shh。 And it has a sub collection called runs。

 Runs has one document in it that was given a random ID by the system since we didn't provide， an ID。 And that random ID has a document with three values in it。

 The three values that we sent to it in our JSON file。 So the function does seem to be working the way we want it to work。

 And just to keep things clean let's go back into the runs collection and delete that one。 document that we could put in there manually。 It's going to also delete all of its child sub collections。

 Okay。 Now let's go ahead and create the web application using App Engine。 We're going to go ahead and look into the App Engine section and let's go ahead and just。

 open a new tab for the console。 And we'll go into App Engine。 And we're going to see that things are going to be a little bit different for this than everything。

 else we've done with the console。 And that's because we cannot create App Engine apps through the console。 We have to do that from the command line。 There are simply too many pieces to have a good console experience。

 And let's go in here to App Engine。 The first time we do that it may take a minute to initialize but it says okay welcome to App。 Engine。 There's an empty App Engine app but there's been nothing deployed to it yet。

 Let's see how to deal with that。 We need to go into the serverless game contest source code the manager App Engine subdirectory。 And let's take a look at what's in there。 Here App Engine we have our actual program main。

py and it is a flask web app。 You can read about flasks elsewhere but basically what it does is it lets you decorate different。 functions with the paths that should be given to it to handle。 So let me go on up here。

 I overscrolled past it。 Here we see if somebody makes a request to slash request round with a post。 It's going to go ahead and pull stuff out of the form and check who submitted it。

 Update the database if there's been a requested round。 Let me scroll down。 It'll then use Pub/Sub to send the message to all the questioners to actually play the， game。

 Even if we scroll up we see that if somebody makes a request to that same path request round。 but with a get it's going to just return the form。

 And if it makes a request to the root the slash page with a get it will go through the。 database build a data structure with all of the results from all the rounds and then it。

 will render a template filling in the data from those results。 So if the user will see what the current standings are。

 And then there are some helper functions for keeping track of the user nicknames。 Fairly straightforward。 There are several different Python modules that are nonstandard that need to be included。

 so it also has a requirements。txt file。 Even if we don't have any nonstandard modules app engine is going to require we have a。 requirements。txt file。 It can be empty but it has to exist。

 And the one big new thing in app engine is app。yaml。 A yaml file that says which of the many app engine run file times and other environment。

 selections you have to make。 In order to do Python 3。7 all we have to have in there is one line。 This is a runtime it's a runtime for Python 3。7。 We have a couple of templates that are used to build the web pages。

 We simply fill in the blank templates for the home page and the page with the form。 And we have some minimal testing。 Basically something that just checks it when we ask for the home page。

 We get a home page that has word standings in it。 Probably it would be nice to be more thorough with this。 Okay that's what the source code looks like。 In order to deploy it we're going to have to go ahead and use a command line。

 Gcloud command from our cloud shell。 Before we deploy it though there's one line we need to update in our source code and that。 is the line saying where the manager function that we just created is。

 That's because when this function sends a message to all the questioners to do their testing。 they're actual checking submitted runs。 It's got to tell them what URL to send the results back to。

 And that's going to be the URL of the function we just created。 So let's go ahead and change that one line in our source code for main。py。 Result。

url= and we need to put in our manager functions URL。 Well that's going to be over here in the functions tab。 The trigger is right there。

 We'll copy its address。 And put that in our source code。 Again it will be saved automatically but just because of how it I tend to do control S anyhow。

 Alright now that it's ready we can deploy the app and it's really really simple to do。 after all that preparation。 Go back to our console。 Go to the terminal。

 Change the correct directory。 And say Gcloud app deploy。 All of the necessary account credentials and project definition and everything that is。

 needed for this to work are built into the cloud shell environment。 Alright we're asked is this what you really want to deploy？

 This yaml and source file to this target project and it tells us the URL that it's going to。 end up with。 And yes let's go ahead and do that。 It uploads our files and then it's going to go ahead and update and create the new version。

 and update it。 That's going to take a few minutes so let's read ahead to what's going to happen next。 Well we've already looked over the code so we have a sense of how it works。

 The one thing that's a little bit tricky is what the nicknames are doing and we're going。 to talk about that really when we talk about identity or proxy but there's a little bit。

 of background here。 If we put identity or proxy in front of our app engine app then every request that gets。 sent to the app is going to be first filtered through the identity or proxy。

 And it's only going to let requests through if it's already authenticated the person making。 the request。 If it has authenticated that person it is going to include a header with information about。

 that person's Google email address and a unique Google ID code。 All that kind of stuff that we're going to use if we need to use it。

 So get nickname is going to look in the form。 If it doesn't have anything that's already been saved in the database for the nickname。 So get nickname when we see a user we check to see if we know that user。

 If we don't already know that user we'll get the nickname out of the form。 We'll save it otherwise we'll say hey we need to have a nickname。

 Again we're going to talk about exactly how that works when we get to identity or proxy。 We now should be able to go ahead and run this app。 Let's actually take a look。

 Have we finished the deployment？ Usually by now it would be ready。 While we're waiting again look ahead。 All we need to do is a Gcloud app browse and it will show us the URL and we can open。

 the page to see the URL。 Go ahead and give it a try。 Let's step ahead while we're waiting for it to deploy and take a look at how it uses identity。

 or proxy。 Okay。 In order to use identity or proxy we're going to have to enable it。 We'll go through those steps when we get finished deploying and testing it。

 But once it's deployed and we've said that we are going to allow anybody to use our app。 What it will do is when you go to the home page you're going to be forced to log in first。

 That login doesn't go to your app it goes to identity or proxy but from that point on。 every time you make a request that request is going to include some headers。 Let's scroll down here。

 The headers include the authenticated users email， a user ID code that's unique to that。 user and a digitally signed assertion that has that information as well if you want to。

 make sure that nobody could have somehow slipped you bad headers。 That's not really possible in the simple architecture we've created where we don't have any other。

 programs that can bypass the identity or proxy。 Once we use those information that information the app is going to remember the nickname。 The first time is shown it and it's going to do that by getting the user ID from the。

 header and saving it in a nickname collection。 That's all。 If a person says my name is Mary it's going to save okay user Mary exists for this Google。

 unique user ID。 That's all it needs to do。 And then get nickname later when we come along and say hey when we see a new page do we know。 who this user is already。 It's going to look up that authenticated user ID header and if it's there it's going to look。

 up the nickname in the database and it's going to ignore whatever nickname the user submits， again。 So the user is not going to be able to say hey I'm Mary in one submission and then say。

 I'm Susan in the next。 We're going to know that that's not going to work。 So have we finished the deployment？ Yes we have。 So Gcloud app browse comes says go to this URL so click it。



![](img/d75a7af45610ca4b253c770090edc225_7.png)

![](img/d75a7af45610ca4b253c770090edc225_8.png)

 And our application is run and we don't have any contest standings yet。 We can request a new contest round。 So I can say hey I'm John Doe and for the URL let me go to my functions tab and pick one。

 of my players and give it that URL。 Let's give it the okay player and see what its URL is that's trigger。 So let's go back to our request to contest round。 Once it's around and it's going to return us it's already put the fact that there's been。

 around requested。 I mean just gorgeous formatting here I know but who requested it and when。 And once those results have been submitted back they'll be in the database and we'll be。

 able to see that hey the easy questionnaire is finished running and our solution one。 Remember the hard questionnaire asks you to guess the number one million in the range。

 one to a billion and since our okay player just always guesses one more than the last。 time it's going to take a while before it fails。 Did a hundred moves。

 So we see the contest standings that's the overall system。 Let's now go in and turn on identity where proxy which will let us finish this。

 So we've got to go to the I am for identity and account management admin slash identity。 where proxy。 So let's again open a new instance of the console。

 And unfortunately we're going to see there are a few annoying steps that are necessary。 in order to keep people from selecting information unknowingly from other users。

 You're not going to be able to get Google to tell you a user's email address without。 the user knowing that accessing your app is going to make that occur。

 So we pull down the menu choice for I am an admin identity where proxy and the first。 time we do it we have to enable it and then it's going to tell us that we have to set up。

 a permission page a page that tells the user who's doing it。 So let's go to identity where proxy we have to configure a consent screen。

 And that simply is something that's going to be shown when the user logs in so the user。 knows who's asking for my credentials and what are they going to use it for。

 So this set the OAuth consent screen requires a few pieces of information。 If we were making this available to just one G Suite domain we could say internal but we're。

 going to make this external。 That's going to have to be reviewed before we can use it widely but we can use it on small。 scale at first。 The application name， programming contest， a logo that's optional。

 The support email for somebody who wants to ask any requests we've got to put in a known。 email or it will use an email created by our particular solution。

 What information we're going to ask a user for and by default we're going to ask a user。 for minimal information their email address and their user ID。

 We can ask for more information and it will tell the user that we're asking for that so。 the user will know。 What domains？ Well we're going to have to tell it just the domains for our application。

 So let's go to our App Engine app and we can see that its domain is serverless workshop， anglekey。uc。r etc。 Then we're going to be asked for the home page link。 Whoop。 Okay， did I press enter？

 Apparently yes。 The home page is going to be at HTTPS， that domain。 Privacy policy。 I'm going to just use the same link even though I haven't created one yet。

 And the terms of service are optional。 So let's save that。 Okay。 now that we've created the consent screen we can go back to the tab and refresh。

 it and it should let us go ahead and turn on IAP。 Okay， there we go。 We can restrict access to HTTPS resources or actually SSH。

 We're going to restrict web access to our App Engine app。 We're going to turn it on。 And if we then try and go to our website we're going to be asked to authenticate and then。

 we're not going to be allowed access because we haven't told it who if anybody can actually。 access it。 So let's go ahead and show the info panel。

 Select that line and add members who are allowed to access our application。 And we're going to add all authenticated users。 And we're going to give them the IAP permission to be a web app user to connect to our web。

 app。 That's going to allow public access。
![](img/d75a7af45610ca4b253c770090edc225_10.png)

 Our policy has been updated。 It may take a minute to fully replicate but let's go ahead and see if it's replicated yet。 by going back to our application and trying to request the home page again。

 We're being made to sign in。 And it hasn't propagated yet that I am allowed to sign in。 And this is going to take about five minutes。 But once it is signed in it will then pass through to our application and let me access。

 the app and it will know who I am。 Okay。 Come on。 One thing I may need to do is go into the debugger console which lets me empty my cache。 and do a hard reload。 And there we go。 It finally reloaded the page。

 I could have closed and reopened my browser to do this。 But what did that do？

 Let's go take a look at our database and refresh it。 And we should see a database of nicknames showing up。

 Well the name will show up after I ask for a run。 So I don't think we're going to have them yet。 Right。 But if I go ahead and ask for a run， let's request the same function as before。 Okay。

 If I now request a new contest round it's going to already know I'm Bill。 And even if I was able to change this which in a web browser if you know how you can override。

 the read on the attribute it will ignore my new change because of the way the code has。 been written。 So that keeps people from behaving badly。 While still remaining anonymous。

 And again our data if we look at it now should have a list of the nicknames that have accessed， it。 And that list is not going to have email addresses because we didn't need that and we don't want。

 to store any more personal information we absolutely need。 Instead it's going to just have that Google unique ID code。 So nicknames。

 We should see a single nickname in there for the ID code that Google created that tells。 us that ID is Bill。 Alright that completes this code lab。

 It lets us get through all of the major steps and build our solution。
![](img/d75a7af45610ca4b253c770090edc225_12.png)

 To a quick recap we created a distributed serverless system that has different portions。 that can be owned by different identity entities。 The player。

 the manager and questioners each different groups and it used quite a few serverless， tools。 And we have a lot of options as a service which is to say cloud functions。

 Platform is a service which is App Engine， reliable messaging， Pub/Sub and a no SQL database。 and Firestore。 And we optionally put user authentication as a service in front of it。

 Thank you for your attention and time。 I hope this has been useful。 Again。 all these resources are available for you at serverlessworkshop。dev and there you。

 can either get my individual email address or you can use this alias to send us any kind。 of questions or comments you have。 Appreciate your time once again and have a good virtual picon。

 Thank you。 and we'll see you next time。 and we'll see you next time。 [BLANK_AUDIO]。
![](img/d75a7af45610ca4b253c770090edc225_14.png)