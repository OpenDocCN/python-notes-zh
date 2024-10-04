# P62：Talk Pratyush Das - Python in High Energy Physics - 程序员百科书 - BV1rW4y1v7YG

 Hi， I am Pratush Das from the Institute of Engineering and Management in India。



![](img/a9a4e892500dae9f5e728b3864abe80c_1.png)

 Before I begin my talk， I would like to give a small disclaimer that I am a CS undergrad。

 and not a physicist。 So this talk is a completely personal account from my own experience of writing software。

 for high energy physics。 What is high energy physics？

 Perhaps he will be more familiar if I said particle physics。

 High energy physics explores the fundamental particles that the world is made of and their。

 interactions。 High energy physicists try to answer the big questions like what is the universe made。

 of， what are the forces that govern it。 Something that might be surprising to the audience is that high energy physics has some。

 of the largest scientific collaborations in the world。 For example。

 the CMS and the Atlas experiments that are located at CERN have around 3000 people。

 affiliated to them。 Just imagine 3000 people working on solving the same problem and collaborating with each。

 other。 Something most people probably do not know is that the first computers were built for。

 use in physics research。 When I say this， I am not really talking about the secret computers that were used for。

 breaking secret codes and used in the wars。 So John Vincent at Anasov and his then graduate student Clifford Berry created the first electronic。

 and non-programmable digital computing device that they call the Anasov Berry computer。

 They created this in 1937 and it was quite popular or considered the only computing device。

 still 1942。 This was then replaced by NEA which was inspired by it。

 NEA was initially developed for use in ballistics and it was developed by another physicist John。

 Mauchley and Jay Presspert。 This is something that the audience might be more familiar with。

 I would ask for a raise of hands if this was a light presentation but I assume a lot of。

 people in the audience have heard of Monte Carlo techniques。

 This was invented by Nicholas Metrobales' group at Los Alamos National Laboratory for Physics。

 problems。 Most computer science people would have heard of the one human architecture that forms the。

 basis of most computers。 It was never that because John von Neumann。

 someone who played a major role in physics， research。

 his internal memo about using the NEA for nuclear simulation。

 And then something that is more close to my heart in 1991， Tim Gjarnersley invented the。

 worldwide web at SART。 Before I go into how we use Python in high energy physics。

 I would like to talk about， what are the biggest computing challenges in high energy physics。

 High energy physics research generates a lot of data。 Elementary particles travel extremely fast。

 I mean， I guess we are taught in our physics and the chemistry courses in high school about。

 how fast an electron moves around the nucleus。 And in trying to imagine how many interactions there would be between different electrons。

 and each of these interactions need to be stored。 But it is not just the size。

 It is also the complexity of the data that needs to be stored。

 The representation of the particles and the trajectories and also the collisions and that。

 is not very easy。 When I said that we need to store this much data。

 we actually will not store all of the， data。 These experiments have a trigger system built in that rejects some of the data outright。

 But even then it is petabytes and petabytes of data。

 Here I have a screenshot of a CERN webpage and I will see the CERN data centered past。

 the 200 petabyte milestone and this was back in 2017。

 It has been three whole years since this has happened。

 But even this is not really the limit of the data that we need to store。

 This data storage is going to increase exponentially when the large electron collector is upgraded。

 to the high luminosity larger term collector or what is more commonly called the HLEAs。

 And this is going to happen in less than a decade。 So we really need to prepare for this。

 The high energy physics community has built something that I think is unique to the high。

 energy physics research field， the worldwide LHC computing grid。

 It is a network of computers that are distributed all over the world at research institutes and。

 universities where high energy physics research is conducted。

 This is used to distribute the data storage and computation。

 This solves some of the problems because the burden of storing the data or computation is。

 not on one particular research institute。 Then again。

 the WLCG is not enough to solve all of the high energy physics computing requirements。

 SI-06 or spec in 2006 is an industry standard benchmark with a wide variety of codes represented。

 HS-06 or H-spec-06 is the high energy physics variant of it。

 It uses only the C++ base codes from SI-06 that match high energy physics use cases。

 If you follow the plot on the slide， you will notice that there is almost a factor of five。

 increase for SI-06 whereas just a factor of two increase for HS-06。

 This shows that for high energy physics use cases especially， CPUs aren't getting much， faster。

 So what do we do if we can't use CPUs？ Let's use GPUs。

 A lot of the high energy physics computations are embarrassingly parallel which makes it ideal。

 for using GPUs。 So， in its extensive use of GPUs for high energy physics research also found a spot on the。

 NVIDIA website。 At the lower half of the slide， there is an excerpt from Adrian Ofteger's talk title。

 GPU resources for the CNF HPC cluster that shows that using GPUs could drastically improve。

 performance for high energy physics problems。 The awkward array library attempts to solve some of the problems that I discussed in the。

 earlier slides。 It was built with a need to perform computations on the complex high energy physics data that。

 I mentioned and was also recently redesigned to have different levels of abstraction that。

 allow multiple things like possibly having a GPU back end in the future and to allow。

 someone who wants to play with the source code to have access to the underlying C++ layer。

 And most important to this talk， it is meant to be used via its Python interface that is。

 located at the top of the design tree to the right of the slide。

 So what makes a language ideal for use in high energy physics？ It has to be easy。

 This is don't really want to spend a lot of time learning a new language。

 You have to remember they are not software engineering or computer science people and。

 are usually not interested in the code itself。 Python is easier to use and learn than most other languages。

 Ok， there are some purists who would probably say that Python is not necessarily easy if。

 you want to use it properly and not be really really slow。 But let's ignore that for now。

 The language has to be fast as mentioned in an earlier slide， high energy physics has。

 a lot of data and to perform computations on that data， you need the language to be fast。

 Python is not necessarily slow。 When used with libraries like NumPy and NumPy， it can be quite fast。

 The language has to be mainstream。 If a language is popular。

 it is easy to find resources to learn the language itself and， find support if required。

 And according to the screenshot of the TIOB language index， that is definitely true for， Python。

 So although Python was not the main language used in high energy physics， physicists started。

 using Python soon after it was developed。 In 1994。

 a handful of physicists like Jeff Templon started using Python。

 These were just small scripts for their own type of projects but not really used on a， large scale。

 After three years， one of the largest national laboratories in the US published a paper。

 about using Python as an extension language for the D0 experiment。

 They published the set CHUP which is the flagship conference for computing in high energy physics。

 So it was kind of a big deal。 After a year in 1998。

 Jeff Templon published a paper titled "Python as an Integration Language"。

 at the physics department of the University of Georgia。 In 2000。

 Python slowly started being adopted on a larger scale。

 There were two talks presented at CHUP about Python titled "Dynamic Graphical User Interfaces。

 Using XML and J Python from Formula" and "Ida LHC++ User Interface in Python from CERN"。 In 2001。

 Philip Kanal， who is one of the core developers of one of the most widely used。

 software libraries today， gave a talk summarizing the data analysis and visualization track at。

 CHUP where he emphasized the usage of Python in high energy physics。 By 2003。

 experiments had started putting Python in some of the analysis frameworks。

 And once it is in an analysis framework， most of the people in that experiment start using， it。

 So let's say by 2003， physicists had started seriously using Python for their analysis。 But even so。

 C++ was still the main language being used。 So this is a plot created by Jil Pivarski who finds the term "year of Python" as according。

 to this plot in 2019。 Python usage in high energy physics trumped the usage of C++ for the first time。

 And okay， so this plot should not be relied on solely because this is based on the GitHub。

 repositories of users who have parked a well-known high energy physics repository on GitHub where。

 CMS is done new。 But this does give us an indication of where things are headed。 In the past。

 there have been mainly two languages that have dominated in the field of high-energy， physics。

 There was Fortran up to early 1990s。 There was this whole ecosystem built in Fortran with power， H。

 Book and Zebra。 And then in the early 1990s， root was created， which was written in C++。

 And root has still been used to this day extensively。

 I'm going to talk about more about root in the next few slides。

 But let me complete by saying that I am optimistic that Python is the language of the future。

 So how do high-energy physics people work with data？ Every high-energy physics researcher uses root。

 So root is a more diverse scientific software toolkit。

 It provides all the functionalities needed to deal with big data processing， statistical， analysis。

 visualization and storage。 It is mainly written in C++。

 And when I say that it provides all of the functionalities， it really does provide all。

 of the functionalities。 It provides everything from plotting graphs to machine learning libraries and data streams。

 or studying following data， all in this one monolithic package for it to be。

 The root is probably the primary reason for the dominance of C++ in high-energy physics。

 So root was built in the mid-1990s by daily improvement funds and amplifiers。 It convincingly。

 quite convincingly， decreased power in the Fortran ecosystem in high-energy， physics。

 I don't think any people use power or H， Book or Zebra today or， I mean， even if they do。

 it is probably to maintain some sort of backward compatibility with data that was recorded back。

 in the 1980s。 So root is probably the file format or to be more accurate。

 the largest open-source file， format， storing the largest amount of data。

 Root is developed by a small team， directly led by Axel Norman and Sarn， who work quite。

 actively on maintaining and developing it。 I think to this day computing and high-energy physics is quite synonymous with root。

 It's hard to imagine both of them being separate。 I think you might have heard of the discovery of the Higgs boson at Sarn in 2012。

 If you look at the slide at the bottom， it is the official Atlas plot created by root。

 that was used to represent the existence of the Higgs boson。

 So root is quite important in high-energy physics research。

 But many people asked for a Python interface and I'm not surprised。

 So root has Python wrappers around its C++ code and they call it Python。

 But the root code base is actually quite large。 I think it senses of millions and millions of kinds of code。

 If you look at the contribution statistics to the right of the slide， you will notice。

 that each of these developers themselves have more than a million lines of code added to。

 the code base。 And this doesn't even count people who contributed before 2000。

 So how does one handle this？ You don't really expect people to add Python bindings around each class for millions of。

 kinds of code。 So root came up with something called CPDYY。

 So CPDYY creates dynamic bindings and this was initially developed by WinLab itself。

 And quite deeply integrated with it， even though it does exist as a separate line。

 So CPDYY is quite an interesting thing， which I think is quite unique to the high-energy。

 physics world。 You might be surprised to know that root actually just comprises of three main files for generating。

 the Python bindings。 It's root。py， CPDYYY。py and Pythonization。py。

 Root doesn't have separate files for each C++ to link it to Python。

 So this is quite convenient if you think about it。

 And although it is deeply integrated with the root， the authored name lever itself is。

 currently trying to develop it to be a standalone library。

 And I think that once it is fully developed， it might be quite revolutionary for fields。

 other than high-energy physics。 So there is another way to read and write root files。

 It is quite uproot。 So uproot is an alternate implementation of root IO in Python。

 Even though pyroot is improving day by day and the root developers are working on something。

 called experimental pyroot to solve some of these issues， but there are still some issues。

 that users would run into if they tried using pyroot code。

 There are object ownership issues between the C++ and Python root code。

 Pyroot is not really completely Python-y， like you would expect it to be。

 And it is kind of slow to deal with certain types of data。

 And here we are not really talking about Python slow。

 We are talking about slow as in slower than even naive Python。

 So that is why uproot was written to solve some of these issues。

 And since it is written completely in Python， it implicitly solves the first wishes out。

 of the path。 And although rewriting something in Python might be quite difficult。

 it is not impossible。 Uproot was actually created by a single full-time employee， Jim Pivarsky。

 and his student， me。 And also， of course， helped by many volunteers who still push comments and send full requests。

 to help in maintaining and developing it。 So， this is one of the most widely used Python packages in high-energy physics。

 although， it is relatively quite new。 Okay， when I said relatively。

 it started being written by Jim in late 2017。 But when you think about it。

 the other library that came before it or rather the library that。

 is still being used on the main library root was developed in mid-1990s。 So compared to that。

 uproot is really quite new。 And it is really just Python。

 If you look at the graph or rather the bar at the top， you will notice that all of it。

 is just Python and Jupyter notebooks。 The one when 5% see that is present is a developer tool that I wrote that is just to be used。

 for and by developers and users don't interact with it。 And if you look at the graph。

 you will realize that uproot is used equally up there with。

 the other industry tools for scientific computing like NumPy and SciPy。 So it is quite popular。

 And this graph was plotted from scientific Linux usage only。

 Scientific Linux was created in formula for use by this is。

 And we rejected the other operating systems because those contain some noise with data。

 from batch jobs and such。 So this is quite more accurate than other devices。

 So Python is not so slow。 Contrary to popular reading。

 But that is only when used with libraries like NumPy or NumPy or other libraries。

 I am sure there are a bunch of them out there that include performance or release the GIL。

 or something。 And okay， so in this big in this benchmark with the time and the speed up。

 It is performed on a code computing a fractal。 If you want to look at the code。

 there is the link to the notebook at the bottom。 If you look at this closely。

 you will notice that the vector is NumPy platforms almost。

 as fast as C++ root with vector is NumPy at 30 times the speed up 95th on the root at。

 32 times the speed up 95th on。 And if you use something like NumPy。

 then it is much faster than C++ root with the compiled。

 by NumPy number benchmark at 90 times speed up with the root just at 32 times。

 But this is to be taken with a pinch of salt because this again this is for just one piece。

 of code that we chose especially to show that Python doesn't mean that it is slow。

 And I am sure that for more general uses， they would be at par or C++ root would be faster。

 than uproot。 So this is a tool that was developed outside of pinaigee physics unlike a lot of the things。

 that I have been talking about。 And I think most of you might have heard of this already。

 Pyvine 11 is a very popular tool that is used in industry and even by hobbyists I would， say。

 If you look at the GitHub stars， there are 7，000 stars that reminds me I should probably go。

 and start it myself since I started using it。 And if the stars are 7，000。

 I am sure the actual number of people that are using it are， a bunch more。 By bunch。

 I mean exponentially more。 And so my point in this slide is not to talk about Pyvine 11 itself but how we can use。

 Pyvine 11 for high energy physics software。 So why would a high energy physics researcher use Pyvine 11？

 It is easy to do have existing C++ code in Pyvine 11 and it is not always easy to vectorize。

 all operations implemented in Python even if you are using a library like NumPy。

 Although we showed that Python does not necessarily mean false， it was for a particular process。

 like I said earlier。 In general， I guess naive C++ code will always be faster than naive Python code。

 There might be a few exceptions but yeah， C++ faster than Python。

 And I am going to again talk about awkward array， the library that I mentioned at the。

 start of this talk。 So awkward array was initially written solely in Python but it was recently implemented in。

 C++ and Pyvine 11 with a lot of success。 Even though it is still in development。

 a lot of physicists have already started using， it。

 So there are multiple reasons why the author decided to rewrite it in C++ and Pyvine 11。

 The original NumPy interface was hard to extend and maintain。

 And also the author conducted a survey where he asked physicists what they required and。

 they said that although they appreciate a NumPy interface and they like to use it but。

 they still sometimes would need an interface for imperative。

 There are other high-level physics libraries except awkward array that are using Pyvine， 11 already。

 For example， the first histogram library， PyFMC and Goofy。 This brings me to the Scikit-Her project。

 So the Scikit-Her project is meant to contain all Python tools that might be required for。

 research in high-energy physics。 There are a lot of active developers building tools in Python for using high-energy physics。

 that are housed under this project。 And since all of the tools are in Python。

 one can actually consider it to be a more， utilized tool set and physicists can choose the library that they need for their current。

 use case at that point of time。 And then once they're done with that and they need to do something else。

 they can just move， to another library。 And also some of the libraries under Scikit-Her are extensions to industry or develop libraries。

 And this means that it does not require a lot of reinventing the wheel for high-energy。

 physics uses。 And the Scikit-Her project also has an actively monitored heater channel。

 So if you want to reach out to people in this community， I have the name of the channel。

 in the slide。 So please go ahead and I'm sure you'll find a lot of people willing to talk to you。

 So this is a list of currently active packages under the Scikit-Her project。

 This slide was presented by Eduardo Rodriguez at CHF 2019。 If you look at the slide closely。

 you will see that there are tools for everything。 There are tools for tracking particles and decays。

 but histogramming， even processing， fitting， simulation， everything。

 The Scikit-Her project is growing with many new packages still coming up。

 This is not very surprising as it mirrors the Python growth plot that I had shown in。

 an earlier slide。 This is made more evident by the fact that if you look at the slide。

 you will notice that， some packages are tagged as new packages。 These were developed after CHF 2019。

 So they are quite new。 I think the recent advances in the fields of machine learning and deep learning have played。

 a major role in the shift from root and C++ to Python in the form of Pyroot and uproot。

 Root has its own machine learning library， TMBA， but in my personal opinion， I think it。

 cannot really measure up to industry standard libraries such as PyTorch and TensorFlow。

 But high energy physicists are open to change and invite people from industry to speak at。

 their conferences。 For example， so much in Tala， one of the creators of PyTorch was invited to speak at ACAD 2019。

 about how physicists can use PyTorch for their research。 So far。

 this has all been from the perspective of a computer science student， me。

 But let's see what high energy physicists have to say。

 This is a slide by Chris Bird that he presented at PiHab 2018。

 He says that 90% or more of the code that he writes will probably never be used again。

 The time it takes to write and execute his code is important to him。 Okay， C++ is fast。

 but it probably takes longer to write C++ code than Python code。 So in certain cases。

 the benefit of performance in C++ might actually be costing him time over。

 writing something in Python。 Python is designed to be readable。

 It is quite easy to read when compared to other languages like C++。

 His last point elaborates on what I have already discussed in some earlier slides。

 So we can see that there are compelling arguments that make some physicists want to switch over。

 to Python from C++。 So far， I have been talking about Python usage in high energy physics。

 but I would like to， briefly talk about Python usage in other physics fields。

 So I think the physics field having the most usage of Python is astronomy。

 AstroPy is a really well known library。 It is a little similar to scikit help。

 although I would say that it has more adoption than， scikit at least at this current stage。

 And if you look at the plot on the slide， you will notice that when software is mentioned。

 in astronomy publications， the tendency for it to be written in Python has exponentially。

 grown over the years and completely dominates over other languages used for writing software。

 in astronomy like ideal math lab or footram。 So in my mind。

 there are three main ways to push Python adoption in high energy physics。

 So this slide describes what I think should be the way to promote Python for libraries。

 that already exist。 So one could use Cpvy to create Python findings。

 And this is definitely the easiest way to do it because like I said， you just need a few。

 Python files to do all of the binding。 But this may not be ready for generalize to see。

 The more common option as in used outside of energy physics is using pi by 11。 But again。

 it's tedious to have entire code bases in pi by 11。 But then again。

 it's easier than rewriting the entire code base in Python。

 And since it is used outside of energy physics， you will find people that have faced the problems。

 that you face by trying to use pi by 11 to wrap all of your code。

 So help might be more easily available。 And the third option is rewriting it all in Python。

 And this might be a little more difficult than the other two options。

 But this can be done as demonstrated in up。 But then again。

 it's impossible to rewrite all of the C++ physics libraries in Python。

 For energy physics libraries that have not yet been written or are being planned to be。

 written in the next few days， this is what I recommend。 So one can use CPP， but again。

 it's not really ready for generalize to use it in my opinion。 And if you try using it。

 you might run into some unsarmontable problems。 The other option is using pi by 11。

 And using pi by 11 probably involves writing a lot of additional code in addition to your， C++。

 But then again， it is not really difficult to do this。

 And this is definitely the goal to strategy C++ if required。

 And the third option is rewriting the entire thing in Python。 And for a new library。

 this might be preferred because for a physicist who does not know C++。

 or Python or any language for that matter， using Python might be easier because it is。

 easier to learn than other languages。 But then again， if performance is critical。

 then it is recommended that you only write， it in Python if you are familiar with the ecosystem and libraries such as Numpy and Namba that would。

 help you get the performance that you desire。 To conclude。

 I would like to say that it is evident from my talk that Python is a popular， language。

 even in sciences like high energy physics where performance is critical。

 Python is quite readable compared to other languages like C++。

 And this is something that people whose primary interest is not learning any programming language。

 would desire。 And according to me， Python is most definitely the natural bridge to machine learning and。

 other statistical software within outside of high energy physics。

 Although there are some alternatives in other languages， the most popularly known and used。

 ones are definitely in Python。 Newcomers to high energy physics。

 you probably know about industry standard tools like pandas。

 and tensorflow as compared to the root alternatives like RTF， FAM， and TMBA。 So in my firm opinion。

 I believe that Python is here to stay in the high energy physics。

 community for the foreseeable future。 So several people have helped me with my talk。

 I would really like to acknowledge that help。 Jim Pivarsky， Jeff Tamblon， Henry Steiner。

 Eduardo Rodriguez and many others were motivated， to me and helped me write my slides。

 So here are some links， mostly talks that you might want to look at if you want to know。

 more about how we use Python for high energy physics research。 On this slide。

 I have the links to the projects that I mentioned in my slides。

 If you want to go ahead and look at the source code and maybe even contribute to some of them。

 That brings me to the end of my talk。 If you want to contact me， you might do so at ricktas@gmail。

com。 And if you want to view my work， you can do that at GitHub under the name ricktas。

 Thank you for listening to my talk。 [BLANK_AUDIO]。



![](img/a9a4e892500dae9f5e728b3864abe80c_3.png)