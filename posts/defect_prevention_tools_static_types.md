Title: Defect prevention tool: Static Types
Date: 2024-03-05
Tags: software quality, tools

Now that I've explained my two main [evaluation](/team_quality_maturity.html) [frameworks](/defect_taxonomy.html) for software development quality,
it's time to delve into the assessment of prevalent defect prevention tools.

While each tool exhibits unique strengths, it's crucial to acknowledge the associated costs, some of which may surpass initial expectations.

Understanding the potency of these tools empowers teams to invest in solutions that efficiently detect delivery or regression defects,
ensuring comprehensive coverage of the defects taxonomy at an optimal cost.

Let's begin our exploration with one of the foundational tools: **Static Types**.

<!-- end-of-preview -->

## What Static Types ?

Despite ongoing debates surrounding type systems—whether static or dynamic, strongly or weakly typed—every software project inherently adopts a type system of some kind.

In my professional experience spanning a decade with C/C++, followed by two years with TypeScript, and intermittent exposure to Java (primarily on Android), 
I've encountered different implementations of static type systems. 

There are others, more advanced Statically Typed languages (Ada, FORTRAN, Haskell, OCaml, Elixir, etc.)
I've read books about them, I've played with some on personal projects, but I've never used them on a professional project.

However, for the purpose of this discussion, **I'll focus on the static type systems prevalent in languages such as Java, C++, and TypeScript,**
as they constitute the primary tools used in a majority of software development endeavors. 

### Expert or Average usage ?

There is a plethora of literature on optimal utilization of type systems, including sections on best practices and advanced techniques. 
However, there's often a notable absence of guidance on common pitfalls or anti-patterns (which already tells us a lot about these tools and the damage they can yield in innocent hands).

In our exploration, it's crucial to differentiate between two distinct modes of usage:

1. **Ideal, "Best Practices" Usage**: utilization by a hypothetical team of impeccably skilled developers,
implementing solutions with flawless adherence to established norms and principles.

2. **Concrete, Everyday Usage**: usage out there in the real-world, where teams consist of a diverse mix of experience levels,
including relative novices or [moth programmers](/moth_programmer.html).

I'll primarily focus on the latter, and the tangible advantages an average "real life" team can derive from static type systems.

I'm sure a team of experts programmers might undoubtedly achieve more with these tools, at a reduces costs, but it's important to maintain a realistic perspective.
The reality is that most teams are not composed solely of experts;  in fact, many would be fortunate to include just one.

Therefore, **I'll focus on the concrete usage I've seen in the field**,
with occasional insights where I think concrete usage differs from ideal usage.

## Quality evaluation grid


While literature on Type Driven Design offers tantalizing insights into this realm, the practical application within real-world development environments often falls short. The discrepancy between theory and practice underscores the importance of considering the prevailing maturity levels and skillsets within development teams when evaluating the potential impact of static types on product discovery.

Obviously static types are a developer's tool that only impacts code before delivery, as most static types systems cease to operate at runtime.

Static Types are designed to detect bugs during development, and prior to delivery—in your CI chain for example.

As such they are ideally positioned on the delivery axis, most importantly preempting bugs before they can impact users.

Static types are well known for catching defects in new code, but also for providing a regression safety net across entire projects.

However, can they extend beyond bug detection to uncover value gaps within the product itself?

In theory, the principles of Type Driven Design suggest such a possibility.

Yet, in my professional experience this potential remains largely untapped,
due to the poor discipline and expertise of most teams using types in professional projects.

I've yet to witness a project where meticulous exploration of the type system led to meaningful product discoveries.

|                        | delivery failures | regression failures | missing value |
|:-----------------------|:-----------------:|:-------------------:|:-------------:|
| **before development** |                   |                     |               |
| **during development** |         👾        |          👾         |               |
| **before delivery**    |         👾        |          👾         |               |
| **before user impact** |                   |                     |               |
| **after user impact**  |                   |                     |               |

## Defects taxonomy

### Algorithm bugs

![Algorithm bugs](/assets/defects_taxonomy/algorithm_bugs.svg)

**Contract bugs** are what the Static Types are designed for.
They are probably the fastest tool to catch those bugs, and if your project is fully typed, they will provide a good constant coverage.

Unfortunately they are also not sufficient to catch all those defects.
While they excel at ensuring data shapes conform to predefined contracts, they fall short in enforcing more nuanced constraints or invariant properties.

Consider the following scenarios:

**Complex Data Shape Enforcement**: Despite the flexibility of modern typed languages like TypeScript, enforcing intricate data shape requirements, such as date formatting or list integrity, remains a challenge. 
While theoretically feasible, practical implementation is rare due to its complexity and the lack of familiarity among development teams.
- Want to enforce that in `addYear(dateString : string, count : number)`, `dateString` should use a `YYYY-MM-DD` format ?
- Want to enforce that the `sort(list)` function return a list comprised of exactly the same values as its argument ?

**Invariant Property Enforcement**: Enforcing constraints like `min <= max` or delineating the behavior of unique identifiers like `userId` is not come easy.
While again conceivable, achieving such granular enforcement requires considerable effort and often exceeds the expertise of most developers.
- Want to enforce that `min <= max` in `{ min: integer, max: integer }` ?
- Want to say that `userId` is a `string` but you can't do stringy stuff with it since, well, in fact it's an unique identifier, and
  you can only compare it with another `userId` ?

Static types, while invaluable for catching contract bugs, actually suck at identifying **Algorithm bugs** within code.

`function add(a : number, b : number) : number { return a * b; }` is perfectly fine for types, and also totally wrong.

Static types inherently struggle to identify algorithmic bugs, such as off-by-one errors or inverted conditions, which can evade type checking altogether.

The most common joke about this is the famous sentence "If it compiles it works". Which, of course, is total bullshit.
(Not sure the joke is not lost on most static types enthusiasts though).

Static type are almost useless for **Incomplete algorithm bugs** too.

While static types may flag missing check conditions in certain constructs like `switch` statements (lol who still use `switch` statements in 2024 ?),
their efficacy diminishes in complex scenarios where the full spectrum of legitimate values is not adequately considered.

The absence of static types at runtime precludes their usage for preventing **Validation bugs**,
leaving developers reliant on types integration with auxiliary tools like JSON-Schema validation or custom runtime checks.
In an era dominated by interconnected software systems, this is totally archaic.

In essence, **static types primarily serve as guardians against fundamental contract bugs** pertaining to data shape conformity.
However, their utility wanes when confronted with the more complex challenges posed by algorithmic intricacies, incomplete logic, and runtime validation.

### State machine defects

![Algorithm bugs](/assets/defects_taxonomy/state_machine_defects.svg)

In theory, with Type Driven Design, types can help you design a robust state machine.

However, in practice,I've never seen a team reach this level of practice.

Most statically typed languages default to permissive default types, lacking the granularity required for robust state machine representations.
For example, union types are still unfamiliar terrain for many developers.

The widespread adoption of automatically-generated, globally-shared types throughout codebases hampers attempts at nuanced state machine design.
These all-inclusive types, while expedient for generic usage, lack the specificity necessary for intricate state transitions and constraints.

### Time defects

![Algorithm bugs](/assets/defects_taxonomy/time_defects.svg)

Guess what ? Yup. Useless.

### Costs defects

![Algorithm bugs](/assets/defects_taxonomy/cost_defects.svg)

Useless ? Yup yup yup.

### Product defects

![Algorithm bugs](/assets/defects_taxonomy/product_defects.svg)

At the very best types can serve as a rudimentary means of documenting the product value embedded within the code.

In practice, you can't expect developers that don't know how to express business value in their code,
to fare much better in their types. Since they won't even identify their types as an opportunity to document the product.

### Safety defect

![Algorithm bugs](/assets/defects_taxonomy/safety_defects.svg)

While advanced type systems, such as those found in languages like Ada, have the potential to provide safety checks on basic operations,
the practical impact of static types on safety-related defects remains limited.

Despite theoretical assertions, empirical studies consistently highlight the negligible safety-related advantages conferred by static type systems.

Static types offer scant mitigation against safety defects within software projects.

## Static types feedback speed

For a defect prevention tool, feedback speed holds significant importance.

Among the array of common tools, Static Types stand out for their rapid feedback capabilities.

Most modern IDE seamlessly integrate with type checkers, facilitating immediate feedback as code is written, eliminating the need to wait for a full compilation cycle.

## Static Types cost

The cost associated with static types may not always be readily apparent, particularly for developers having spent their entire careers in projects utilizing a single,
statically typed programming language.
The true impact however, is often underestimated by many in the development community.

I've started my career with 10 years of statically types languages, ending with some pretty heavy Object Oriented Programming code
(or as I like to call it: "Classes Oriented Programming").

Then I switched to dynamically typed languages. Now I'm coming back (under duress) to statically typed languages.

I've seen both sides of the coin.

**First there is the tooling cost**: most of those tools introduce significant overhead in terms of tools maintenance and build time.

This could seem to be negligible in our day and age, but this has a big impact on all the ecosystem:
- **increased build times overhead**, inducing a loss of focus as the developers wait for code compilation to complete.
- it also leads to the heavy use of compilation caches, with the inherent **confidence issues** (Did my compiler see my codes changes ? Had it finished compilation when I did my last exploration ?)
- which combine with **increased reload times** to result in slower development cycles as the developers develop full-reload/cache-clean habits to make sure they run the last version of the code.
- commpilation-intensive workflows also **inflate CI/CD execution durations**, prolonging deployment cycles and hindering collaboration on pull requests, and the swift dissemination of hotfixes.

Drawing from personal experience with integrating TypeScript into CI/CD pipelines, the tangible repercussions of static type adoption became apparent.
The effort invested in maintaining pipeline stability amidst increased compilation times, coupled with the resultant escalation in CI/CD execution durations and associated costs, underscored the substantial burden imposed by static type tooling.
Today most of our CI jobs spend more time compiling Typescript than actually running tests.

**Secondly, there's the development cost**, notably the complexity introduced by static types.

You see, most Static Types systems start out fine and simple. But then, as the developers require always more functionality, the complexity creeps in.
Ironically, [many type systems ultimately become Turing-complete](https://github.com/microsoft/TypeScript/issues/14833),
enabling developers to write programs within the type system itself, complete with loops and conditionals.

However, these programs in types world have little to no value for customers as static types typically vanish at runtime.

Enthusiastic developers may invest significant time fooling around with the type system, creating elaborate constructs in the name of "type safety",
while delivering zero customer value.

I've witnessed this phenomenon frequently. Static type systems lack guardrails, allowing developers to indulge in excessive complexity without tangible benefits.

Another common cost is the added complexity of type hierarchies and coupling. Untyped languages typically rely on function call hierarchies,
but static types introduce in addition complex type hierarchies (all the usual interfaces/implementations, inheritance, factory of factory of hammer bullshit)
and coupling mechanisms (let's use a tool to derive types from our database schema, then let's make those whole types global and use them everywhere in all our modules).

Even in contract-oriented type systems like TypeScript, poor practices prevail, perpetuating the proliferation of tangled type hierarchies and coupling.

**Thirdly, there's a significant opportunity cost:** static type systems can prohibit the implementation of certain highly reusable code patterns.

While one might argue that code patterns impossible to express in, say, TypeScript, might be inherently complex and difficult to maintain, my experience suggests otherwise.
Many invaluable software composition patterns become impractical or outright impossible to implement with static types.

For instance, TypeScript documentation explicitly discourages [Point-Free Programming](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-func.html#point-free-programming),
citing poor support within the language. Similarly, functional wrapper composition suffers from limited support.

Teams reliant on static types often find themselves constrained, unable to employ these powerful programming styles.
Consequently, developers may remain unaware of these patterns, further limiting their toolkit for designing maintainable code.

This limitation stands in stark contrast to the purported goal of static types to enhance maintainability.
In reality, static types can inadvertently stifle developers' productivity and hinder the implementation of highly maintainable code.

A prime example is statically typed dependency injection tools, which often devolve into maintenance nightmares.
Meanwhile, patterns like functional wrapper composition, known for their maintainability benefits,
become nearly impossible to implement within the confines of static types, perpetuating a cycle of limited innovation and constrained code design.

**Overall, I'd rate the cost of static types as high,** ranking amongst the highest among the common defect prevention tools I've encountered in my professional career.
This assessment is compounded by **a significant risk factor**: static types necessitate a high level of expertise and team discipline to mitigate their associated costs effectively
—a proficiency that many teams lack.

## Conclusion

Delivery quality impact:

|                        | delivery failures | regression failures | missing value |
|:-----------------------|:-----------------:|:-------------------:|:-------------:|
| **before development** |                   |                     |               |
| **during development** |         👾        |          👾         |               |
| **before delivery**    |         👾        |          👾         |               |
| **before user impact** |                   |                     |               |
| **after user impact**  |                   |                     |               |

Defects taxonomy coverage:

| Defect type               | Coverage          |
|:--------------------------|:------------------|
| **Algorithm bugs**        | only basic Contract bugs |
| **State machine defects** | - |
| **Time defects**          | - |
| **Costs defects**         | - |
| **Product defects**       | - |
| **Safety defect**         | - |

As we can see Static Types globally suck at finding but the most basic of basic bugs.

Speed: **fast**

Cost: **high, with high maintenance risks**

As I hope is made evident, **Static Types are in fact a poor defect prevention tool**.

While they excel at catching basic contract bugs, other tools like automated tests can do that too, with minimal differences in speed and cost.

Let's face it: contract bugs are often the simplest and cheapest defects to address in software projects.
They're the low-hanging fruits that can be easily picked.

However, the belief that static types alone ensure the delivery of "working code" reflects 
**a somewhat naive understanding of software defects** and the complexities of delivering valuable products.

Certainly, having a clean code base free of basic bugs is beneficial, allowing developers to focus on tackling more intricate issues.
But there are alternative defect prevention tools that offer better coverage at a similar cost.

Moreover, there's a real risk that **static type systems can fall victim to the same bad practices as production code**
—over-design, complexity, and so on— you can see the types system as another opportunity to shoot your maintainability in the foot.
One that has absolutely no value whatsoever for your customers.

In my professional experience, the teams working on statically typed OOP codebases often struggled the most with delivery quality.
Many of these projects suffered from unmanageable code bases burdened by poorly designed type hierarchies.

Ironically, despite heavy reliance on static types, these teams were frequently plagued by defects directly impacting end-users.
