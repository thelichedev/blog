Title: A simple software quality evaluation framework
Date: 2024-02-21
Tags: software quality

Why should software development teams care about quality ?

I'm not going to elaborate much on the "why" here, I'll just borrow from the [Quality is free book by Philip Crosby][1]:

> Quality always pays for itself

And in my experience, quality is in fact a key competitive advantage.

Teams I've worked on, that adopted a quality standard of "zero-defect", actually delivered new product value orders of magnitude
faster than teams that "could never afford quality, because they needed to deliver fast".

But how do we know if we're doing ok on quality ?

I've always found that measuring quality directly is not easy. However measuring nonconformance always seemed to be easier.
When we find a defect in our product, there is evidence to analyse, and we can often evaluate the cost of the defect.
At least far better than the direct benefits of focusing on quality.

[Quality is free][1] also confirmed to me that the measurement of quality is the price of nonconformance.

So how do we evaluate nonconformance in a software project ?

<!-- end-of-preview -->

And how should we react to defects ?

Are all defects unavoidable in a software project ? 

Should we just accept defects as a price of actually adding new value to the product ?

Or are some defects symptom of a lack of quality consciouness in our practices ?

Do we just need to improve our tools and our processes, or is our team exhibiting a bad quality culture ?

Over the years I've developped an implicit mental model to evaluate the maturity of my teams' approach to defects.
This blog is an attempt to exteriorize this model.

Please not that the approach here is mainly oriented toward monitoring the quality practices,
not finding the root causes of defects, or deciding on the corrective measures.

## First a few definitions

Again, a lot of this is borrowed from [Quality is free][1], because it greatly relates to my experiences as a software developer.

**Quality** is conformance to requirements. In a product-oriented team, requirements should mostly mean end-users requirements.

**Defect** or nonconformance as a consequence, is any aspect of our product that does not meet the end-user requirements.
The user is trying to use our product to do something, but our product is unable to provide this service to the user.

This is intentionally a much larger definition than the usual term of "software bug".

Defects can be caused by our team's failure to deliver a working feature, but also by required features altogether missing in our product.

**Customers vs users** are any human being who will have to use our products, including for example our coworkers in customer support teams.
Customers are users giving us money to use our product.

Here I use the two words indistinctly, because I don't distinguish between paying and non-paying users when I think of product quality.

I think this is legitimate since in both situations, nonquality will incur important costs:

- users will not give us money
- support operation costs will rise

Different dynamics, same consequence: we loose money and users are not happy.

**Team vs developer vs manager** in this blog I'll refer to the "software team" for simplicity,
but I use the same framework at a more intimate level to evaluate my personal approach to quality.

In the same spirit, management expectations mostly apply to actual managers and lead developers at the team level,
but I also use them to "manage" my personal quality practices.

**Mature vs Immature team** (and developers, managers...) I've seen a wide range of quality approaches in my professional experience.

I'll speak about mature teams to describe teams where quality is a competitive advantage, and immature teams where quality is seen mainly as an unaffordable cost.

Mature teams exhibit (often inconsciously) control over their quality practices, immature teams exhibit a absence of control, or even of awareness of their quality problems.

## A simple sotware defect evaluation grid

I mainly use two axes to evaluate the defects in my teams' products:

- quality of delivery
- maturity of discovery

### Quality of Delivery

First, I try to evaluate the **quality of the delivery**:

- **delivery failures**: the user value was never delivered.
  Our users' expectations were never met by the product.

  This could be caused by defects in our implementation (what we call traditionnally "bugs") or by a missing part in the feature (we "forgot" to implement a part of what was expected).

- **regression failures**: the user value was delivered, but was lost later on.
  Our users' expectations were met at some time, but now they're not satisfied anymore.

  This could be caused by errors in our implementation of a new feature (leading to the loss of an older feature), 
  or any change in the operation context that prevents our old feature to work
  (eg. external services behaviour changes, tools retrocompatibility failures, load increases rendering the product unusable, etc).

- **missing requirement**: we never intended to satisfy this particular requirement of our users.

  It could be because we just discovered a feature we didn't know our users needed, 
  or because some process is not yet automated in our product and we accept that the user needs are answered by human customer support, 
  or even that we decide the opportunity cost to deliver the feature to the user is too high and we choose not to include it in our scope.

### Maturity of Discovery

Second, I try to evaluate **the maturity of the discovery** of nonconformance: the control my team or myself have over the defects.

I do this by looking at how far the defects made it into the product before they were found.

- **before development**: the defect never made it into the product implementation.

  It was found or prevented before the developer coded a new feature, or made impossible by the development process/tools.

- **during development**: the defect was found during the development of the feature.

  The developer's quality process/tools found or prevented the defect.

- **after developement but before delivery**: the defect was found before being delivered to the users.

  The users were never affected by the defect and in fact could never "see" it.

- **in delivery but before noticeable user impact**: the defect made it into delivery, but was found out before users operations were noticeably affected.
  Most importantly, we found (and fixed) the defect before users feedback came back through the customer support.

  Fixing the defect does not imply massively fixing users data, or informing the users of the defect.

- **after noticeable user impact**: we found the defect through direct users feedback, increase of customer support load, or discovery of users data corruption.

  Fixing the defect implies fixing users data, sometimes informing the users of the defect and apologizing.

It is well documented that defects cost more to fix, the later they are found in the development process.

Our attention should focus on tools and processes that prevent defects from being delivered,
or even making it into the product's implementation during development.

As professionals who aim to deliver a good product to our users, we want to be proud of our work, and there is also a psychological difference between:

- a defect that is caught before the users (outside our team) are impacted.
- a defect that massively impacts our users and increases our customer support load.

The second scenario hurts, and in my experience, in the long term it will lead to a lot of disengagement from team members.

### Defect evaluation grid

Those 2 axes give me a nice little grid on which to place the defects found by my teams:

|                        | delivery failures | regression failures | missing value |
|:-----------------------|:-----------------:|:-------------------:|:-------------:|
| **before development** |         👾        |          👾         |       👾      |
| **during development** |         👾        |          👾         |       👾      |
| **before delivery**    |         👾        |          👾         |       👾      |
| **before user impact** |         👾        |          👾         |       👾      |
| **after user impact**  |         👾        |          👾         |       👾      |

The limits on the two axes are not clear-cut and are subject to appreciation:

- a defect can make a new feature unusable because of an interaction with an old feature, breaking the old feature in the process.
  The defect is a mix of delivery and regression failure.
- discovery of a missing feature often comes after the delivery of a new feature that does not solve the problem correctly.
  Analysis of user usage and feedback improves our knowledge of the user's needs, and we realize the value was never delivered, or required ;
  and another feature is required to meet the user's needs.
- if you include your customer support teams into the "users" of your product (as I always do), 
  the limit beetween a defect that impacts the users or a defect caught before impact, will vary accordingly.
  The same is true for the product managers. When a product manager finds a defect on a pre-delivery test environment,
  after your team considered the feature delivered and integrated in the product, does it count as a defect found before delivery or after ?

The Maturity of Discovery axis is a continuum, and the limits are more of a guide helping me to decide how I should react to a specific defect, or which tools to use to prevent/discover it.

## Evaluating quality with the grid

### Quality of Delivery

**Delivery failures are not acceptable** in a mature team, in our day and age.

A mature team should be able deliver new user value that works, reliably.
We have a lot of tooling and methodology to help us dramatically reduce delivery failures.

The worst immature teams I've worked with, made it a habit to deliver features that don't work reliably,
and showed no interest in improving on this point.

Immature teams can also try to deliver working software, but using such methods and tools that the costs are indeed too high for their context,
reinforcing the management's perception that quality is a cost that they cannot afford.

**Regression failures are much harder to control.**

Interactions between existing and new features are hard to predict and discover.

Again, we have tools at our disposal to detect those defects as soon as possible,
but in my experience it is unavoidable that some of these will find their way into delivery,
and impact users.

**Missing requirements are actually a desirable kind of defects**.

You're discovering user value and you're updating your knowledge of the users expectations,
allowing you to adapt and maintain you competitive advantage.

### Maturity of Discovery

In my opinion there are 2 importants barriers:

- **delivery to the users** is the risk pivot point.
- **users discovery of the defects** is the trust pivot point.

**Every defect caught before delivery implies less risk** than after.

Of course, techniques and tools that actively prevent defects are invaluable.
As long as their cost does not make implementing new features prohibitive.
(We are engineers, our work is all about cost/quality tradeoffs).

On the opposite, **a defect being delivered to the user comes with a big risk**: the cost of discovery, diagnostic and the probability of
massively impacting operations are an order of magnitude above defects caught before delivery.

If all our defects are found and corrected before users outside of our team are impacted, we can develop **a relation of trust with our users**.
They will see our product as having a high quality standard.

I've worked on projects that achieved this, and it was always funny to see our users confronted with one of our defects,
but looking everywhere else into their context for the cause, because "it can't be in your application, your stuff always works".

On the opposite, teams that deliver products with a lot a defects impacting users, even mostly harmless ones, **will never build this trust capital**.

The humans using the product will always see its behaviour with suspiciousness.
Anytime they are confronted with a problem, they will suspect a defect in your product, and generate a lot of support requests.

The team's difficulties with diagnostic (the product is constantly flooded with errors) will increase the cost of this mistrust even more ;
it'll take time to exonerate our product when the defect is caused by another part of the context (eg misuse, miscommunication, defects in external services, etc).

## Teams maturity

### Immature teams

Immature teams can't afford quality because they have to move fast.

To be clear this is the situation of the majority of the teams I've worked on in my careers.

They have a frequency of defects that looks like this:

|                           | delivery failures | regression failures | missing value |
|:--------------------------|:-----------------:|:-------------------:|:-------------:|
| **before development**    | never             | never               | never         |
| **during development**    | rare              | rare                | never         |
| **before delivery**       | only the most trivial | only the most trivial | rare          |
| **before user impact**    | rare              | rare                | rare          |
| **after user impact**     | frequent          | frequent            | under extreme user pressure |

**Most defects are found in delivery, through user feedback and support.**

Those teams most likely have a support tickets system in place, probably even a backlog of issues that are never corrected.

Management in these teams considers their support tickets tools and boards as a proof of the maturity of their team's approach to quality.
But it is in fact a symptom of their quality problems: they have so much defects that they need a whole organisation to prioritize them.

If we look at the "after user impact" line, we see why **those teams can never build a trust capital.**

|                           | delivery failures | regression failures | missing value |
|:--------------------------|:-----------------:|:-------------------:|:-------------:|
| **after user impact**     | frequent          | frequent            | under extreme user pressure |

Users are constantly impacted by defects, even if they are harmless, whick **take their toll on the trust they have in the product**.

Missing requirements are developped on the basis of "how many users requested this feature", aka "extreme user pressure".
The team cannot anticipate the needs of their users since they are undistinguishable amongst the flood of daily errors in delivery.

Management in those teams usually only reacts when the impact of the defects is extremely high.
Low impact defects are ignored as "business as usual, move fast and break things".

This is **a major sign of management immaturity**, since the low and high impacts often have the same root causes.
Ignoring low impact defects actually leaves you exposed to a future high impact defect that could have been avoided by acting sooner on the quality signals.

The symptoms are also visible in the relationship between the development team and the support.
Support finds ways to "go around" the defects, that are well known and even documented in the support knowledge base.
**Support considers most defect will never be fixed** and doesn't even report them to the developers team anymore.

Developers show no interest in support operations and difficulties, and might even develop a form of contempt for the support team, enabled by management.

"Support does not know how our product works, and do more harm than good trying to fix the clients' problems"

### Mature teams

The mature teams I worked with have an implicit culture of **"do it right the first time"**.

I've only worked on such a team twice in my career. They are clearly the exception.

They have a frequency of defects that looks like this:

|                           | delivery failures | regression failures | missing value |
|:--------------------------|:-----------------:|:-------------------:|:-------------:|
| **before development**    | rare              | rare                | rare          |
| **during development**    | frequent          | rare                | rare          |
| **before delivery**       | frequent          | frequent            | frequent      |
| **before user impact**    | rare              | rare                | frequent      |
| **after user impact**     | never             | rare                | frequent      |

**Most defects are found before delivery, and never impact users outside of the team.**

Those teams might have a support tickets system in place, but it is mostly empty.
They have no issues backlog, as defects impacting users are rare enough, and considered severe enough to be immediatly fixed.

Management in these teams don't give much attention to the tracking of issues, since it mostly works fine as is.

If we look at the "user impact" line, we see why **those teams can capitalize on trust.**

|                           | delivery failures | regression failures | missing value |
|:--------------------------|:-----------------:|:-------------------:|:-------------:|
| **after user impact**     | never             | rare                | frequent      |

Users are almost never impacted by defects, and build **a capital of trust with the product and the development team**.

Missing requirements reported by the users or the support team are rapidly acted upon.
The team often anticipates the needs of their user by discovering missing requirements during development, or through the observation of the impact of their delivery.

Management in those teams usually does not actively manage quality.
It could be seen as a sign of immaturity of the management of quality (and in a way it is),
but it reflects the fact that quality is a core value of the team, which doesn't need managment.

**Quality is "left to the team"**. The team, and each of its members, reacts actively enough to defects reaching delivery.

The lack of management could however lead to a loss of this core value, as developers leave the team and
are replaced with new developers who don't have this core value.

**The team also has a trusting relationship with customer support.**
Support appreciate that they are not uselessly flooded with silly defects.

Developers show interest in support operations and actively develop tools to help support in their work.

"Support are our first users"

### My quality ethics

Over the years I've developped my own ethics regarding my and my teams' defects.

|                           | delivery failures | regression failures | missing value |
|:--------------------------|:-----------------:|:-------------------:|:-------------:|
| **before development**    | ideal             | ideal               | ideal         |
| **during development**    | good              | ideal               | ideal         |
| **before delivery**       | average           | good                | ideal         |
| **before user impact**    | risky             | average             | good          |
| **after user impact**     | unacceptable      | risky               | average       |

- **ideal**: good but unattainable for most teams I've worked with
- **good**: in the best teams, most of the defects should be caught at this stage
- **average**: the stage where most of the defects are found if we don't make any particular effort, in the average team.
- **risky**: defects found at this stage, though not always avoidable, should be considered as materialisation of a risk.
- **unacceptable**: finding defects at this stage should not be considered acceptable, and should lead to a team/developper introspection ("never again").

## Management

As software developers, we know that any software activity worth doing comes with a quality risk (risk of defects).

This is not the same as saying "we cannot afford quality", which is still today the approach to quality management in the vast majority of teams.
The difference is what I call **quality management**.

### Levels of management

I feel there is also a scale of maturity for quality management:
- **level 0 management: ignorance** the team only react to defects according to their impact.
- **level 1 management: awareness** the team at least tries to monitor defects, but will still react only to sudden increases in the number of defects.
- **level 2 management: improvement** the team monitors defects, and actively tries to improve tools and culture in order to reach a "zero-defect in delivery" state, but is not there yet.
- **level 3 management: maturity** the team has made quality a core value of the development practice. The team as reached a state of (mostly) "zero-defect in delivery", and is capitalizing on the trust they gained from all actors in their context.

Level 3 is not a lofty ideal, I've worked on teams and projects that had reached this level of quality.

They were also the teams were the delivery was the fastest I ever experienced, orders of magnitude above the speed of level 0 teams, who can't afford quality.

### Managing risk

The grid also shows where I would expect risk management in a mature team.

Developers should **implement risk mitigation on the "average" line**, using enforcing tools to catch defects at this state, 
and monitoring metrics that show if there is a derive in the amount of defects reaching this stage.

Developers and managers should **actively manage the "risky" line**, monitoring metrics and correcting practices when the number of defects increase - independently of the defects gravity/cost.

Unfortunately in the immature teams, no active risk management is peformed on those kind of defects, 
until the consequences are too important to be ignored, but then it's already too late
to fix the underlying team culture.

**Any defect found in the "unacceptable" stage should be acted upon**. 
Actions should be taken to ensure that defects are found before this stage.
Root cause analysis should be performed to understand the underlying management practices that enabled this defect.

Finding excuses or considering those kind of defects as unavoidable should never happen. 

Unfortunately it regularly does in the immature teams.

[1]: https://en.wikipedia.org/wiki/Philip_B._Crosby#Quality_is_Free
