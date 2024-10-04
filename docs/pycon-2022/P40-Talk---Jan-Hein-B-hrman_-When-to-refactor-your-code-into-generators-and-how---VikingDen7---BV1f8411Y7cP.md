# P40：Talk - Jan-Hein Bührman_ When to refactor your code into generators and how - VikingDen7 - BV1f8411Y7cP

 All right， let's start the third talk of the session。
![](img/33efdd09484518636e8cf628d585467b_1.png)

 We have Janhain Biermann here who is going to talk about when to refactor your code into。 generators and how。 Thank you。 Thank you。 All right， let's start。 Well， first of all。 it's great to be here。 I hope everybody is doing fine here at the venue。 but also perhaps the viewers at home。 So let's start with my presentation when to refactor your code into generators and how。

 All right， so the goals of this presentation are that I hope that after this presentation。 you will be able to recognize certain loop constructs as candidates for refactoring and。 know how to convert these constructs into more maintainable and Python code by refactoring。 them into elegant pipelines of generators。 And finally。

 you will be more acquainted with the standard library editor tools， module。 and the more editor's package。 But first， let's introduce myself。 My name is Janhain Biermann。 You see my email address and my Twitter handle over there。 As you can see in the email address。 I'm working for Ordina already more than 10 years now。 Time flies when you're having fun。

 I witnessed and a long time ago， more than 30 years ago， I witnessed the first baby steps。 of Python when I was working at the Dutch Institute for Mathematics and Computer Science。 And during my career， I was working as a C++ developer with IIT also management roles。 But I always kept an eye on Python and its developments and how it progressed。

 And while being a Java unit manager at my current company， I sparked the idea for starting。 up a dedicated Python practice， the Ordina Python years。 And I think it's quite special for a broad-spectrum IIT service provider to have a Python dedicated。 unit within a very combined software ethics， software development ethics， or software development。

 craftsmanship together with Python。 We like that combination。 And finally。 the Ordina Python years are a participating sponsor of the BSF。 And the last four years I'm working as a Python programmer as a happy Python coder。 All right。 so the topic we'll be addressing is， first of all， I'm going to show you some。

 code with certain kind of loops， which have a certain pattern。 These loops have similar codes。 but they vary in start， stop， and selection criteria。 And I'll give a short， I don't know。 address the topic of refactoring and how you can recognize。 certain patterns of loop constructs for refactoring candidates。

 Then I touch upon the topic of generators and I will introduce you to the earlier mentioned。 etituz module in the two-specage。 So let's start。 So this is， I'm going to tell you a story。 but it is a fictional story。 It's not a true story。 It's a story that we're working in a team and we have a product owner who is representing。

 the Fibonacci sequence fan club。 And he would like you to make a function returning a list of Fibonacci numbers which are less。 than a certain value。 So first of all， who knows what a Fibonacci number is？ I see many ends。 perhaps not 100%， but close to it。 So short， short introduction to Fibonacci numbers。 I would just read it out。 In mathematics， the Fibonacci numbers form a sequence。

 the Fibonacci sequence， in which， each number is the sum of the preceding two ones。 So that's the essence of the Fibonacci sequence。 You start with two numbers。 Usually it's zero and one。 The next value is simply the addition of the previous two。 So in this example， you see the first number is zero and one。 If you add them together， one。

 zero plus one makes one。 So that's the reason why you see another one in the sequence。 And then the next value is calculated by taking the two previous ones of the next value， which。 are one plus one and that makes two， et cetera， et cetera。 One plus two makes three。 Two plus three makes five。 And then you get this sequence。

 So that's the essence of the Fibonacci sequence。 So let's start coding that in Python。 If you Google around a little bit， perhaps you end up with the tutorial of Python itself。 where a variation of this function is actually written out。 Has another name。 It doesn't contain type annotations， but for the rest it's the same。

 Doesn't contain a doc string either。 But it works as follows。 You initialize your function。 you initialize the result value as an empty list， and you， start initializing the two。 what you could call the Fibonacci registers， A and B。 And。 you could view A as the current Fibonacci number and B as the next to come Fibonacci， number。

 And as long as your current Fibonacci number is less than the threshold that you have given。 as an argument， as a parameter to your function， then you enter the loop， you append the current。 Fibonacci number to the list， you calculate the two new registers。 And that is the new current Fibonacci number is the old value of the， is the old next Fibonacci。

 number。 So A becomes B。 And at the same time， that's nice from Python。 You can make this kind of funny， how you call it， two-pole assignment。 You can calculate a new next to come Fibonacci number by adding the old values of A and B。 together and that's what you store in the B variable。 And as long as the condition holds。

 you just simply repeat that。 And then finally at the end， you return the resulting list。 So looks great。 Seems to work。 Everything has been tested by the way。 So this will be working。 And the product owner is happy with the results and he wants more。 So he now asks us。 he also makes a function that returns the end Fibonacci number counting， from zero。

 So means that if you ask for Fibonacci number zero， then you get the first one。 If you ask for Fibonacci number one， you get the second one and so on and so on。 We programs like to index from zero one words。 So let's call that one。 Looks a bit the same but also a bit different。 It doesn't return a list。

 It just returns one Fibonacci value。 But again， you have these A and B registers。 Works the same。 You make a counter but you don't need the counter-valuable。 You only use it to iterate a certain time through this loop and you calculate the Fibonacci number。 and when you have calculated that many n times， then you can return the current Fibonacci number。

 All right， so product owner now really gets enthusiastic and he says， okay， well， can you。 now make also another function which provides the first n Fibonacci numbers。 So not Fibonacci numbers up to n but the first n and perhaps also a function that gives me。 the smallest Fibonacci number greater than or equal to a certain value。 All right。

 we start coding again。 So here's the first implementation。 Here we are returning a list again。 You use this counter， what you have seen from the construct before and you append the current。 Fibonacci value to the list， et cetera， et cetera and then you return the result and the last， one。 smallest Fib greater equal n。 It's a bit different。

 There you go into the loop and as long as the current Fibonacci number is less than n。 you simply continue until you， that condition does no longer hold and then you return the。 current value because that is then equal to n or greater than n。 So now we have four function and in my original presentation we have a five but makes the story。

 a bit boring perhaps and these line up nicely together in this matrix。 So here you have the condensed form of these four functions together and I don't know what。 you're thinking but I'm seeing a pattern。 A lot of people also are seeing some sort of pattern or whatever。 You see the pattern explained over here because for all those functions function starts with。

 a name except some parameter， a threshold or whatever or an index kind of value and sometimes。 you need to initialize a container， a list in our case， then you initialize these Fibonacci。 registers， the A and D B。 Then you go into some while loop or a for loop with certain ending。 conditions and then you are in the loop calculating the new values of the Fibonacci registers。

 And then at the end you return the current Fibonacci number or you're returning the container。 where you're putting all the numbers。 So that's the pattern and as， so there is a lot。 there is a bit of duplication in this， code right and you don't like duplicate code because you might run the risk that you for。 one of the four implementations you just missed it， missed it， did it wrong。

 So you want to try to factor out the common code which is present in these functions。 So how to get rid of the redundancy of this code and that's where this talk is about。 Seems to be quite difficult but in general if you want to get rid of redundancy you would。 refactor your code and extract that part。 So first the question， who knows what refactoring is？

 Yeah， many ends。 Great， great， great。 Yeah so refactoring is about that you restructure your code without changing the external behavior。 But you usually do this to improve the design， the structure and the implementation of the。 code but you make sure that the original functionality of the code remains the same。 And you do this for a reason because your code becomes more readable， less complex and。

 better maintainable。 Alright， you try to extract these common parts。 You can of course extract that single line， the A and B initialization， put that in a function。 or something like that， it's quite useless， right？

 You replace one line by another line so that doesn't seem to work。 You can replace that other line where you're calculating the new Fibonacci registers which。 is happening in the loop。 It doesn't look very sensible either。 So you're kind of stuck。 how can you refactor it？ Perhaps you should combine the functionality of all these functions into one super function。

 which is able to do all these different kind of calculations depending on the mode flag。 You could make an enum for example， designating the mode where the function is running in。 We could try it first with just combining the first two functions of this story and then。 not using an enum but just a boolean， designating the mode of the function。

 So I should have warned you， first you had to put on your paracensative sunglasses on。 because this code really hurts， is hurting the eye。 I hope you'll agree。 It starts already with the ugly type annotations。 I don't know。 everybody has seen the keynote of a book as long and the ugly type annotations。

 are also an indication of ugly codes。 In this case also it's a bit strange that this function is either returning an int or。 a list of integers。 So it doesn't make sense to explain。 this code is working by the way but it doesn't make， sense to explain this code。 This is not the way to go。 It's also a bit of a contrived example of course。

 but it's just to demonstrate that， the resulting code is not quite maintainable and this is not the way to go。 So again， how to reflect this？ Well， I think the key thing is here that you have to take a step back。 You have to more conceptualize the things that are happening in your code。 Perhaps fly on a higher altitude， so to say。 And if you do that and you start thinking about what is this code actually doing。

 actually， it's doing two things or one thing which is common for all the code and that is that。 a part of it is producing these Fibonacci numbers。 And the other part of all this respective function is doing something else。 It's dealing with ending a loop criteria or the way the values are collected in a container。

 or that you are working towards the final calculated Fibonacci number。 So that is the separation that you can mentally make in this code。 So when you see that you have some sort of， you have codes where you do things but you。 also have a data producing part in that code， then you might be thinking of this refactoring。

 pattern because the data producing part is the part that you can actually quite easily， isolate。 But how will you do this isolation？ Well， for that I need to introduce another concept and that's our generator functions。 Yeah， generator function。 When you call this function it creates an iterator， a generator iterator。 And if you code a function it's quite， it's just like you're coding a function except。

 in the simplest form it just doesn't contain any return statements。 It contains yield expressions。 And if a function contains a yield expression then it automatically becomes a function， a。 generator function。 And the yield expression is used to give back a value to the caller but not all values。 only one value at a time。 So you can use for example such a function in a for loop or some other thing that can。

 deal with iterators。 Well each time that the caller， the consuming party needs another value。 the generator， well， it starts with the generator giving its first value back， right？

 And then when it does so it saves its state and it passes control back to the calling code。 to the consuming code。 And once the consuming party needs another value then the generator is revived。 It starts with left off， it still has its value， all the local variables still have their own。 value and it can produce the next value and so on and so on until some end-incruent criteria。

 is met or something else or the consuming party doesn't need any more values so that's。 how it sort of works。 So then the next question is how can you code a generator function？

 How can you code a generator function producing these Fibonacci numbers？

 Well that's this lovely elegant piece of code I would say。 Let's call it the Fibgen。 a Fibonacci generator and this function generates an endless sequence。 of Fibonacci numbers until your memory in your computer cannot cope with the ultra large。 numbers that you're producing and you recognize some familiar parts in this code I guess because。

 here you are again， before you enter the loop you're initialize your Fibonacci registers。 A and B to the starting value 0 and 1 and then you enter an endless loop， well through。 and there you yield this first Fibonacci number and then the consuming party can do something。 with that number and then after that when the consumer party wants another element then。

 your function generator function continues so you start calculating the next values A。 and B with a familiar formula you go you loop again into the same loop and you yield the。 next value and it stops again so that's how it works and in my view what you see here。 is perhaps the most canonical code a representation of the Fibonacci sequence when you would code。

 it in an imperative style you could also express it in a more recursive style but if you express。 it in an imperative style it would look like this I would say。 The next question is how can you use this this generator？

 Well on the repo on the read evil print loop it would look like this for example if you。 want to print the first 8 Fibonacci numbers then you can write this loop what you do is。 you're using the zip function to zip together a counter a range of 10 it will give you the。 values 0 to 9 you don't need those values they are zipped and they end up in the score variable。

 and you call the Fibonacci generator and those values end up in the fib variable in this loop。 and then you simply print this fib variable and then this is how you can use this generator。 so you could say we're set we're ready and you have a long break but perhaps there's。 a little bit more to it because we can go one step further and that is by using some。

 beautiful building block that's provided by the Python standard library and the is it。 tools module uses provides a lot of these nice building blocks here it says the module。 standardized course set a course set of fast memory efficient tools that are useful by。 themselves or in combination together they form an iterator algebra I would say a pipeline。

 of iterators making it possible to construct specialized tools succinctly and efficiently。 in pure Python and there's another module which is called it's not a module it's a package。 you can fetch it from pi pi pi pi the more it does a package and the more it does package。 implements many of the documented recipes which you can read in the in the it tools module。

 and a lot of more a lot more convenient helper routines so those tools to the rescue in the。 sense that you can make things even more compact for example let's start with the explanation。 of the take while function that you see here and the take while takes a predicate function。 it's just a function returning a falsie or a true V value and the function will be caught。

 with the current value that is given back by the by the iterable and in our case the iterable。 is fibgen and if you you write a lambda expression you see that's as long as the current number。 that is given back by the Fibonacci generator is less than the the end parameter it returns。 to true V value in this case actually it's actually just true and then the value is passed。

 on by take while and as soon as the condition does not hold any longer than the the the。 generator is shut off so to say and your set and all these values that have been passed。 through also as an iterable from take while are collected in a list by using the list。 constructor so this first function that we have written can also be expressed in this。

 way using the eta tools same for the second function here we are going to use the islice。 function from the data tools module and there is in two forms to sim and the islice function。 is just like you would like to use slicing with lists right you you remember this syntax。 with the square brackets and the columns where you can provide a start a stop and a step。

 value well the islice is the equivalent of that but then for iterators except that you。 cannot provide negative values for either for one of these parameters so there are two。 there are two forms or you provide only the stop value the stop index index between quotes。 or you provide the start index the stop index and the step index but there must be no negative。

 and we have another two from the more eta tools package which is called one and the one。 function accepts an iterable and it's make sure that iterable only contains exactly one value。 not zero not more exactly one and if it contains this value then it extracts that from from。 the sequence and returns it as a value so with the combination of these functions you can。

 implement the second the second function that was requested by the product owner so well。 we can continue like that for example here we have the islice this is the first function。 here we use the islice function in the one index form just only the stop value this is。 the one with the first and fix and finally the smallest fit you remember we had the previous。

 function where we were using the take while and this one is using the drop while so this。 one also takes a predicate function and it will drop any value as long as the the condition。 holds and once the condition does not longer hold it will start passing through the values。 that it gets from the iterator and the first function from the more eta tools package just。

 selects the first value and returns that one so that's how how you can implement the。 the last the last requested function so this now we're moving towards the end of the presentation。 I would say I would encourage you to read through the documentation of the eta tools package。 the eta tools module and the more eta tools package it's a very valuable collection and。

 you can use it perhaps more frequent than you than you were thinking and if you have。 an idea but it all has to offer then it can make you code more expressive as well so that's。 my my plea and part of the goal of this presentation and just to to to teach you little bit for。 example if suppose you have a massive amount of data build data that you need to put into。

 a database and you're using some sort of ORM like SQL alchemy to put it in then sometimes。 it's it could be useful to to to put it in chunks yeah I'm still there in large chunks。 instead of all the data in one goal and then for example you can use the more eta tools。 chunks or the eye chunked function of this module so that's it so the recap is we are。

 now perhaps more able to recognize a certain pattern of certain loops constructs as candidates。 for reinfecting you know what generator functions are and how how you can use them to extract。 the data producing part out of your entangled codes and you have made acquaintance with。 the eta tools module and the more it's a two's it's a package here are some resources and。

 links I was inspired by Wikipedia for the definition of the Fibonacci number code refactoring。 the Python glossary contains generator function definition the documentation of eta tools。 and more eta tools and I would like to thank you you can check my you can check out my。 repository don't look into its history but it's a final result please on the main branch。

 and actually perhaps as an additional note I have used pi test BDD behavior driven development。 gurging language to test all the functions that are that you've seen actually what you。 the functions that you've seen in this listing are actually from the repo as a link linked。 in so to say and so the code that has been shown is working code and I've used the pi。

 test BDD to test both the pre refactoring and the post refactoring variants and even that。 weird combination function for its correct working and I understand that questions it's。 better to get them later on or perhaps in the in the early or so I would like to thank you。 for your attention and hope to speak you later。 [APPLAUSE]。



![](img/33efdd09484518636e8cf628d585467b_3.png)