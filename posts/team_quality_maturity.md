Title: Teams and quality maturity
Date: 2024-02-27
Tags: software quality

I've seen a wide range of quality approaches in my professional experience.

From mature teams exhibit who have firm control over their quality practices.

To immature teams who exhibit an absence of control, or even of total lack of awareness of their quality problems.

<!-- end-of-preview -->

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

Developers attitude towards customer support:
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

Developers attitude towards customer support: "Support team members are our first users"

## Conclusion

We often like to talk about the mythic 10x developers. Developers who delivers product value 10x faster than others.

In my experience, there is only one factor that make a team delivers an order of magnitude faster: a strict quality standard.

Yes, quality does pay for itself, but it also repays you 10 times mores.

Two determining effects that I can't stress enough coumpound themselves to gives mature teams a competitive advantage.

- **quality buys us a trust capital with our users**. They learn to trust our product, and beyond the savings on customer support expenses,
  this trust will also keep our users giving us honest feedback, instead of just trying to blame every defect on our work.
- **zero-defects in your product makes it possible to discover your next product value faster**. Instead of spending our time on the diagnostics
  of our delivery or regression defects, we can clearly see the impact of our delivery on our users, and discover missing requirements instead
  of waiting for customer feedback to reach a critical point to react.
