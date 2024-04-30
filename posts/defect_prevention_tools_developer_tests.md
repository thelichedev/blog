Title: Defect prevention tool: Developer Tests
Date: 2024-05-01
Tags: software quality, tools

Let's continue our exploration of the defect prevention tools with another classic: **Developer Tests**.

<!-- end-of-preview -->

## What are developer tests ?

Developer tests consist of suites of automated tests writtent by the developers as an integral part of their development process.

Typically, these suites rely on specific test tools and frameworks (such as JUnit, Mocha, or Cucumber).

Developer tests are executed during the development phase, and they are also often run on a Continuous Integration server responsible for testing new developments before delivery.

These tests are example-based: developers provide examples of the expected behavior of their code within specific contexts.

Sometimes, developer test tools are utilized to create other types of automated tests, including:
- Automated tests authored by individuals other than developers, such as QA or support teams.
- Generative tests written by developers but [based on properties](https://propertesting.com/) rather than specific examples.
- Performance or stability regression tests.

In this discussion, I'll primarily focus on their most common usage by developers, relying on example-based tests.

### Expert or Average usage ?

Developer tests represent another area where the disparity between best-in-class and subpar projects is striking.

Similar to previous discussions, **I'll focus on real-world applications I've encountered**, which span a broader quality spectrum than for Static Types.

In my experience, while many professional projects I've engaged with exhibited inadequately constructed test suites—lacking clear intentions
and suffering from neglect over time—there have been instances where I've had the privilege of contributing to projects boasting well-crafted test suites,
and maybe to craft some of these myself.

The advantages conferred by robust test suites far outweigh those of their weaker counterparts, particularly in terms of defect prevention.

Just like with Static Types, there exists a wealth of literature on Developer Tests.
However, the landscape of recommended best practices for crafting maintainable tests is even more diverse,
presenting a challenge when assessing a team's proficiency in testing practices.

One glaring observation in the majority of web articles discussing how to craft effective tests,
is the conspicuous absence of a clear objective for writing tests.
Authors often dive into a lengthy series of recommendations without clarifying their overarching goal in writing tests.
Needless to say, following such advice can do more harm than good if your implicit objectives differ from those of the author.

## Quality evaluation grid

In their most rudimentary function, Developer Tests serve to prevent defects during development
or just before delivery when executed on a Continuous Integration server.

However, in projects boasting top-tier test suites, especially those developed using Test-Driven Development (TDD),
I've noticed a recurring phenomenon: **the test suite can uncover defects even before development begins**.

When an entire project boasts a comprehensive executable product documentation in the form of a test suite,
developers consistently find they can identify flawed feature specifications or implementation choices,
with a brief exploration relying on the existing test suite.

The practice of TDD amplifies this effect by guiding developers to articulate their expectations for the new feature explicitly.
In doing so, they explicitly detail what would otherwise be implicit product and operations-related micro-choices.
Consequently, an inability to formulate satisfactory tests for a new feature often serves as a clear indicator of intrinsic defects in the feature or its implementation.

While this phenomenon may be more closely associated with TDD than with Developer Tests per se,
the concept of tests as executable requirements and exploration grounds offers significant value in identifying defects before substantial development efforts are undertaken.

Additionally, I've discovered that tests serve as an invaluable opportunity to consolidate and integrate product knowledge.
Well-crafted tests essentially act as an executable and verified (up-to-date) product documentation,
often serving as the sole comprehensive product documentation available.
This is especially significant as Product Manager requirements often overlook the myriad micro product choices made during development.

Through meticulous maintenance of the test suite, some of my teams have consistently uncovered missing product value
and identified underlying shared product concepts among seemingly disparate features.

Therefore, it is my opinion that well-maintained **Developer Tests can serve as one of the few tools at our disposal for uncovering missing product value before delivery.**

|                        | delivery failures | regression failures | missing value |
|:-----------------------|:-----------------:|:-------------------:|:-------------:|
| **before development** |         👾        |          👾         |      👾       |
| **during development** |         👾        |          👾         |      👾       |
| **before delivery**    |         👾        |          👾         |      👾       |
| **before user impact** |                   |                     |               |
| **after user impact**  |                   |                     |               |

## Defects taxonomy


In a sense, regardless of the type of defects you aim to catch with your test suite, you can always devise a Developer Test
to identify them—provided you understand precisely how these defects manifest.

Such tests are known as regression tests: they are designed to ensure that a previously identified and rectified defect does not resurface in your product.

They serve less as a means to prevent or discover defects, and more as a method to document and contain defects after they have already materialized.

However, by the time such defects are addressed and regression tests are implemented, the project has typically already incurred the cost associated with those defects—at least once.

In the subsequent discussion, I will clearly delineate between **regression tests** crafted to contain specific known defects, and the use of Developer Tests as a tool for **defect prevention**.

I'll also differentiate between the impact of Developer Tests **within or outside of the practice of Test-Driven Development** (TDD).
It's my belief that their efficacy as a defect prevention tool can significantly differ depending on whether developers adhere to TDD principles or not.

### Algorithm bugs

![Algorithm bugs](/assets/defects_taxonomy/algorithm_bugs.svg)

Developer Tests excel particularly in preventing Algorithm Bugs. This is where their efficacy truly shines.

While they may be slightly slower and less developer-friendly compared to Static Types,
**Developer Tests still excel at identifying Contract Bugs**.

They even surpass Static Types in capturing high-level contract violations, such as constraints between function arguments or specific data formats.
While the errors they produce may be less explicit than type errors (a point that is debatable, especially for those familiar with cryptic TypeScript errors),
Developer Tests provide a robust guarantee that dependencies are invoked correctly because you're executing the actual contract (**including possible undocumented constraints**).

However, heavy usage of mocks in untyped languages can compromise this guarantee. Since there's no assurance that mocks adhere to the same contracts as actual dependencies,
Contract Bugs might slip through. Most reputable literature on testing recommends minimizing the use of mocks whenever possible, a stance I fully support.

**Algorithm Bugs are where Developer Tests truly shine**. In TDD, every aspect of the algorithm is meticulously crafted to make tests pass,
resulting in clear and deliberate algorithm behavior. Even if you're not following TDD, you can mitigate risks by using coverage tools to identify untested parts of your algorithm.

Moreover, Developer Tests serve as an invaluable tool for identifying potential Algorithm Bugs in production.
By capturing suspicious execution contexts in production, writing tests based on these contexts, and observing code behavior, you can determine if the problem can be replicated.

**Do Developer Tests effectively prevent Incomplete Algorithm Bugs? Well, not flawlessly, but they do assist in tracking which use cases have been implemented**, which is significant in itself.
In this regard, Developer Tests heavily rely on the developer's testing discipline.
Strong testing practices naturally ensure thorough coverage of all legitimate execution contexts, as developers systematically explore and write tests.
However, if developers are less acquainted with testing practices, they may overlook many use cases, resulting in inadequate test coverage.
If they're less familiar with testing practices however, they'll probably miss out on a lot of use cases and tests will not enforce a good coverage by themselves.
Other types of automated tests, such as[property based tests](https://propertesting.com/), aim to alleviate this issue.

In terms of **Validation Bugs, Developer Tests have limited utility**. While developers can write tests to document validation code,
the example-based nature of these tests prevents them from guaranteeing that all invalid contexts are caught by the validation code.

On the other hand, Regression tests are instrumental in documenting and containing identified Validation Bugs.

In my own professional experience, **Developer Tests have consistently proven to be the most transformative tool for Preventing algorithm Bugs**,
easily surpassing the impact of Static Types, despite their frequent misusage.
By functioning prior to delivery, they substantially reduce the cost associated with such bugs.

### State machine defects

![Algorithm bugs](/assets/defects_taxonomy/state_machine_defects.svg)

Among the desirable properties of a good test suite are tests Isolation and Simplicity.

Developer Tests typically focus on verifying individual state machine transitions in isolation,
eschewing more complex and comprehensive transition scenarios.

The resulting context expected in the test suite of a transition may never be tested as part of the inputs of another transaction.

Complex test scenarios, intended to assess state machines on a global level, are often implemented at higher levels of the test suite, such as end-to-end tests.
However, these complex scenarios tend to become brittle and exceedingly costly to maintain, rendering them an ill-advised practice.

As a consequence, **Developer Tests are primarily ineffective in preventing State Machine Bugs**.

However, Regression tests can provide significant value in containing identified State Machine Bugs.

### Time defects

![Algorithm bugs](/assets/defects_taxonomy/time_defects.svg)

Being Fast and Repeatable are among the most desirable properties of a robust test suite.

As a consequence, test suites are typically designed to execute scenarios that **avoid time-related Performance Defects**,
notably by refraining from calling real-world production services and only instantiating the minimal amount of data necessary.

Time emerges as one of the most crucial external dependencies to isolate in a test suite,
resulting in tests that will **be inadequate at detecting Scheduling Defects**.

Again, Regression tests can play a significant role in containing identified Time Defects.
Nonetheless, it's often preferable to segregate performance regression tests into their own test suites to prevent them from affecting the speed of the primary product documentation suite.

### Costs defects

![Algorithm bugs](/assets/defects_taxonomy/cost_defects.svg)

For similar reasons, test suites typically refrain from incorporating all the sources of cost defects encountered in real-world scenarios.

As a result, **Developer Tests are ineffective in preventing Cost Defects**, even when utilized as Regression Tests.

### Product defects

![Algorithm bugs](/assets/defects_taxonomy/product_defects.svg)

It's somewhat ironic that developers who primarily write tests to uncover defects
often overlook the importance of crafting test descriptions that accurately document the product value of their code.
Consequently, they miss out on the opportunity to detect product defects through their tests.

Their test descriptions may be simplistic, like "my function should work" or "returns 5 when called with 42,"
rendering their tests ineffective in externalizing the product knowledge of their code.

Conversely, well-crafted, product-oriented tests have consistently proven throughout my career to **be effective in preventing Product Defects**,
as early as the initial exploration phase before the development of a new feature.

By meticulously documenting even the smallest implicit choices made by developers during their work,
these tests serve as an invaluable tool for identifying contradictions in requirements, missing code to handle legitimate scenarios, and more.

On the best projects I've been a part of, I've been able to proactively develop code to support upcoming use cases before they were officially identified,
simply by exploring the emerging concepts in my production code through the test suite.

There is something magical when a senior developer, who lacked prior experience with a robust test suite,
returns and shares, "Yesterday, as I began work on a new feature, I was unable to write any meaningful tests.
It dawned on me that our new feature was in fact useless. I promptly revisited the product manager, and together, we sidestepped two weeks of unproductive work."

### Safety defect

![Algorithm bugs](/assets/defects_taxonomy/safety_defects.svg)

I've never witnessed Developer Tests directly impact safety.

While developers can incorporate safety requirements into their test scenarios or utilize regression tests to contain past safety defects,
Developer Tests alone lack the capability to enforce safety measures.

Some might argue that by eliminating common algorithmic errors such as off-by-one mistakes or buffer overflows,
tests indirectly contribute to enhancing the overall safety of the product.

However, **it's challenging to view Developer Tests as a sufficient tool for preventing Safety Defects.**

## Developer tests feedback speed

Being fast is indeed one of the most valuable yet frequently underestimated properties of a test suite.

Ideally, you should strive for a reload-execute time of around 1 millisecond for unit tests and less than a second for higher, more integrated tests.

However, maintaining such speed on large projects can be challenging, especially in teams with heterogeneous testing practices.
In practice, many of the test suites I've encountered were too slow.

Developers with limited experience in testing practices also tend to prioritize test coverage and extensiveness over test execution speed.
This preference often leads to the creation of test suites that take so much time to run,
that they are reluctantly executed only at the last possible moment before delivery.
Consequently, they become essentially useless as a cost-reducing practice.

It's ironic that Static Types, by introducing a compilation step, often negatively impacts the feedback speed of other practices like Developer Tests.
This could be seen as an argument against adopting Static Types.

Despite all this, **Developer Tests can still offer one of the fastest feedback speeds available to developers**.
While they may be slightly slower than Static Types, they cover a much broader range of defects, making the extra few milliseconds well worth it.

## Developer tests cost

The story of bad test suits usually goes this way: developers, armed with minimal formal knowledge of test writing challenges,
start writing tests because "everybody knows you should test your code".

However, lacking clear objectives, they develop their test suite without a defined purpose:
- Should the emphasis be on product documentation, defect prevention, or exploration?
- Should tests utilize mocks or perhaps automockers?
- Is there a preference for unit tests over integration tests, or vice versa?
- Should test code adhere to the same principles as production code, such as DRY (Don't Repeat Yourself), encapsulation, or the use of factories?

The resultant test suite typically mirrors the complexity of the production codebase, becoming a maintenance nightmare and a breeding ground for regressions,
offering little discernible value to the project.

Initial enthusiasm for testing, fueled by rudimentary online articles, quickly wanes as the test suite becomes sluggish and fragile,
plagued by unstable tests that erode team confidence and burden maintenance efforts.

After grappling with test suite maintenance for a time (often just weeks or months), teams find themselves disillusioned,
abandoning tests altogether with statements like "tests don't work" or "we can't afford test maintenance."

Our profession's fundamental mistake was to assume that developers struggling to write maintainable production code would effortlessly produce maintainable tests.

In my experience, crafting maintainable tests is actually more challenging than writing production code.

**When executed poorly, developer tests are perhaps the most costly tool in a project's arsenal.**
I've witnessed teams deplete their entire development budget on futile testing endeavors.

It's also crucial to acknowledge that "higher-level" tests, which integrate more components and potentially involve external services such as a real database,
as well as execute more complex scenarios, are significantly more costly to write and maintain than basic unit tests.

Many teams make the mistake of favoring fewer component or application-level tests in an attempt to reduce the amount of test code
and to execute tests "closer to the reality of production." However, this approach has consistently proven to be the most ineffective and costly approach to test maintenance.

- Writing high-level tests on a codebase lacking in unit tests, where basic Algorithm Bugs still exist, often leads to spending a lot of time debugging unstable high-level tests.
- The complexity of high-level scenarios makes it harder to understand the expected product value of the tests.
- Writing maintainable end-to-end UI tests requires even more skill than unit tests. If a team is not already proficient at writing unit tests, they are unlikely to achieve meaningful results with higher-level tests.

An example of this common mistake can be seen in the recent resurgence of the "diamond-shaped test stack,"
which opposes the prevailing "pyramid-shaped test stack." Some authors claim that diamond-shaped test suites
are superior to pyramid-shaped test suites without providing clear objectives in writing tests
or explaining the trade-offs in preventing different types of defects with their approach.

Overall, while higher-level tests (often called integration, component, or application-level tests) can be useful in small, controlled amounts,
Overall, higher level tests (often called integration or component or application level tests), while useful in a small controlled amount,
they are often too costly to maintain and make understanding defects much harder.
Therefore, **higher level tests are usually not worth it unless the team already has good unit test coverage** that has eliminated all basic algorithm bugs.

Even in projects boasting well-maintained test suites, the cost of test maintenance should not be underestimated.

Beyond mere time or effort, I've found it to be more of a matter of design, reflection, and discipline.

Crafting and upholding efficient tests demands ongoing vigilance regarding the evolution of the test suite,
thoughtful consideration of the established testing objectives, and a wealth of experience in the nuanced practices of test maintenance—practices which,
in my view, diverge significantly from those applied to production code.

**Developer tests represent the tool that demands the highest level of experience and reflection to wield effectively.**

## Conclusion

Delivery quality impact:

|                        | delivery failures | regression failures | missing value |
|:-----------------------|:-----------------:|:-------------------:|:-------------:|
| **before development** |         👾        |          👾         |      👾       |
| **during development** |         👾        |          👾         |      👾       |
| **before delivery**    |         👾        |          👾         |      👾       |
| **before user impact** |                   |                     |               |
| **after user impact**  |                   |                     |               |

Defects taxonomy coverage:

| Defect type               | Coverage          |
|:--------------------------|:------------------|
| **Algorithm bugs**        | yes |
| **State machine defects** | regressions |
| **Time defects**          | regressions |
| **Costs defects**         | - |
| **Product defects**       | yes, for product oriented tests |
| **Safety defect**         | regressions |

Speed: **fast**

Cost: **high, with high maintenance risks**

Time for a confession: I am a practitioner of Test-Driven Development (TDD).
I firmly believe TDD to be the fastest approach to delivering tangible product value to users.
For me, the notion of delivering professional software without TDD is inconceivable.
You're welcome to practice differently, but personally, I rely on TDD because I don't feel confident enough to work without it.
Consequently, on projects where I have the liberty to choose, test coverage tends to reach 100%.

That being said, **I do not view Developer Tests as an optimal defect prevention tool**.
In truth, I do not write tests with the primary objective of preventing defects; if it does have that effect, it's merely a welcome bonus.

**I firmly believe that writing Developer Tests solely to "find bugs" is a futile endeavor.**

As evident from the analysis above, Developer Tests primarily serve to prevent basic Algorithm Bugs—the most trivial yet abundant type of defects.
While they undeniably offer immense value in this aspect, it speaks more about the limitations of the programming languages and tools we commonly employ.
These tools often make it too easy to make simple algorithmic mistakes and provide insufficient valuable feedback.

While good product-oriented tests hold significant potential in preventing Product Defects, the high cost in terms of discipline and experience
makes it challenging to consider Developer Tests as a universal solution to Product Defects.

The primary drawback of Developer Tests as a defect prevention tool lies in **their limitation to testing only scenarios that developers have considered**. 
Unfortunately, most defects arise from situations that developers did not anticipate.
However, tests can still aid experienced developers in keeping track of the scenarios they have considered, which is a notable benefit.

Also, one of the most classic rookie mistakes when writing tests is to never witness the test fail.
The adage goes, "never trust a test you've never seen fail." A test that has never failed may, in fact, be testing nothing at all.
While this may seem like a remarkably stupid oversight, it remains a recurring issue even for experienced developers,
vividly illustrating the ongoing challenge of utilizing Developer Tests as a defect prevention tool.

**Developer Test, by design, are practically useless to prevent any of the most costly type of defects**.

An aspect of Developer Tests that significantly impacts quality, but hasn't been discussed yet, is Developer Confidence.
A well-covered test suite instills developers with the confidence to refactor their production code
and manage knowledge churn within their projects effectively.

The difference in knowledge churn between projects where developers have this confidence and those where they're hesitant to make changes is stark.
In the worst-case scenarios, developers are so apprehensive about altering existing code that they resort to duplicating entire folders of code to avoid potential risks,
exacerbating long-term issues.

However, in practice, this confidence is often illusory. As previously mentioned, Developer Tests are unable to capture most types of defects
that could arise from code refactoring. In fact, even for Algorithm Bugs, it's common to witness Developer Tests failing to detect regressions.
Nevertheless, despite being based on an illusion, gaining this confidence can still yield important benefits.

**While not always grounded in reality, providing developers with the Confidence to Refactor may be the most significant positive effect of Developer Test on code quality.**

In conclusion, I view Developer Tests as an exceptional tool for detecting product defects at the earliest possible stage, provided they are utilized correctly.
However, most test suites crafted solely for defect prevention purposes often fall short of achieving meaningful results, indicating that tests are not universally effective as a defect prevention tool.
Nevertheless, the illusion that they are effective plays a crucial role in the overall improvement of software project quality through Developer Tests.
