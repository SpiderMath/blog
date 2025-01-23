---
title: "Breaking Golden Rules: Learnings from Limit 2024"
weight: 1
draft: false
description: "I talk about my experience while conducting Limit as the Technical Team Head"
blog_tags: ["personal", "limit", "2025", "storytime", "programming"]
showAuthor: true
date: 2025-01-23
---

Well, there's a very common piece of advice which is given out to newbies in the tech industry, which is to **never** trust the user (what can go wrong WILL go wrong) and **never** touch a live database. It is a very obvious statement, since you always know what the risks are of doing both.  

## Reasons for Advice

Even in our extremely online world, there's a huge population who is very new or just inexperienced. Maybe they're just a diligent sales worker using it to run WhatsApp for their employment or maybe they're a housewife only watching YouTube to kill some time. Or just maybe, they're just a wide-eyed new user of the internet, only recently obtaining unlimited access to internet to explore a world outside the regular, because let's just admit it already, internet is a massive place. Everyone's experience is different, just like everyone's life is different and unique in their own ways. Assuming even basic technical literacy is a massive ask from the user's end, which although is admittedly a massive headache to consider.  

Tampering a *live* database is also a BIG no-no. While there aren't massive risks per se in terms of syncing when we manually tamper with a database, but *if* we mess up our operation, our entire setup will be screwed, resulting in critical loss of data while cannot be recovered! It is an insanely scary prospect, particularly when you're working with user data, since if it's gone, there often may not be any way for you to even contact the affect parties, often resulting in major losses

## Why am I bringing this up?

Welllll, while I was involved in the organisation of Limit, there had been a *select* few instances where we had to break those golden rules, and honestly, those have really taught me a thing or two about working on a website. 

### *Wait, what even is Limit? Why do you keep bringing it up?*

Limit is an open book examination on mathematics for school students and undergraduates, conducted by the Students and Research Scholars of Indian Statistical Institute, Bangalore Centre. It is entirely conducted by students of the institute, from start to finish. They set the papers, they communicate with the students and they set the website up. They manage everything without any institutional backing! It had stopped due to the COVID-19 Pandemic, and since then, there hadn't been any significant pushes to get it done. However, after I and some of my other batchmates joined the team, the push restarted and intensified, finally leading to a successful examination on 2024.

### *How are you related to Limit?*

I initially started off as a member of the Tech Team, under the guidance of [Bikram Halder](https://bikramhalder.github.io/), an extremely talented super senior and [Atreya Chowdhury](https://www.atchox.com), another extremely talented alumni from ISI Bangalore, then a Masters first year at ETH Zurich! I picked up the tools used to build the site and pushed to finally add a properly functioning payment portal using [KonfHub](https://konfhub.com/). This wasn't uneventful however, as I kept struggling in terms of integrating the same and often randomly even breaking the site locally 💀 (standard *intern* behaviour). Sadly, the implementation could not be completed fully before the Summer Break of 2024 arrived... Bikram da graduated to continue his M.Math in ISI Kolkata, leaving me and [Hrishik Koley](https://hrishik-koley.github.io/) (a senior also doing B.Math) as the only tech-literate people on-campus familiar with the code base.  

I had to reluctantly pick up the title of "Technical Team Head", attempting to work on the site while also communicating updates to the rest of the campus to ensure enthusiasm and also ask the new first-years to join the Tech Team. I was successful in my first endeavour but the Tech Team didn't appeal to the new first-years, needing them to get used to a lot of new frameworks amidst intense coursework of ISI. Bikram da and Atreya da could've brushed their hands off this project, however they decided to work on this despite their own academic responsibilities, massively boosting morale and actually taking immense load off my shoulders, since I wasn't still fully capable yet. Heck, I still don't think I am. They massively carried major implementations, particularly Bikram da, who built the entire exam server over the span of around half a month!  

During the examination, I provided over-the-call support to students giving the examination and tried to handle the issues some faced during the examination, together with Bikram da, Atreya da, [Sanchayan Bhowal](https://sanchayan-bhowal.github.io/), [Rinkiny Ghatak](https://in.linkedin.com/in/rinkiny-ghatak-75b792294), [Souparna Pal](https://in.linkedin.com/in/souparna-pal-113290324) and Ruchira Mukherjee (she doesn't have any official socials yet, sooo yeah). I'll be honest, I have never had so much fun AND stress at the same time! That's how I'm related to Limit. 😉  


## So why **did** you break these rules?

### Assuming user knowledge

For an examinee, the UI basically consisted of the question, a question number map on the right and buttons for the following actions.

- **Next:** You move ahead to the next question in the paper (Doesn't save the answer)
- **Save:** You stay on the same question but you are recording the current added answer 
- **Save and Next:** You are recording the current added answer AND moving on to the next question
- **Mark question:** You mark the question, probably for later review and solution (Doesn't save the answer)
- **Previous**: You go back to the previous question in the paper (Doesn't save the answer)

Now, out of all the present buttons, do you see something missing? If you guessed a {{<button>}} Submit {{</button>}} button, then you're right! However, we didn't *actually* need it in our case, technically speaking. See the way it was working is basically the intended purpose after all. It is an open book examination, so examinees are allowed to close the site and solve the questions if they wish, or refer any other site.  

The way it was configured set it up so that after the examinee started their examination, they just had a timer of two hours to solve their paper, after which their saved answers were auto-submitted, and the student lost access to their (often partially) solved paper. There was no notion of "submit" button involved, as every time a user decided to save their answer, it got saved to the database, in real time.  

However, during the Mock Tests we organised for stress testing purposes, we got an excessive number of calls inquiring about the Submit button, as everyone was "used" to seeing it. After all, it's an industry standard of sorts, you submit the paper early, and you DO NOT get to see it again or even correct it. We had thought that it wouldn't be too much of an issue for people but the sheer number of people asking for this caused us to change our minds. We had thought people would get our reasoning and our statements that the answers would obviously properly submitted to the server fell onto a lot of deaf ears. So finally, we decided to implement the submit button.  

This is exactly why, one should NEVER assume user knowledge. The overall decision we'd taken had assumed that the user understood how saving the data to databases usually work, and what would happen if the network request failed by chance! Along with that, another major pitfall with our initial decision was that the presence of a Submit button is practically a standard for all Computer Based Tests (CBT) and us being different all of a sudden basically caused major issues for everyone involved. It is now a de-facto standard that the final confirmation of submission is provided to the user to ensure that the user gets to be sure that their solution has been submitted.  

> However, we could've gone with another approach as well. When someone solves a question, we could also show a confirmation after the requests are successful to inform the user that their answer has been saved to the database successfully. But again, there was a risk of a lot of questions about the submit button, so we didn't go ahead with it.

### Working on a *live* database

After the implementation of the Submit button and a decent amount of testing, we all continued with our days, till the first date of the examinations for Category A on 11 January 2025. It turned out that, due to our focus into seeing whether the button actually did its job well enough, we never added a confirmation prompt to the user to ensure their submissions.  

This was a catastrophe, or so we thought as we got the first call from an understandably distressed examinee after they accidentally clicked the submit button. However, we were lucky to have only a handful of students facing those issues. However, how were we to fix this? We were in the quite the pickle!  

The good part: A simple solution we'd implemented for people to be able to start after the deadline but before the exam ended helped us out. We could just set it so that the examinee never submitted the exam!  
The bad part: We would have to do it over the live database, the scariest thought of any programmer.  

Hands shaking, Hrishik da connected to the live MongoDB database with me standing beside him, anxiously looking at the screen. We matched the affected examinees names and emails to ensure no one else's examinations got botched. After nervously cross-checking the database a few times, he finally changed their values on the live database. One by one. Manually. I stood by, anxiously standing by all the while, feeling like an intern suddenly propelled to the big leagues. Adrenaline pumping and fear on my mind, I just keep looking and relaying the affected examinee details. The updates were successful. After the updates were finished, we informed the affected examinees instantly, all of us sighing a major sigh of relief. All the anxiety and adrenaline in my system practically evaporated, I was finally free from the worry of messing up the database during the examination, finally I had some relief.  

We fixed the modal issue by the next examination date, 12 January 2025. However, this experience was definitely something I'll never forget. Maybe some rules are really meant to be broken atleast once in your life. 😉


## Conclusion

If the experience has taught me anything, it would be to never ever trust the users, and always assume the worst that can happen will happen, and prepare accordingly.  

Another really obvious realisation, atleast during the developmental phase would be to have a testing group, because as programmers, we usually care about whether a program works as intended by us or not. We don't always realise the point of views of a regular person who's not that tech-savvy or able to comprehend the complexities of the system in place. So, getting some people who do not share a programmer's point of view and instead represent the normal people's point of view, and getting them to test the final product is a really good idea which I believe would fix a lot of these issues people will face before it even needs to be public!  

~~Regardless, rules are made to be broken after all 😁~~