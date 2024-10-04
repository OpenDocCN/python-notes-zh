# P4：TALK _ Emery Berger _ Scalene_ A high-performance, high-precision CPU+GPU+memory - VikingDen7 - BV19Q4y197HM

 Hi， I'm Emery Berger， a professor of computer science at the University of Massachusetts。
![](img/a9915b6f966cee2d9257e21938089fd0_1.png)

 Today， I'll be talking about a new profiler for Python called scaling。 As you know。 when you have a performance problem or you just want your code to run， faster。

 you reach for a profiler to help you identify problems and hopefully fix them。 Listen here is a selection of some of the most popular profilers， including the ones。

 that come with Python by default。 In fact， there's such a proliferation of profilers that one of the profilers is actually called。 Yappy， which stands for yet another Python profiler。 So today， I'll be talking about Scalian。

 which is another profiler， but one that's， quite different from the others。 So it's not really yet another profiler， as I hope you'll see。

 So one thing that you want out of a profiler is not too much runtime overhead。 If you're profiling a program， it's because it's already too slow， so you don't want。

 to get that much slower。 So let's see how all of these profilers stack up in terms of performance。 To measure this， we're going to run them on a benchmark from the Pi Performance suite。

 On the Y-axis of the graph， we have normalized execution time， which means we took the time。 running the profiler and divided it by how long it took to run the program without profiling。

 That means that the best case would be 1。0x， meaning no overhand。 So some of the profilers do quite well with minimal overhead， 1。0 being as low as you， can get。

 and these three profilers go up to no more than 1。5x， which is pretty good， which。 is why they're marked in green。 Unfortunately， some of the other profilers are considerably slower。

 These are shown in yellow for caution。 This includes the built-in C profile。 And here。 the slowdowns range from 2x to almost 7x。 This isn't great， obviously。

 You certainly wouldn't want to run in production with profiling on， and it's inconvenient that。 things are slowed down this much， but maybe it's not a deal breaker。 On the other hand。

 some other profilers impose pretty drastic overheads， up to almost 40x， as you can see。 To make this concrete and really feel the pain， imagine profiling a program that takes。

 just one minute to run， but now it takes 40 minutes。 For most people。 this would be an unacceptably high overhead， which is why， in the graph。

 they're colored red for stop。 But it gets even worse。 so there is a profiler for memory called memory profiler， and memory。

 profiler imposes overheads of almost 300x， which is pretty extreme， really an extraordinary。 amount of slowdown。 Now， obviously， we should put kind of a big asterisk here by memory profiler。

 because after， all， all these other profilers that I've shown before only profile CPU time。 Well。 memory profiler profiles memory， so maybe it makes sense for it to be more costly， and。

 really a lot more costly。 But as you'll see， this is not necessarily the case。 It is possible to profile memory vastly more efficiently。

 So to make the rest of this discussion easier， we're going to move from a graph to a table。 and then we'll rotate it， so it's a little bit easier to read。

 And so now you may be asking yourself where Scalene fits in on this graph。 And the answer is Scalene is pretty efficient。 For this benchmark。

 it's just 20% slower than the original program。 There are options that let Scalene impose even less overhead。 but this talk is mostly， about the full Scalene with its default options。

 and the full Scalene profiles CPU， a Profiler， memory， and a Profiler GPU。 and we'll talk about that more about that in detail soon。 Now。

 profilers typically fall into one of two categories。 They profile either by functions or by lines。 So what this means is if you have a function-level profile， yes， it still shows line numbers。

 but only the line numbers that correspond to the start of the function。 Otherwise。 the rest of the information is aggregated over the entire function， which。

 is fine if you have lots of little functions that each don't do very much。 And then the profiler says， hey， this function is taking a lot of time。

 Maybe you should make it run faster。 However， it's really not so great when you have long functions。 Really， once you get over a certain length， a function-level profiler is not super helpful。

 because it says， hey， this function is slow， but you might want more granular assistance。 which is where line-level profilers come in。 So a line-level profiler， of course。

 reports information for every line of code， which， is great when you're profiling large functions。 like I mentioned earlier， or code that interacts， with libraries like NumPy。

 where each line is potentially doing really a lot of work， or。 in cases where function calls are just obscured because of Python overloading。 On the other hand。

 when you're profiling a large program， this might be far too fine， of granularity， right？

 Profiling every single line in a large program could really miss the forest for the trees。 So you might be asking yourself， why not both？ And indeed， that's the approach that Scameline takes。

 Scameline simultaneously performs function-level and line-level profiling。 So that's great。 There's another characteristic of some profilers， which is you have to change your code to make。

 the profiler be able to actually profile it。 Now it's not an extraordinarily huge change。 You just put at profile decorators on functions that you want to profile。

 But this is kind of putting the card before the horse。 It assumes that you already know where the problems are， which is not always the case。

 Also it's a pain to go in and change the program before running the profiler。 So Scameline。 like many of the other profilers， works on unmodified code， reporting performance。

 information for the whole program。 But it also supports the at-profile decorators。 So when you use these in Scameline， it tells Scameline to focus just on specific functions。

 which works best once you already know where the problem spots are， which you can find。 out without modifying any code。 Now a number of profilers have surprisingly limited support for Python threads。

 So you can see that there's a number of profilers， nearly half of them， that actually don't work。 when you're running multi-threaded code。 And what I mean by that is either they literally don't work。

 that is they just fail to operate， entirely， or they misreport the performance。 So if you have code that's running in a thread， its runtime might be completely ignored。

 To our surprise， it turns out that no existing profiler correctly profiles code that uses。 the multi-processing library。 So Scameline turns out to be the only option if you want to profile such an application。

 But these features are not the biggest advantages of Scameline overpass profilers。 No pass profiler can do any of the things listed here on this slide， except for one， which。

 I'll mention in a second， just to read the information across the top。 Scameline can separate out Python time versus C or native time。

 It can identify how much time is spent in system。 System calls meaning IO。 It profiles memory。 It profiles a GPU if you have one。 It illustrates memory trends and reports copy volume。

 which I'll explain in a minute， and， it automatically detects memory leaks。 Now like I said。 there is one profiler that does one of these things。 Naturally。

 memory profiler does memory profiling。 Remember， for this benchmark。 memory profiler was almost 300 times slower， while Scameline。

 ran the same benchmark and produced a memory profile with only 20% overhead。 All right。 so in the rest of this talk， I'm going to talk about how Scameline is able to。

 profile all of these other things， which gives you vastly more information about your。 Python programs and makes Scameline way better at helping you identify and fix performance。

 problems。 So Scameline is straightforward to use。 You've just replaced Python 3 with Scameline。 There are lots of options that let you tailor how Scameline profiles your code。

 #help provides a list， as is normal。 I will walk through a few really useful options that you would want to know when you're using。 Scameline。 So one particularly useful option is reduced profile， which only produces profiles from。

 lines of code that run for at least 1% of overall execution time， or which allocates some。 reasonable amount of memory。 So I'm not going to show it。

 but we're going to be using this option for all the next examples。 By default。 Scameline prints its output to the console， but you can also tell Scameline， to output to a file。

 And if you want， you can tell it to use -html， which will lead it to produce a webpage containing。 its results。 And here is an example profile。 We'll dig into this in more detail in a minute。

 but the top part of the profile is the line， level information with lines emitted when they have a little contribution to runtime or。 memory consumption。 And at the bottom， you get the function level profile。

 which Scameline reports for every， single profile all the aggregated information。 It also provides a breakdown in descending order of the lines responsible for the most。

 memory consumption。 So you can look for things that might be consuming an ordented amount of memory at a glance。 Finally， briefly， Scameline also allows you to disable memory profiling and some other。

 aspects of profiling that I'll talk about later by saying CPU only。 This is a bit of a misnomer because if you have a GPU， Scameline will still do GPU profiling。

 but both of these are extremely efficient and effectively impose zero overhead。 So when you go ahead and you run Scameline with CPU only， you can see that there's far。

 fewer columns of information that are reported and then you just focus on CPU time or potentially。 GPU time。 Okay， so let's walk through a simple example of how Scameline's holistic approach to profiling。

 can root out performance problems。 So here is some code that uses NumPy。 And we'll focus in on this。 so don't worry about it right now。 But here's what a C profile profile looks like。

 So this is a function level profile and it may be hard to see， but what it tells us is。 that main consumes all the runtime， which is not super helpful。 By contrast。

 here's a profile from line profiler。 And this of course tells us how much time is being spent on each of the lines of main。 but it's not particularly actionable。 It doesn't really give us much of a clue as to what's going on or why there would be a。

 slowdown or a particular line of code。 So here is Scameline's profile。 I'm omitting the function level profile and memory consumption summary for now。

 So rather than just saying how much time was spent in each line of code， Scameline breaks。 down execution time into time spent running Python code， time spent running native code。

 which means C libraries like NumPy and system time， which includes time spent doing IL。 Usually as a Python programmer， if you're trying to make a program much more efficient， your。

 goal is to move execution time out of the Python interpreter and into native libraries。 Right now。 it's clear that the code is actually doing just that。

 That is most of the time is being spent running native code。 So if that was all the information you had， you might reasonably conclude that there's。

 really no optimization opportunity here， it's all running in native。 This is as good as you could hope for。 But Scameline provides more information。

 So in addition to CPU time， as has been mentioned， it produces memory profiles like memory profiler。 But it breaks down how much of the memory consumption was from Python and how much was， native。

 And where it's blank， it means that all of the memory consumed was in native code。 And so here you can see there's a lot of memory consumption happening in native code， in line。

 six in particular。 In addition to profiling overall memory consumption。 Scameline profiles memory usage trends over， time。 So it illustrates these things called sparklines。

 And a sparkline you can think of as an inline graph。 So the x-axis is time and the y-axis is memory consumption。

 Each line has its own memory trend sparkline。 Finally。 Scameline reports a novel metric that we call copy volume。

 And this is meant to capture several performance problems。 So copying can be quite expensive and it's often indicative of a problem like accidentally。

 converting between native and Python data structures。 And as I mentioned before。 Scameline also reports GPU time， but we didn't run this on。

 a GPU so that would all be -- there would be no reports。 And in this case， regardless。 NumPy doesn't use the GPU。 So it's immaterial。 All right。 So let's dig in。

 Let's look at this line six， which seems to be where all the action is happening。 This one line is responsible for 34% of execution time。 It's all a native code， which， like I said。

 does not necessarily make it a good candidate， for optimization。 But the native code is also allocating a lot of memory。 Again， that maybe seems reasonable。

 It's really a lot。 It's 87% of all memory activity。 But the memory trend is interesting。 It exhibits this sawtooth pattern。 It goes up and it goes down。

 And this indicates that it allocated a big chunk of memory， allocated what looks at a。 glance to be the same amount of memory， which it is， and then freed it。 So this is a big clue。

 And this is confirmed by the copy volume number。 The copy volume indicates that there's a ton of copying happening。 So what's going on？ So if we look at this code， np。

array is a NumPy library call that converts its argument into， a NumPy array。 which really means a native C or Fortran array。 But in this case， the argument。

 which is the return value of np。random。uniform， is already， a NumPy array。 And unfortunately。 by default， np。array makes a copy of its input。 And in this case， that copy is entirely unnecessary。

 So we can easily speed up this program quite drastically。 So what we're going to do is we're going to change this one line of code slightly。 On the bottom。

 we have the optimized version。 And if you look， you can see that the only difference between these two versions is that。 we got rid of the redundant np。array call。 We can immediately see that this has the desired result。

 CPU time has dropped because we're no longer copying as much。 But more importantly。 the sawtooth pattern in the memory trend is now gone and the copy， volume has gone to zero。

 So our modification worked。 We can， of course， verify by testing that we get the same results。 There's one change that scaling effectively directly led us to dropped peak memory usage， from 1。

6 gigabytes to about 900 megs。 And not only did it reduce maximum memory consumption， almost by half。 but it also cut， 15% off total execution time。 So not bad for a day's work。

 So in the rest of the talk， I'm just going to talk about some technical challenges on。 how scaling achieves its precision and unprecedented level of detail while maintaining low overhead。

 And one of the most pernicious challenges is how Python handles signals。 Scaling relies on signals to track many aspects of performance。 Whenever a timer goes off。

 Scaling looks to see what line of code is currently executing。 and it also measures the load on the GPU。 And over time。

 this very accurately predicts which lines of code are consuming the most， time。 Now。 the way that delivery works in Python is that if you're running Python code， the delivery。

 of signals happens promptly。 So if you say I want a timer interrupt to go off every 0。01 seconds。 it pretty much will， happen every 0。01 seconds when you're running Python bytecodes most of the time。

 Because Python delivers signals right after it executes one bytecode in its interpreter。 However。 if you are running native code， so if one of these bytecodes actually is a call。

 to a native function， like a NumPy call， for example， the whole execution time will have。 no signals delivered until Python regains control。 So in effect。

 it's as if from the perspective of a sampling profiler that no time elapsed， whatsoever。 If Scaling just relied on timer signals alone， it would misreport where programs are spending。

 their time。 This would really degrade the quality of information from the perspective of a Scaling user。 To combat this， Scaling uses an algorithm that infers how much time was spent in native， code。

 It does this by checking the current time and here we're looking at the so-called virtual。 clock that is to say the time spent when the process is actually scheduled for execution。

 And it checks the current time according to that clock。 Then when it gets another signal。 it checks again to see what the time was。 And it turns out that you can then actually accurately say how much time was spent in Python。

 and how much time was spent in native code。 Intuitively。 this makes sense if the program was delayed beyond the expected interval。 So the interval was say 0。

01 seconds and it took 10 seconds。 We kind of know where those 10 seconds minus 0。01 seconds went。 They went to C code。 And so there's a little formula here that we can show leads to an accurate prediction of。

 the amount of time spent in Python code and its C code。 And I'll show you this in just a second。 In addition， we can rely on the same insight to separate out system time from total execution， time。

 So here， Scaling records the actual wall clock time， which includes any time spent on。 the CPU or sleeping because of I/O。 And this lets Scaling compute how much time was spent in system time for each line of。

 code。 So here I have an example that demonstrates Scaling's ability to tease apart native code。 execution time and Python time。 This particular program is designed to spend 0。

7 seconds in a native extension that we wrote， specifically for this purpose and 0。3 seconds inside of Python。 And Scaling accurately determines that the Python code is consuming roughly 30% of the。

 execution time and the C code roughly 70% of the time。 This despite the fact that the sampling is not being delivered， right， the timer signals。

 are not being delivered during the entire time the C code is running。 It's quite accurate。 The error here is just 1%。 So we're going to look at two more features of Scaling quickly。

 One is GPU profiling。 Here is an image of Scaling running in a Jupyter Notebook on a system that has GPUs。 So Scaling does not yet do full profiling on Jupyter Notebooks， but we're working on， it。

 It does CPU profiling and GPU profiling， but not the memory profiling。 So to profile a program with Scaling， as usual you pip install it， and then in a Jupyter Notebook。

 you load it with %loadxt， and then you finally run it with %scrun。 And here's what a profile looks like。 If Scaling detects that there is an NVIDIA GPU on the system。

 it will automatically do， GPU profiling。 And you can see the results here。 This is on a program that uses PyTorch， which does take advantage of available GPUs。

 if you tell it to。 And you can see in addition to profiling Python， native， and system time。 Scaling is now showing， in the final column right before the source code。

 GPU time also is a percent。 And Scaling implements GPU profiling using the timer-driven sampling just as with the CPU。 time。 It measures the load on the GPU and attributes it to the currently running line of code。

 And over time， again， this delivers a precise and accurate reporting of the time spent。 So now let's talk finally about how Scaling performs automatic memory leaks detection。

 And at a high level， imagine that for every line， every time a line of Python code allocates。 some memory， we say that it got heads on a coin flip。 And every time that that memory is freed。

 we call it tails。 So if that line of code is not leaking， every single time it allocates memory。 eventually， there's going to be a matching free when it reclaims that memory。 And so over time。

 you'll get a bunch of heads and a bunch of tails。 And if they balance out， then we can conclude。 quite reasonably， that there is no memory leak。 If however。

 these allocations don't get paired up with any freeze， and we just get allocations。 allocations allocations， you can conclude， quite safely， that you do have a memory leak on。

 your hands。 So tracking this for every single allocation performed by Python or a native library。 and， then associating it with every line of code， and then checking every free to see which things。

 were allocated would be very， very costly。 So instead， as usual， Scaling does this via sampling。 So Scaling randomly chooses allocations to track。 It records the address of the allocated object。

 And then， on every free， it does a quick comparison to see if that same object was just， freed。 If。 over and over again， allocated objects are not freed， then we can conclude that the。

 line is leaking memory。 So conceptually， Scaling assumes that all lines of code start with an equal number of。 allocations and freeze， right？ And one heads and one tails， meaning no leaks。

 It then gradually builds up information as the program is running， sampling allocations。 and recording whether they were freed。 And over time。

 we will see a pattern that will start to emerge。 And after we have enough data。 we can conclude when the allocations are roughly balanced， there's， no leak。

 and when they're quite unbalanced， it indicates a leak。 And when Scaling concludes with high probability that there is a leak， it reports the probability。

 in the volume in megabytes per second， which indicates how serious the leak is， which lets。 you as the programmer focus your effort on the leaks that really matter。 So in conclusion。

 I hope I've convinced you that Scaling is not yet another profiler。 It's a very different profiler。 It's precise， providing unprecedented levels of detail for CPU time while simultaneously。

 profiling memory， GPU utilization， copy volume， and identifying leaks。 It's reasonably fast。 and it's accurate， correctly attributing time for code that uses threads or， multiprocessing。

 And finally， it's easy to install。 It works on Linux， Mac， and Jupyter Notebooks。 We look forward to feedback and success stories， of course， on using Scaling to identify and。

 fix performance problems in your code。 And thank you for your attention。 [BLANK_AUDIO]。 [BLANK_AUDIO]， [BLANK_AUDIO]， [BLANK_AUDIO]， [BLANK_AUDIO]， [BLANK_AUDIO]， [BLANK_AUDIO]。



![](img/a9915b6f966cee2d9257e21938089fd0_3.png)