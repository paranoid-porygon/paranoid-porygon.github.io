---
layout: post
title:  "You need to lock your shit down. Seriously."
summary: "Foreign operatives, scammers, script kiddies, and predators are using your public information to impersonate, defraud, stalk, or frame you, your kids, your employer, and maybe even your country."
date: 2026-05-08  
last_updated:
tags: [privacy, tech hell]
published: true
assetdir: 
---

I conducted a couple interviews yesterday for a developer position at my current employer. Neither of them went well. But one of them was worse than the other, and I and my team are increasingly convinced that it was a foreign operative trying to embed within our company. We are also convinced that the credentials and background he used to get an interview were real... they just belonged to someone else whose name, face, and academic and professional work history were all publicly available online.

Recommendations up front: you need to lock down your online footprint **RIGHT NOW**. And I mean as much as you can. Make your LinkedIn private and remove your profile picture; deactivate your account if you are not actively job hunting (or aren't one of those goofball content creators on the site). Erase your employment and academic history from your Facebook and other socials, and don't have an actual picture of your face as your external-facing profile picture. Do not post videos that have both your face and voice on the web unless your job requires it for whatever reason; publicly available recordings of your voice open you up to deepfake impersonation. When sharing your resume on job boards, redact your current employer and tell recruiters that you can privately email them a copy of your full resume upon request. Never give a photo of your ID to a website for identity verification, and if you have to send a copy to a bank or landlord, use electrical tape to cover your birthdate, expiration date (minus the year), and license number, and request that they delete the photo upon verfication (or just insist that you handle ID verification in-person).

I won't go into details about my employer or the position that we're trying to fill, but I can talk a little bit about what indicated to us that this guy was an op, and how we think he got as far as he did.

# The Incident

### Timeline

The interview was remote; the candidate claimed to live on the West Coast while we interviewers and the job in question are on the East Coast. We saw the candidate attempt to join the Teams meeting lobby 8 minutes early, but then promptly left before any of us could join to let him out of the lobby and into the actual meeting. He re-joined right at the start time of the interview with both camera and microphone off. We asked if he was having technical issues, and then a full minute passed before his camera and mic turned on with no explanation or apology.

The candidate had a blurred background, however it was obvious that he was in a conference room, not a home office or anything similar; he was seated at a large table and had a swivel chair next to him identical to the one he was seated in.

We gave him a rundown of the interview process and he acknowledged extremely curtly, no facial expressions or niceties whatsoever. He had a non-native English accent but he didn't say enough for me to be able to place it. He appeared to be East Asian by race.

Interviewer 1 asked him a general question about his interest in the role. The candidate noticeably hesistated before responding and then began to obviously read from a script on a computer screen. We were one question in and already knew this guy was using an LLM to interview. The answers were extremely generic and more-or-less just read back the question to the interviewer.

Interviewer 2 and I decided to probe further by asking opinion-based questions (e.g. "where do you see yourself in 5 years"). Similar hesistation, similar absense of expression, and the answers were basically nonsense. We ended the interview after three questions.

### Evidence

Before we even interviewed him, there were orange flags about the candidate. Without saying too much, the role he interviewed for was an obvious departure from his work history, including that it would require moving to the other side of the country. We went into the interview already planning to ask just what possessed him to pursue this role.

His terse behavior and speech was immediately off-putting, but I was able to initially explain it away as "spectrummy engineer spent all his time learning to code and none of it learning to be a person". Even still, considering his storied work history, I expected him to be a better interviewee. Maybe the West Coast just doesn't care about being human?

The interviewee claimed to have been recently laid off. If that was the case, why was he in what was obviously a conference room? Even if he were still employed, who in their right mind would interview for a new job at your employer's office?

The usage of an AI chatbot to interview is by itself disqualifying, but also by itself not evidence that he was a foreign operative. Taken into consideration with the rest of the evidence, however, it makes that case more compelling: it's much easier for an operative who neither has the technical expertise in question nor the ability to fluently understand and speak at this complicated of a level to use an LLM to tell him what to say.

Interviewer 2 did some cursory OSINT on the alleged name and background of the candidate. The public-facing profile pictures from social media accounts with that name did not totally match the face of the candidate, but the camera quality was poor so we could not be certain.


### How they did it

I suspect they simply looked for Asian-Americans with a relevant professional background that looked similar enough to the candidate that the untrained white person wouldn't be able to notice the lack of resemblance and then built a persona around a real person's real identity. The poor sap whose identity got stolen had a publicly-searchable Facebook page and a detailed LinkedIn page. Combine that with the Equifax leak from years ago, and you have a whole persona served up on a platter for a state actor to take advantage of.

They then targeted job postings by employers that work in sensitive sectors where it would be valuable to embed an operative. These aren't hard to find: if you look for cybersecurity firms, telecoms or utility companies, federal contractor roles, or anything that is similarly highly sensitive, a bad actor can do some real damage or score some valuable intel.

Once a relevant job posting is found, they can use an off-the-shelf AI model and train it on the job description and relevant tech stack and required skills to answer interview questions with a reasonable level of passability...if the interviewers are morons or asleep on the job.

You then put your operative in front of a computer with both the Teams meeting and chatbot open, feed the audio output into a dictation program, and use the dictation as input into the chat prompt and you have an easy-to-set-up infiltration operation. The advantage of this approach is that it's very cheap and little is at stake: if the ruse is detected, there's no way at the moment to report a fake persona and share its usage across the industry, so it only gets blacklisted at the company you interviewed with and can be used repeatedly. Gone are the days when an operative's life is at risk during an espionage operation. Now all that's at risk is a burnable persona.

State actors can afford to do these kind of cheap, low-quality operations and then just "spray and pray" that one of them gets far enough to get embedded. It doens't matter if 95% of them fail if they are doing hundreds of these a day.

### Eliminating alternative explanations

I want to address some other likely scenarios besides this having been a foreign state actor.

#### Paid Interviewer

If this were an individual that was paid by another individual to interview on their behalf, I would have expected him to have been much better at his job. The cost for an individual to hire someone like this is less trivial than for an organization or state to do so, so the customer would expect a higher level of proficiency. Additionally, invididual paid interviewers likely are not doing this sort of thing at scale the way that org/state sponsored ones are, the latter of which can afford to throw more manpower at the problem even if the likelihood of case-by-case success is lower; if even one succeeds, they got their money's worth. An individual customer wants quality results, not just any result.

#### Hacking ring or organized crime group

This is the most compelling alternative explanation in my assessment. Organized crime rings and scam shops can do this sort of thing at scale in a manner similar to state actors. That said, organized crime rings are profit-driven and likely would have had better-trained interviewees. They also tend to go after low-hanging fruit in terms of moneymaking, and embedding an operative in a company doesn't produce fast cash the way that other scams do. This might be a more sophisticated crime operation, however, where the crime group embeds an operative and then sells those services to nation states. I haven't seen anyone report on this sort of collusion, and I also expect it'd be more expensive than rogue states are willing to pay for when they can just do the same thing themselves in-house more cheaply.

#### Just an idiot who is bad at interviewing

If this guy *was* who he said he was, and his resume *was* authentic, he would have been much better at interviewing considering the places that he claimed to work and how long he worked there. If his identity was authentic but his work history wasn't (which would also require a fabricated LinkedIn), he should have known that we'd figure that out when we checked references. Surely if he was smart enough to make a persona, he would have been smart enough to know how to interview better.

### How did he even get *this* far?

The existing screen process just requires a phone interview with an HR representative. The inability to see the candidate's face obscures their identity and if they are reading from a script. The HR specialist also doesn't have the technical expertise to be able to assess if this person is actually skilled in the areas listed on their resume or if they are full of shit.

In this instance, the candidate hit a wall during the first interview after the phone screen. They didn't get anywhere close to sensitive company data, but they did manage to waste the time of four employees.

### How can this be identified earlier?

Screening interviews need to be conducted by a single interviewer with some technical knowledge. The screening session needs to be on-camera. This screener needs access to the candidate's full, unredacted resume and must perform preliminary OSINT to validate the candidate's identity and authenticity.

The screener can ask that the candidate, while on camera, to wear a blindfold or otherwise cover their eyes to prevent LLM usage, and put their hair up so that it is clear that no audio device is feeding them answers.

If they pass the screen, the screener should then be present for the first actual interview to verify that the same person showed up to each and that there is no discrepancy in appearance or whereabouts.

## Why does this matter?

The persona that this operative used was taken from publicly available information volunteered by a real person on social media that they declined to secure. We ended up blacklisting the identity of the real person the persona was based on from every being able to interview with our company. I asked my boss if there wasn't a better way to handle this so that we didn't punish an innocent person, and his response was something to the effect of "tough shit, should have been more private online". And honestly, I think he was right. The likelihood of that person ever interviewing with us in the future is slim, but not impossible, however the likelihood of someone attempting to reuse his persona is much higher. The harm done by blacklisting a real person is less than the benefit gained by blacklisting the persona.

This is only going to get worse as AI gets more sophisticated. And it won't just be highly technical IT personnel that are going to be targeted by state actors. It will be average people having their identities stolen to perpetrate financial crimes by organized crime rings and individual criminals. It will look like kidnappers and predators [impersonating you on the phone with your kid's school][its-not-sweetie-its-michael-scott] and using a voice synthesizer to copy your voice. 

## What can the average person do?

Stop whining about "my information is already out there, it's hopeless, I'm being tracked everywhere." Shut up. There are steps you can take to stem the bleed, they just require work. Stop being a whiner. I was very easy to find online roughly 3 years ago and I have almost completely erased any useful information attachced to my real name on the surface web. It can be done.

We're not concerned with government or corporate tracking of your online behavior here; that's a different topic with different solutions. We're talking about how to minimize identity theft and stalking, which are much easier to tackle.

* Make your LinkedIn private and remove your profile picture. Deactivate your account if you are not actively job hunting (or aren't one of those goofball content creators on the site). 
* Erase your employment and academic history from your Facebook and other socials, and don't have an actual picture of your face as your external-facing profile picture. 
* Do not post videos that have both your face and voice on the web unless your job requires it for whatever reason; publicly available recordings of your voice open you up to deepfake impersonation. 
* When sharing your resume on job boards, redact your current employer and tell recruiters that you can privately email them a copy of your full resume upon request. 
* Never give a photo of your ID to a website for identity verification, and if you have to send a copy to a bank or landlord, use electrical tape to cover your birthdate, expiration date (minus the year), and license number, and request that they delete the photo upon verfication (or just insist that you handle ID verification in-person).
* **BONUS**: poison your data by putting intentionally false info on your public profiles (fake employer, bogus birthdate, AI generated profile picture that kinda looks like you but isn't actually you) or make a bunch of fake accounts under your name with different info on each (and leave your real one sparse)

## Wrapping up

There might come a day when you have to interview for a role only to find that you have been blacklisted from the company by no fault of your own other than you were too leaky with your personal info and it got turned into a persona for a criminal. This is avoidable, and the onus is on you to avoid it.

{% include img.html alt_text="I want YOU to give a shit about your own privacy and identity" asset_path="post-images/you-need-to-lock-your-shit-down/i-want-you.jpg" %}

[its-not-sweetie-its-michael-scott]: https://youtu.be/UZSiB77mHJs
