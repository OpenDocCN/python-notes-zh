# P81：Talk - Zachary Sarah Corleissen_ Localize your open source documentation_ a Kube - VikingDen7 - BV1f8411Y7cP

 Hello everyone， welcome back。 So we have our first talk in the evening by Zachary Sarah。
![](img/1ab8ec46d3e9322bf33e33244fb68778_1.png)

![](img/1ab8ec46d3e9322bf33e33244fb68778_2.png)

 Carlson on localized your open source documentation。 What to use there？ Hello。 So as advertised。 this talk is about localizing documentation for open source projects。 based on the experience of localizing Kubernetes documentation。 My goal today is to encourage you。 to localize your projects documentation。 I will do that by sharing advice based on what the Kubernetes。

 project has learned about localization in our own documentation set。 I have given previous versions of this talk that focus on different aspects of the Kubernetes experience。 If you are interested in research and tooling， I recommend watching my talk at Fozdem in 2019。 titled Multilingual Kubernetes。 If you are more interested in site analytics relating to localization。

 I recommend watching my talk at Write the Docs EU in Prague， also in 2019。 titled Found in Translation。 So who am I and why am I up here talking to you about all this？

 My name is Zach or Sarah。 My pronouns are they/them or she/her。 Feel free to use my names and pronouns interchangeably。 I am currently a staff technical writer at Stripe previously at the Linux Foundation。 And I served as chair for Kubernetes documentation from 2017 to 2021。

 And I'm still an approver for the documentation in the Kubernetes project。 And I'm also here because one of the maintainers of the NumPy library invited me to submit this talk and I said。 "Sure。"， So what is localization？ To paraphrase the W3C definition。 localization is the adaptation of documentation to meet the language。

 cultural and other requirements of a specific target market。 This definition。 I like it because it highlights that localization is more than translation。 And when I first started learning about localization for Kubernetes， I wondered。 "Why isn't machine translation good enough？ Why can't I just plug all of this into Google Translate and have it work？

"， And the short answer is that while machine translators are powerful tools。 they often return translations that lack linguistic and cultural relevance。 So in this talk。 I'm going to speak to you as if you are all project maintainers。 But this talk is for contributors in any role of an open source project。 So TLDR， this talk。

 I absolutely believe that most open source projects can and should localize their documentation。 And while localization does require some effort， the rewards greatly exceed the work required to do it。 So here， I say expensive and costly to differentiate between two different things。 Localization doesn't have to be expensive or costly。 And by expensive。

 I mean the financial expense of localization tools， tool subscriptions， things like that。
![](img/1ab8ec46d3e9322bf33e33244fb68778_4.png)

 It doesn't have to be expensive。 And by cost， it doesn't have to be costly。 I mean the cost of attention and labor in projects where maintainers already struggle with the burden of maintenance in the project。 So localization doesn't have to cost you a lot of money or impose a significant burden of labor。 That said， localization does require maintenance。 It requires maintenance to do and then some maintenance to upkeep。

 And for localization to be sustainable in open source projects。 that means choosing our maintenance wisely。 And I said on a previous slide that the benefits of localization are greater than additive。 When you localize， you get more back than you put in。 And one way in which localization benefits projects is that when localization， sorry。

 my time has me hurrying along and I'm just going to stop that。 So one way in which localization benefits projects is that when projects localize。 they usually attract a new set of maintainers， new contributors and new maintainers。 So when you localize， you open the door for more people to become contributors and maintainers to your project。

 So with the need for selective maintenance in mind， based on what we've learned in Kubernetes。 some of these things we learned the hard way， here is some advice about how to localize and scale your project while minimizing the amount of required maintenance。

 This is probably the single greatest factor to being able to scale localization in Kubernetes docs。 Be indifferent to the origins of localized content。 Don't care where it comes from before it shows up in a pull request。 And this applied indifference is one of the keys to sustainable maintenance。

 There are some reasons to be indifferent to localized content before it shows up as a pull request in your repository。 And one of those reasons is that different tools work better for different teams。 There are a host of tools out there that trans-effects， crowd in， Google Translate。 your time and attention as a maintainer， gaining expertise in these tools and then trying to support them for multiple localizations that doesn't scale。

 and your attention and time are not valuable when you try to support these things。 So applied indifference is one of those things that saves you maintenance。 And every team has tools and processes that are going to work better for them。 And that kind of non-standardization is healthy， but it also means that you as a maintainer have to be selective with how much attention you put into the origin of localized content。

 One of the benefits of this as well is that it is somewhat toolproof。 But if machine translation somehow becomes the perfect localization tool tomorrow。 you are already prepared because you already don't care。 That said。 once localized content does arrive in a pull request to your repository。

 you must care deeply about how it's reviewed and approved。 So here are some tips for a good strict review process。 Don't allow authors to approve their own pull requests。 This is seldom a good practice。 It's especially not a good practice in localization。 Require a review process。

 And in addition to requiring reviews， it's good to limit who can review localized content。 I highly recommend requiring reviewers to have native or equivalent fluency with the content they review。 I have zero fluency in Mandarin。 I do not approve Mandarin language content for Kubernetes documentation。 But because I know that strict reviews are required， I know that that content， when it is approved。

 meets the standard of quality。 That it is legible and meaningful to the people who consume that content。 So establish a canonical source language and branch。 A canonical language and branch are useful for the same reason upstream Git remotes are useful。 A canonical source language and branch helps your project scale。

 It means that localization teams are working from the same understood source。 And it's an efficiency piece。 I recommend matching your canonical source language to the primary language of your contributors。 and matching your canonical branch to the project's published branch。 For Kubernetes。 English is the canonical source because it's the primary working language for the majority of contributors。

 both to documentation and to feature development。 Main is the canonical branch in Git because it's the most current version of docs published on the website。 It doesn't have to be English。 If your contributors work primarily in another language。 consider using that language as a canonical source。 I recommend establishing a minimum viable documentation set。

 The origin for this phrase minimum viable documentation is Heidi Waterhouse， who is seated here。 Identifying a minimum viable documentation set serves a couple of purposes。 I think the most valuable is that it clarifies your project's most critical content。 This will help you develop empathy for your users。 What do they most need？

 What can they not live without？ And another reason is that it establishes a threshold at which a localization meets the requirements for inclusion in your published docs。 You have a very clear understanding of what is the requirement for a localization to meet before it can be published。

 I recommend scripting away common functions。 These links are to Python scripts in very tiny print for which I apologize deeply。 These are Python scripts for tracking difts between localization branches and for tracking difts from upstream source from the canonical branch。

 I talked earlier about how some translation tools。 how it's good for each team to have their own workflow around creating localized content。 As a corollary to that， there are some tasks that every localization team shares in common and it's good to standardize those。 Every team needs to track difts across their own collaboration branches。

 This is a script to basically find out what did everyone else work on since the last time I ran the script。 Where are we as a localization？ The other script tracks upstream changes in order to keep that localization current。 What changed upstream we need to now localize。 Unlike translation tools， which we don't care about。 difts are automatable。 If it's automatable， it's scalable。 This also。

 standardizing in this kind of scripted way， makes it easier for you as a maintainer and it makes it easier for teams to be able to support each other in a decentralized way。 This is another one of those greater than additive benefits teams support themselves。

 I recommend using tools that support localization or internationalization， what I have on the slide。 but localization as well。 Choose a content management system and static site generator that have built-in support for localization。 For example， Hugo and Gatsby are both static site generators that have built-in support for localization。 I recommend using a tool， whatever it is that you use for a static site generator。

 Choose one that is built on a language that aligns with your own projects。 languages and sources of expertise。 This increases scalability。 Kubernetes uses Hugo because Hugo is written in Go and unsurprisingly。 Kubernetes has a deep pool of Go-Lang expertise。 Avoid hard-coding strings。

 This is an efficiency piece and it's also a scalability piece。 I will say little about partials apart from mentioning just don't hard-code them。 Instead。 I will refer you to the Hugo documentation for partials and for the Kubernetes website repository as an applied example of what localized partials look like。 Partials only work once you solve for repository permissions。 Rather。

 they're an example of what you need to solve for with repository permissions。 Permissions are tricky to solve for。 The permissions required for localization teams to work effectively don't map well to the granular permissions that Git allows repo admins to assign。

 I do have a recommendation for how to solve for permissions。 but I want to dig into the specifics of why and how permissions are problematic for localization projects。 One example is branches。 Neither GitHub nor GitLab permit granular permissions to only create a branch。 When you assign permission to create a branch that is bundled with the permission to merge or to delete not only a particular branch but any branch in that repository。

 And the easiest way for localization teams to collaborate is on shared long running branches in the project repository。 This creates a challenge in projects where people contribute by forking the repository。 There's a whole set of reasons why it's a poor idea for multiple contributors to work on a branch of one contributors fork。 especially for a long running fork。 Conversely， there are good reasons to localize on a long running branch that is off of the project's main repository。

 So that means assigning a localization team the permission to perform right operations in the project repo。 And that raises a whole host of concerns。 So with that comes the question of permissions for approvals。 So you will almost certainly not be able to assign repo permissions in a way that matches the permissions that localization teams require to configure themselves and only themselves。 So for example， Hugo requires that localized markdown files be placed along a language specific path/content/two-letter country code。

 So it would be for Korean， it would be like /content/ko。 So Hugo requires that localized content exist on that file path。 but Hugo performs the site configuration for a localization in a root level config file。 The Hugo parser will fail a build if content exists on a file path and it isn't properly configured in the root level file。

 So localization requires permission to alter root level files。 And that's concerning for a whole host of reasons。 Partials are another example of that。 Hugo puts all of its string partials into a directory off of the root level that is not on the localized content path。 So granular permissions， it's impossible to assign them in a way that the Venn diagram is 100% overlap。

 So assigning permissions， giving localization teams enough permissions to do what they need to do creates a problem of trust。 And the intersection of team needs and get permissions creates choices about security that are unfortunately pretty either/or。

 It's all or nothing。 Not 100%， but it's pretty hard binary sometimes。 So let's talk about trust。 And trust is where you as a project maintainer decide what kind of maintenance you want to engage in。 Low trust means engaging in the custodial maintenance of periodically creating。 merging and deprecating branches at increasing scale， which is to say that it doesn't scale。

 Or you can have higher trust and engage in occasional forensic maintenance。 Forensic as opposed to custodial forensic maintenance of undoing disruptive actions。 Please note disruptive， not malicious。 There have been many disruptive actions related to localization work in the Kubernetes repo。 but none of them were malicious。 We're very fortunate in that regard。 It does require vigilance。

 It does require maintenance。 But this is where you as a project maintainer and you as a project figure out how you think about trust。 So I recommend treating localization teams with the same trust that you would give to any release branch。 And to treat localization maintainers the same way that you would any other release lead who has right permissions。 Not everyone who contributes localized content needs right permissions。

 But in order to scale as a project， every localization team should have someone who serves as a team lead and with whom you as a maintainer have explicit and well documented conversations about security and trust。 Perhaps unsurprising coming from a technical writer but document everything。

 A frank and explicit conversation about trust and permissions requires a thorough written understanding。 Every project has norms， expectations and boundaries about what people who have certain levels of permission can do with those permissions。 Because expectations and boundaries are cultural and because localization is also about cultural relevance。 It is important， I would say critical， to document the expectations and boundaries around localization work in your project。

 Every single disruptive action in the Kubernetes website could have been prevented with better documentation about the expectations for permissions and what people can and can't do with them。 So I recommend making issues in pull requests filterable by language。

 And one of the easiest ways to do that is with repo labels that are specific to a language。 They are an easy solution to what otherwise is a big maintenance burden。 Without the ability to filter by language， it's just its toil。 And as with any form of toil。 I highly recommend automating it away。 So I recommend language labels that follow the format of language plus the two letter language code。

 And this tracks to the same standard that you use to set that content path in a repository for example。 And it makes it easy to correlate that this label applies to this content。 And you can automate these with a GitHub action。 There is a GitHub action that will specifically apply a label to a pull request based on the content in that file path。 So when localization contributors identify something that needs to change in the upstream source。

 they will often open a pull request that includes both a change in the upstream source and their own localized content。 And these are the most noble and well-intended pull requests and they almost always languish because multiple languages require multiple approvers。

 It's a bit of a hassle to say to somebody， "Hey， can you do this in two pull requests rather than one？

"， But they will get approved more quickly based on practice。 I recommend preview builds。 It just makes the review process faster and easier for everyone regardless of localization。 Kubernetes uses Netlify to generate preview builds。 We use it as our content delivery network。 It's what actually publishes our rendered content。 And it works great。

 It integrates well with the site CICD。 So this is one of the best kept secrets in the Kubernetes website and I wish it wasn't a secret because I wish every project had a smoke test page。 A smoke test is one page generated from a single markdown file that contains an instance of every syntax or formatting element in your docs。

 And this is great for figuring out whether a proposed change works as intended。 Sometimes when you localize different languages have different site element requirements。 And this is a great way to test that。 And you can follow this particular path to a smoke test on preview builds so you can see whether it works before you approve it。 So this page， the smoke test page is crawlable by robots but it's de-indexed from site search so it doesn't interfere with organic search results。

 So handy。 So it is easier in some ways to set up a framework for localization before any of the work actually happens。 On the other hand， there are real benefits to adding localization tooling and workflows in equal partnership with the contributors who are actually doing that work。

 So both of these approaches are valid。 Do it before you need it。 Wait until you need it。 It's kind of a wash。 So is localization worth it？ All of the advice that I've offered so far has been like on the level of implementation detail and systems level process。

 So it's not clear from my advice what maintaining a localization looks like in practice。 So I want to examine some specific maintenance cases and I would like to use the example of Korean language content on Kubernetes。io for the month of March 2022。 So in March of this year。 six people contributed Korean language content to the Kubernetes website。

 And site traffic for Korean content constituted approximately 2。1% of site visitors。 At this point。 you may be asking whether the advice that I'm giving you is worth the work for six contributors at 2。1% of site traffic。 So let's unpack those numbers a bit。 So in March， kubernetes。io had 1。4 million visitors for a total of 4 million sessions and 10 million page views。

 Forget those millions for a moment。 For 1。4 million visitors， 2。1% of site traffic is 30。123 visitors。 And those six contributors in March sustained a one to many support ratio of just one to over 5。000。 One to 5，000。 Each one of those people who contributed localized content had the net effect of supporting 5。000 people in one month。 And that is a phenomenal number。 So yeah， 1。

4 million people of those site visits， only 52% of those visitors viewed content in English。 Almost half of the site's total monthly traffic was to localized content。 It is absolutely worth it。 So localization teams contribute more than just their own language content。 Localization teams contribute to upstream source。 They contribute to each other's mutual support。

 Localization teams in related language groups consult with each other for their own mutual benefit。 Localization teams contribute to shared tooling。 Both of the scripts that I linked to earlier。 None of the English language maintainers contributed those scripts。 Those are from the French and Korean teams respectively。

 And those are a benefit to the entire project。 And localization teams often become not just contributors but maintainers to a project。 So what's the point of all of this？ Why do we localize？ It is not for its own sake。 We don't localize for our own sake。 We localize to increase access to our projects and to drive adoption of our projects。 And when you open a door， people come in。 And that's beautiful。

 So here are some resources for the slide。 Again， in Tiny Print， Deepest Apologies。 This is to the Kubernetes documentation for how to contribute localized content。 This is a link to the website repository and the Python script directory。 This is a link to the Hugo static site generator。 And here's a link to the NumPy projects。

 That library's own localization work and the ongoing discussion there。 Shameless plug by our book。 Last year I co-authored a book called Docs for Developers。 An engineers field guide to technical writing。 I co-wrote it with Heidi。 It's a good book and you should totally buy it。 Thank you very much。 [Applause]， [Silence]。

 [BLANK_AUDIO]。
![](img/1ab8ec46d3e9322bf33e33244fb68778_6.png)