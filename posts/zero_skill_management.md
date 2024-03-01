Title: Zero skill management
Date: 2024-02-29
Tags: management

This is a story that I have seen play out quite a few times in my software development carrier.

A software development team as a delivery problem. Or a quality problem. Maybe both.

Management adopts a new software project management method. Let's call it Agile. Or Scrum.

The team see some effects, metrics are improving a little - no more than a few percents though, maybe 10%.

One year later the problems are back and they are worse than ever.
If metrics still look good, it's mainly thanks to managers and/or team members gaming the metrics.
But quality or delivery or whatever the original problem was identified to be, has never been less under control.
All the old bad habits are back.

There is a LOT of literature describing this kind of story.

Everyone wonder "why are software project management methods failing so much ?"

<!-- end-of-preview -->

Is this a problem of portability ? A method could work for a team in some context but not be a good fit for another team, in another context.

Or sustainability ? Teams adopt the methods and have a few successes, but gradually slide back to their old bad habits and loose sight of "the way".

Or a lack of true understanding ? Teams adopt the tools but fail to understand the philosophy of the method.

## Lack of management introspection

For a long time I've interpreted this pattern along those lines:

- A team have problems with quality or delivery.

  The problems come from a bad team culture ("delivery speed obsession", "just get the job done", "we can't afford quality").

  The team culture comes from values transmitted and enforced by top-middle management.

- Management can't admit to themselves they are the cause of their team problems.

  Managers think the problems come from team members not knowing how to do their job right 
  (symptoms: managers often offer to "show how it's done" to some team members, to remind them of the standard of a job efficiently done.
  After a few hours, at most days, managers leave the team and go back to their normal life.
  Quite often, after achieving absolutely no part of the actual job, and with a vague promise to finish it in a few days).

  Or, they think it's just an organizational problem (maybe a growing pain if the team scaled too fast),
  the team just need to collaborate more efficiently.

- Management pick a random software project management method, the one that's getting all the hype those days.

  "Agile" methods (lol), Lean, Scrum, Shape Up... You name it. The snake oil vendors don't lack in our field of work

- The team looks at the method and think 

  "It's cool, and we really want to say we do the same method as all the cool kids on the block.
  But those parts here look hard, those look boring, and these ones we don't understand. Let's just no do those parts."

  This is what I call "gutting the method". It looks like the actual method, but all the important parts inside are missing.

  And just like a gutted fish, it turns out that the method without any of the important organs inside ? Well it's pretty much dead already.

- The team starts using the method, setups metrics, everyone is working extra hard to learn the new way.

  This extra effort pays up on the short term and metrics seem to improve, but it's not sustainable.

- After a few months, enthusiasm is spent. The method has made no real difference since, well, it's been gutted from the start.

  The culture that bred all the problems in the first place is still here.

  Management has not changed any of its values.

So basically **I attributed the failure of all those new methods adoptions to a lack of management introspection**.

And in my experience, this phase of "new magical method adoption" is actually the first stage of the team's death.

Management doesn't want to see that they are the main cause of the problems.
Management would rather choose any other explanation.

And luckily, there are a lot of snake oil consulting companies that offer them just what they need to escape their reality.

Use Scrum and with those simple 42 steps your team can learn to work efficiently too !

## How I started to suspect this picture was incomplete

I remember a software talk I went to see in town a few years ago. It was about DDD.

One of the DDD community guru was here, and he told his story of how he started to work as a junior consultant in a big team in a big company,
and the team was working with TDD and had a lot of problems.

After some years, he changed jobs and went to work on his first DDD team. A much smaller team of 5, maybe 10 members.
All senior developers with considerable experience. The team worked like a charm.

He used this story to draw the conclusion that "TDD doesn't work, but DDD does".

As a true TTD-er, needless to say, I was triggered 😂 

I immediately thought 

"Well d'uh. Your first team was a big team with a lot of juniors,
so surely you had a lot more causes for your problems than just TDD: lack or heterogeneity of skills, communication failures...
From what I've seen in big companies, you probably didn't event practice REAL TDD (I know I know, I sound like a TDD evangelist in my head, sorry).

On the other hand, in your DDD experience you had a small cohesive team of experts, and yourself had a lot more experience,
so maybe this team's success wasn't entirely due to using DDD over TDD, but rather from having a much higher available skills set 
with less organization costs than in the first one."

Which led me to think about similar experiences in my own carrier.

It appeared to me that teams that worked well and had little to no problems had a few things in common:
- they were small. No more than 10 developers total.
- their members were all experts. Think like, 10 years+ of experience, often more than 20 years. I was always the most junior member with my measly 5 or 8 years.
- they had really high skill levels compared to the problems-teams. And they took pride in honing their skills. Good enough to do the job was not good enough.
- there was little-to-no management or organization. One-two managers max, they met with a manager to discuss your carrier once a year,
  no daily or weekly scheduled meetings. The managers all worked as part of the team, doing the same job - in fact they were experts too.
- they had ABSOLUTELY NO project management method. 

Now, I'm not saying they had NO project management work, or skill.

But they weren't following any widely promoted method. If I could sum up their approach, it would be:
- understand the user expectations
- do the work right the first time
- zero defect policy

That's it. As long as you did this, management left you alone.

All this led me to wonder "what if project management is not about following a method, but about growing a skilled team ?"

## Reading books about software project management

My biggest flaw is that I love to read. Books. In our days and age.

Even worst, I like to read books about stuff that are above my pay grade. Like software management books.

I know I know. Who reads these books really ? I must have a problem.

Recently I've been re-re-re-reading a few classics:
- **The Mythical Man Month** - best known for its metaphor of the 9 pregnant women delivering a baby in one month,
  but really a book worth so much more. Second System Effect explains to you why all your big rewrites failed.
  No Silver Bullet tells you that no magical tool or method replaces skills and hard work. Etc etc.
- **Waltzing With Bears** - about risk management. "Risk management is project management for adults".
- **Software Estimation** - if you want to not totally suck at estimating software delivery.

Those books have always fascinated me because
- what they describe just speaks to me on a basic experience level.
  I re-read them every five years, and what they describe fits my reality more and more each time.
- **I've met ABSOLUTELY NO manager IRL that had ever read ONE of these books**.
  None. Zero. Nada. I strongly suspect most managers are not even aware these books exist.

So I've always had to live with the contradiction that, while those books exists and feel essential to me,
they are useless since managers don't read them.

In my recent re-reading, I've also had a sudden realization

All those books describe the behaviors
and symptoms of the "immature/unskilled" teams, the team were project management is done in the most basic and naive way.

And those descriptions absolutely fit EVERY SINGLE problems-teams I've worked on. And their managers.

(That's in fact the reason why those books speak to me).

Does that mean that **none of the top-middle managers I've ever worked with had any basic project management skills ?** 🤔

Let's take the example of the immature risk management described in Waltzing with Bears.
- start by ignoring all risks
- do not list identified risks
- do not evaluate them seriously
- do not plan a budget to mitigate them
- do not monitor the risks realization
- do not re-evaluate risks during the project life

All these actually perfectly describe everything I've seen my managers do. I've never seen better.
I've never seen any serious attempt at risk management in my life.

Or at software estimation. Besides pure guesswork, "let's go with the lowest bidder, software engineers are pessimists anyway".
Roadmaps are basically cute little stories top managers tell themselves, but meanwhile all the front line workers know
from the start they are not achievable. And we say nothing because, well, our opinion is not, in fact, required.

Or a serious resource allocation plan. That take average team members availability into account.

Etc.

## The zero-skill manager

In with my recent reading spree, I've read a few other books:
- The Toyota Way - a book about lean management.
- The Knowledge Creating Company - a book about how to manage knowledge creation in your organization, and gain a competitive advantage from it.
- Quality Is Free - the naive belief that "we can't afford quality" is basically bullshit.

What strikes me in these books is the crucial role of the middle-manager in the organization.
Middle manager are expected to constantly improve their skills, and literally embody the management culture and values.

Yet, in my own professional experience, **middle managers appear to be the weakest links in the organization**.
Certainly they don't seem able to hone their skills or understand and embodies management values.

Quality Is Free identifies immature management as the main source of quality problems in most organizations.
In my experience this extends to most other types of problems in software teams.

Now, you might think I'm too harsh with middle managers, that I obviously have some beef with them.

Let me tell you another anecdote.

Recently my team leader took a paternity leave of absence. A member of my team accepted to fill in for him during his absence.

This team member had clearly expressed that he had no interest in becoming a team leader himself, but was willing to do the job anyway, for the sake of the team.

During this interim period, this team member had a lot of friction with our VP of Engineering, and felt quite disheartened about the experience.
In particular about the fact that the VP of Engineering did not really mentor or help him.
At some point he asked to be taught at least some basic management skills to help him do the job properly.

The VP of Engineering responded with 

> I'm going to reveal to you a little secret about management: you don't really NEED any skill to be a manager you know.

Now, I know this is just an anecdote, and could be discarded as just a problem with this particular manager.

But I've had a LOT of similar experiences over the past 18 years. The scenario is almost always the same.
- my middle manager points out some flaw in a recent project work
- I ask how I can improve my project management skills
- the middle manager laughs and tells me that "management is not about skills"

**My middle managers themselves, proudly admit that they never developed any management skills.**

## It's about skills, not methods ?

Taking this train of thoughts to its conclusion, the scenario would now looks like:
- A team have problems with quality or delivery.
- The problems comes from an absence of basic project management skills.
- Management can't admit to themselves that their lack of skills is the cause of their team problems.
- Management is sold a management method with the promise that they can dispense with traditional, boring project management skills, like estimation or risk management.
- The team removes from the method any step requiring the missing skills.
- The team starts using the method, but still don't develop their management skills.
- The projects starts to fail again since, well, no skills no win.

**What if the whole "software management methods don't work" thing is not about the methods themselves, but the skills of the managers ?**

All these methods in fact assume at least some basic level of project management skills.

Understanding the mythical man month. Going beyond guesswork estimation. Not just ignoring risk.

What if those skills are actually missing in most teams ?

It would be a bit like planning to win the "Tour de France" cycling tour, reading a lot about the strategies of the teams
who won the previous editions, then immediately getting on the bike on the first day with absolutely no cycling skills
or training beside what you learned when you were 5 years old (leaving aside the problem of your physical condition).

I'll take a guess and say that your main problem is not going to be your team's racing strategy.

This could also explain why **highly skilled teams seem to succeed without any formal method**.

And also why most methods, born in highly-skilled teams, fail to work in less-skilled teams.
Lean books speak about this a lot. Toyota themselves constantly struggle at doing Lean properly.
All Lean adoption stories in less-skilled companies invariably fail, because management enforced
the use of the Lean tools, but never acquired their own Lean management skills.

Developing project management skills is a lot of hard, obstinate work. Not really appealing to modern managers.
The managers described in The Toyota Way are totally different from the specimen I've met in real life.
They look like aliens to me (and I, for one, would welcome our new Lean overlords if they wanted to conquer our planet).

You'll fail at first. There will be doubt. You'll need to actually look at the consequences of your choices.
You might not appear to be in total control. Or even competent, at first.

On the other hand, the snake oil methods vendors have become very good at selling their stuff.

Never fail your delivery again with our world-class no-estimate-necessary method !

Grow the most efficient organization without having to learn about basic resource management theory !

With our method you can't fail so you can continue to ignore risk management !
