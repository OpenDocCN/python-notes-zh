# PyCon US 2022 - P78：Talk - Tetsuya Jesse Hirata_ Productionize Research Oriented Code By Python - VikingDen7 - BV1f8411Y7cP

 Hello everyone， welcome back。

![](img/8ace30fbec6033087b978b46f1139731_1.png)

![](img/8ace30fbec6033087b978b46f1139731_2.png)

 Let's welcome Jess to deliver a talk on productionized research-oriented code by Python or to。

 you Jess。 Hello everyone。 I'm Jesse。 I have been in education and technology industry for several years。

 In this industry， I have a talk of the issues between data science and engineering in AI/ML。

 projects。 So， based on my experiences， I'm going to talk about the process to productionize research-oriented。

 code by Python。 So let me share background at first。

 Python engineers assigned to AI/ML projects and the frequently faced research-oriented， code。 So。

 understanding the process to productionize research-oriented code can help make AI/ML。

 projects work more smoothly。 The target audience might be Python engineers who is involved with R&D。

 data scientists， first owners in AI/ML projects or data engineers。 So， three years ago。

 I made a five-minute lesson we talked about this topic in Python， US 2019。 So。

 this was my first talk on the stage in my life。 So， during the three years。

 I have made the talks about Python and data science topics。

 from the world and published a book about them。 Then， I came back to Python US today。 So。

 this talk is aggregated based on my three years' experiences of the project with data， scientists。

 what I wrote in the book and the past talks in Python。 So。

 I abstracted the four steps to transform research-oriented code into products。 The code。

 modularize the code， refactors them and then makes them product。 So， that's it。 It's very simple。

 I will explain each step one by one， much deeper。 So。

 the core samples in the four steps are written based on item response theory， also。

 known as a two-parameter logistic regression， which is a classic statistical model， mainly。

 using educational psychology。 This model is normally used for predicting how many students can get correct answers。

 based on the features of questions。 So， this talk， I use the code based on this statistical model。

 but the model is not the， main topic in this talk。 So， I will not talk about the model itself。 So。

 let's look at the first step。 The first step is to understand the research-oriented code。 So。

 in general， a research-oriented code is implemented for figuring out something new。

 and summarize the results into paper。 So， research-oriented code。

 AI project is a code with mainly by data scientists or researchers， for figuring out new knowledge。

 So， finding something new is the highest priority when writing the code。 Now。

 this is a common life cycle to write paper with code。 Only one person starts to correct data。

 pre-process it， train or calculate the data， then see what the results are telling you。

 If the results present new knowledge， write paper with it and publish。 So。

 this code is pre-processing code written during this process。

 This code seems to be a long function and used for a group， but it tends to be visually。

 accessible from the top to the bottom。 Also， it can be easily and quickly written。

 It's not critical， but it's enough to quickly get results。 So。

 this is the example of calculation code。 This code is used to append and use data frame。

 It easily handles input data and it takes output data with data frame。 So， on the other hand。

 what is the code in production level？ So， according to this thesis about how we can evaluate a production system based on the。

 rubric， the CC scores ML system from 0 to 12 pass。 So。

 zero points seems to be more like research oriented code before transformation。

 The ML system scoring from 7 to 12 pass is the products a few years after deployed on。

 the production environment。 So， this target to go over the four steps I suggested in this talk is the product scoring。

 from 5 to 6。 That means reasonably tested， but it's possible that most of those tests and procedures may。

 be automated。 So， deep product quality improved through these kinds of the development cycle。

 I have had the experience to be only one pass engineer in a company for a few years， but。

 generally engineers develop real application with a team。 So， they make architecture。

 implement features， testing review and release product。 So。

 they receive feedback all the time unless they develop very perfect product with no， bugs。 So。

 they need to keep repeating， releasing and fix the product。 So。

 this means the production code needs high maintainability， scalability and processing， speed。

 This is an example of both previous code that I refracted in Poissonic way。

 This code seems to be shorter than previous code and there are less complications and。

 set complications。 There are two simple functions on the bottom。

 This code can build the model in a faster and the simpler way。 So。

 I identify three differences between research-oriented code and production code。

 There are different scopes， different characteristics for coding style and different objective coding。

 style。 So， researchers focus more on writing pre-processing code and mr code， calculation code。

 On the other hand， engineers have responsibility to write whole part of the coding in production。

 level。 So， research-oriented code seems to be easily handled and visually traceable and on the other。

 hand， production code needs to be concerned about high-calrogation speed， high readability。

 and there should be testable and the modular。 This is because the researchers focus more on finding the most efficient and suitable machine。

 learning model， what calculation code and on the other hand， engineers have responsibility。

 to make the code work on the server correctly and reliably。 So。

 what are Poisson engineers supposed to do for research-oriented code at first？ So。

 what is the first thing to do？ So， deep typing the code， so read the code before write it。

 The left picture is a Jupyter notebook which has long lines。 I used to frequently see the code。

 Okay。 On the Jupyter notebook from the， okay， I will continue。 So。

 after you see the research-oriented code， take notes for deep typing the code as if you， scribbling。

 So， there are three strategies to take notes prepared for modularization。

 I just quickly take notes while reading the research-oriented code。 So。

 first one is to write comments by using shop。 Second is add to the comments。 These two are things。

 The other one already does。 So， third one， the third is move documentation。 What's that？ So。

 move documentation is very useful to efficiently understand the code with the chip。 So。

 if you're using a VS code， install a live share extension of VS code which enable。

 to share code and edit together。 So， after a moment of documentation。

 the chip members who join this MOOP can be on the same。

 page and do not need to extra wiki and documents about this code。 Also。

 if you do with junior or higher， it can be good for onboarding。 So。

 this is an example of part of a page of research-oriented code with comments。 So。

 while reading the code， are you understood from the code with comments and to the memo？ So。

 this should be done for understanding more about the code， not only for remembering。

 the code and to do。 So， the conscious and attitude about understanding more about the code and getting more familiar。

 with the code is going to make different in the way to take notes on the code。 And， easily。

 lead to the next step。 So， next thing to do is to modularize the code by using levels。

 A research-oriented code has three types of scopes， such as the code to prepare， propose。

 the process and calculate data。 So， based on the comments， you wrote in previous steps。

 categorize the code into preparation， code， propose processing code， and calculation code。 So。

 after grouping each code with levels， we can break them into functions and make them， testable。 So。

 while grouping each code， you would find duplicated codes and use lines or libraries， or small bugs。

 you can fix them in this step。 So， then we can get a modularization outcome like the table。 So。

 we could get modules for each one page of research-oriented code。 We could get preparation。py。

 which has functions to access database， execute clearly and load， input data。

 We also could get processing。py， which has functions to replace， call the boycott data。

 with discrete numbers， filter input data， and rename columns。 So， finally， we get prediction。py。

 which has functions to calculate logistic regression， and output results。 So， now。

 as a result of that， so the research-oriented code became loosely coupled。 So。

 you can make directory structure mapping each module。

 This is one of the directory structure examples of API， developed with Frask。

 I found that this kind of directory structure is very simple and easy to work with。

 The directory structure on the left box has API directory and module directory and main。

 root directory。 The Tandaini。py module located in API directory has a root in a list。

 Each logic for each endpoint is implemented in each module located in your L directory。 So。

 if you are working in a small team from one， two， three members and implement new， modules。

 you can just add it to new endpoint in your L directory。 If the number of team members increase。

 you can split each module into each version and， work together。

 staying loosely coupled between modules。 So， I wrote the directory on the slide in the case of API with Frask。

 And Frask API also can have similar directory structure， but generally， the way of mapping。

 relies on the manner of each framework and where you want to develop。

 So now we reach to the step to refactor preparation code and pre-processing code。

 So before starting to refactor the code， we should write a test code roughly at the dog。

 thing such as restructure， ticket style， non-buy style and Google style。

 And execute code formatter such as Brack or Alt-PEP8 and check if code correctly works。

 by using Pytest and check the consistency of coding style by using break8。 And other than that。

 type annotation could also be one of those options as code documentation。

 if all team members are familiar with the grammar of type hinting。

 So you can narrow down more about the requirements of each code through these three processes。

 Then we can finally start the refactor code。 Now which part of the code should we refactor？ First。

 focus on preparation code and pre-post-processing code。 In general。

 ML library and mathematical library are used in prediction module， so the code。

 does not have many lines to write in refactor。 So optimization of GPU bounding processing can be done later after we could make sure。

 if the code can work correctly on the server。 So we have a chance to figure out if the prediction is not valid yet through refactoring。

 So we are trying to refactor IO。 Preparation。py has a group of codes to access to database or object-strage by using query。

 and grant library。 So let me share a few examples I frequently have faced and how I simplify them。

 In exploratory analysis phase， what data wrangling are data scientists are not sure of which。

 data is specifically useful。 If you receive the first query with a star。

 so narrow down the data to extract from database， and it could be much faster and lower cost like the second code。

 In accessing the object-strage， sometimes you might make effort to write many lines just。

 for loading CSV file by using grant libraries。 So one of my suggestions is that you wrap the code and make it simpler。

 then it can be， reusable and cost effective。 That's because this kind of code is the same code even though anyone writes。

 Everyone AI/MF projects such as data engineers， Python engineers， and data scientists write。

 the similar code。 Not only you achieve members， but also the members in the other team might be writing。

 the same code just for loading the data。 So also， it might be chances for Python engineers to make a big impact internally。

 Next let's look at the P-proscented file。 The libraries for analysis use and development use are different。

 A group of the code to be processed or post processed data tend to include various kinds。

 of libraries and data type is also mixed。 As one of the common cases。

 there are three ways to P-post processed data like pandas， parts only， and the secret query。

 The common operations are filtering， rebraced， deduplicate， and there is the data for each。

 three way。 So most of research-oriented code needs pandas， schools， or secret query or both。

 So these three ways have different features。 The code with pandas is iterative to fix in the writer code。

 Python only code can be testable and secret query has high processing performance and。

 simple grammar。 So it depends on the architecture and the project scale。

 but the first thing to think， about is whether pandas and secret query is necessary or not。

 And refactored them into Python only code for making them more testable。

 If the data using for calculation is already specific and extracted in preparation。py， we。

 don't need to write many pre-processing codes by a person。

 So last step is to make them product which is API。

 So what products can be generated from research-oriented code？ The output is not paper。

 it's a product which means a quarter working on the server and， users can access to it。

 But there are several kinds of product appearances。

 So quickly let me narrow down the scope about the products in this talk。

 So the product can be web application or web API。 So this is drawing of the flow chart of transformation from research-oriented code into products。

 So in this slide， POC is more like products and NRC scripts are mainly written for business。

 reports。 So when having a local general overview of data-oriented projects， both types of the code。

 can also exist in this flow but this talk focuses on research-oriented code。

 So the four transformation steps could be mapped on the flow chart like this slide。

 There are four patterns of transformation from research-oriented code into products。

 One is the model or the configuration code extracted from the research-oriented code is。

 directly integrated into web application。 The second is that we can implement API based on the research-oriented code。

 Third is that when we use ready-made API or third-party API， we normally integrate them。

 into web application for using them。 Last one is that when the model or the configuration code is already integrated into web application。

 we extract it this part of the code and make it API for public use。

 So this talk focuses on developing web API。 So in order to transform our research-oriented code into web API。

 we have to implement how， to route requests and check requests。

 check error and routing points which are in the points。

 Let me share the tips to implement requests routing in the URL。

 There is a table of input data on the left side on this slide， which is used for two parameters。

 logistic regression。 On the right side， there is a table of output data， calculated by the model。

 The input data is about whether students answer each question correctly or not。 The item name。

 which is the name of question， item ID， how difficult each question is， subject， names。

 exam name and correction， which is binary data。 On the other hand。

 output data is about probabilities to answer questions correctly。

 So the role of this API is to get probabilities so we can directly level probabilities as。

 the endpoint name。 So based on best practices， our function name should be verb or verb plus noun。

 So the role of this function is to calculate the result or get probabilities so we can。

 level our calculate result or get props as a function name。

 So understanding what data is input and output， which means understanding what data the code。

 calculates and makes is considerably important step to make API。

 Next is implementation of request parameter check。

 My suggestion is to implement request parameter check with JSON schema。

 This request called common has student name and student grade as parameters。

 This JSON file is sample JSON schema。 In this file。

 you can define what data should be checked and how the data should be checked。

 So this make name gradle JSON file is defining the data type of student name as string and。

 it must be included as the request parameters and the data type of student grade is also。

 defined as string and it must be included as true and the maximum length of character。

 is limited to 120 and the minimum length is one character。 So in this example。

 request parameters are not allowed to be empty。 So based on JSON schema， the JSON validate。py。

 the request parameters， the first validation， function checks whether the JSON schema exists or not。

 The second validate schema function checks whether the request parameters are correct。

 or not based on JSON schema， defining previous slides。 In order to execute these functions。

 write function name as a syntax sugar under the end， point。

 The code with JSON schema seems to be complicated but most of the code are like this on this。

 slide so you can refer to this code and we'll probably slide on my Twitter as well。

 So last implementation is error check。 So before implementing the code。

 we need to think if processing should be stopped or， continue for each code。

 So whether the processing is stopped or not depends on service specification。 For example。

 if you implement recommendation system， sometimes you do not want to stop services。

 and continue recommendation even when the preparation code fails to load certain data。

 So this is the example of error 100 with Frask。 If you stop processing。

 you can use a bold function and detect error。 If you do not want to stop processing。

 you can just return the result with JSON if I。 So it's very easy to implement it but it's more difficult to decide which part of the。

 code processing should be stopped or not。 So I suggest to talk with product manager or QA team about the service specification before。

 starting to implement error check。 Okay， so let me quickly summarize full step transformation from research or linked code。

 into products。 First thing is to understand the characteristics of the code and figure out how it is working。

 by taking notes。 Taken， modularized the code based on the code documentation by labeling the code as preparation。

 repository processing and calculation。 Thirdly refactor the preparation code by simplifying IO and preprocessing code by changing the。

 coding style。 So lastly， make them product which is an API composed of request routing。

 request parameter， check and error check。 So after deploy the product。

 we need to optimize the speed and the state stability by doing load， test。

 So there are useful load test tools such as load cost and vegeta。

 Load cost is a Python based load test tool which can make it possible to manage load test。

 scenario by Python code and monitor the load loading on the server by GUI。

 The vegeta is a go-lamp based HTTP load test tool。 It can be used both as command line。

 utility and library。 If you are looking for the light load test tool。

 vegeta could be one of the options。 And you can do vegeta attack on your code。

 So by using those kind of load test tools， if we figure out the performance is not good， enough。

 we do parameter tuning of web server and the application server。 And think about end synchronous。

 synchronous。 Even if it is not improved， re-think about architecture。

 infrastructure or refactoring the， code with different languages。 So just in case again。

 four steps transformation from research oriented code into product is， very simple。

 Understand the code， modularize the code， refactor them and then make them product after they。

 deploy the code， check the performance。 Thank you。 [APPLAUSE]。



![](img/8ace30fbec6033087b978b46f1139731_4.png)