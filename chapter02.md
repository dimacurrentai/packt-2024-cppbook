# Chapter 2: Programming Paradigms

This chapter begins our descent into the world of Data-Oriented programming. We start from outlining what programming disciplines we have, academically and in the industry. We then go into the details of most common programming paradigms today: imperative, declarative, and functional. We also present the case for low- and high-level programming languages, arguing that C and C++ are very well positioned to let the developer stay close to the hardware and yet code in broad strokes.

By the end of the chapter you will:

* Have walked through the lineage of programming paradigms.
* Understand what we understand as Data-Oriented Programming for the purposes of this book.
* Know -- and, hopefully, appreciate! -- the duality of the term Object-Oriented Programming.
* Embrace that a couple other other programming paradigms are alive and well, and come in handy at times.
* And get a glimpse into what makes C++ so well suited to exercise data-oriented programming in.

In the next chapter we dive deep into the history of programming itself. But let's do a high-level overview first.

## The Academic Approach

For many people programming has started, and remains, most of all, a marvelous craft of pragmatic science that is well worth exploring. Personally, as a kid I found myself fascinated by the idea that a piece of hardware is capable of solving, in seconds, most problems that humanity has ever faced in our entire history. All it takes is to formulate the problem in an unambiguous language, the language that the machine understands: the Programming Language.

Formulating problems in a well-formalized way is itself a great challenge, as intellectually rewarding for many early-days programmers as solving the most elegant puzzles. Furthermore, mastering this art and craft of communicating with the computer in a way it understands opened doors to a lucrative career in the field we all know today as Software Engineering.

Software Engineering has existed as an industry for over fifty years. Around fifteen years ago the branch of Data Engineering branched out into its own discipline. Data Engineering builds on top of Software Engineering, and it has inherited many of the domains that were crystallized over time in Software Engineering. In this book we focus on what is solidly within the intersection of Software Engineering and Data Engineering.

To be frank, C++ as the language is fundamental and low-level enough, so that a pure Data Engineering book about C++ would be quite hollow. Whether we want it or not, a C++ book has to focus on C++, and mastering C++ as the language is solidly within the domain of Software Engineering. At the same time, Data Engineering these days includes building highly-performing low-latency systems about as much as it includes modeling, statistics, and machine learning algorithms. In other words, C++ does enable many of the use cases that Data Engineering experts who are only familiar with higher-level language struggle with.

We should also mention it here, if only in passing, that as of 2024 a novel industry of what we can loosely call AI-Assisted Programming is emerging. This new industry is now in its infancy, although it is advancing rapidly. It is still safe to say that to be an effective AI-Assisted Programmer one has to understand the fundamentals of the approaches used in Software and Data Engineering. Time will tell if this knowledge becomes redundant for the generations of developers that come after us. For now we will assume you are interested in how programming is done in the industry today -- for you would likely not be reading this book if you are not.

After this acedemically laid out landscape of the field, let's dive into the weeds of how the sausage of software is made.

## The Industry

When "talking to the computer" is a hobby, it is most often a single person enjoying their time while exploring and playing around. Friends gathering to hack up stuff together is a common occurrence too, and they would also act as a group of people enjoying a shared hobby. Until software is developed at scale, with a long-term goal in mind, and with business interests at stake, the entire realm often looks more like several people having fun than a fully-formed industry.

The Industry of Software Development is a different world. Certainly, for many people just starting their career, hobby vibes are still in the air. Climbing up on the ladder though, the biggest difference one has to internalize quite early on is that it's not the quality or the beauty of the code that counts; it's something else entirely.

From my twenty years of experience, I would distill these differences to two main points that most distinctly make the Industry of software development stand apart from the hobby of making computers do what the programmer wants. They are:

1. Long-term orientedness on results, and
2. Friendliness with a wide range of developers.

Long-term orientedness on results is essential so that the software product can grow and evolve over many years and many generations of developers. Friendliness with a large range of developers makes the company deliver steadily, with the team composed of people of various backgrounds and seniority levels.

To set the elephant in the room free early on: it is well understood in the industry that an enterprise-grade team of developers in a large company can hardly operate at the same rate of "shipping features" that a lone enthusiast demonstrates by pulling all-nighters. Nothing could be further from the truth. A single highly-enthusiastic individual, and even a few of them hacking together, will outpace virtually every engineering team ever seen in the industry, by leaps and bounds.

All of us have anecdotal stories when a company took a month or two to complete a task that virtually every savvy hacker in their late teens would be able to get done in just a few hours. Growing in seniority in the industry implies, among other things, accepting this not as a bug or as an unfortunate state of the art, but as a feature and as a necessary state of evolution of the phenomena of developing software at scale.

This introduction was necessary to set our journey into various programming paradigms for success. A lone hacker who is enjoying themselves and their craft will hardly operate purely within one paradigm. It is more fun, more effective, and better for one's learning curve to keep inventing creative ways to combine existing paradigms, and even to create new ones as they fit. Nonetheless, various distinct programming paradigms exist today, and it is important to understand them.

In the next sections of this chapter we will cover the paradigms of Imperative, Declarative, and Functional programming, before setting the table for the ultimate quest for Data-Oriented programming.

## Imperative Programming

While it may not be true these days, for those who learned programming decades ago, "imperative" is the "default" programming paradigm.

**Imperative programming is about telling the machine what to do, step by step.** In the early days of developing software, the ability to tell the machine used to imply intimate low-level understanding of how the machine operates under the hood. Before higher-level languages comes the assembly language, and before the assembly language there are machine instruction opcodes.

The C Programming Language is infamous in a unique way. It is commonly described as both:

* As the highest-level language of all low-level programming languages, and at the same time
* As the lowest-level language of all the high-level programming languages.

For the purposes of this book, it would do the justice to mention that C can definitely be understood as a low-level systems programming language. It would be correct to say that C is only a few steps above the Assembly language, in which CPU instructions are described one after another. On the other hand, even though modern-day high-level programming languages have raised the bar significantly, we can also argue that C does qualify for a high-level language. And, of course, C++ ia substantially more high-level than C; oftentimes to the dislike of the folks who prefer their C raw.

Nonetheless, most people who speak C++ understand C quite well, and, whether we like it or not, using C++ effectively implies a certain degree of C-level reasoning and mastery.

It is worth noting that Bjarne Stroustrup famously said that there exists no such language as C/C++. This may not have been the case circa 2003, but it most definitely is the case in 2024. Anecdotally, this means that if your resume cites C/C++, you may want to write them as two separate languages. On a deeper level though, while we will focus exclusively on C++ in this book, your journey will have a few twists of C.

An infamous case of a proficient C programmer who dislikes C++ with passion is Linus Torvalds. With all my love for C++, I have to give this one to Linus. Kernel-level code, first of all, should be straightforward and predictable, and the beauty of C as the programming language is that most C constructs have no side effects.

On the contrary, when coding in C++, the user of the library is very much at the mercy of the maintainer of the library. A benign and seemingly innocent construct can blow up into a huge piece of twisted and convoluted logic. On the bright side, this flexibility often is exactly what we need for the code to be Data-Oriented, so fasten your seatbelts -- that's exactly where the next chapters are going!

## Declarative Programming

The distinction between programming paradigms gets blurry soon, but the line between imperative programming and declarative programming is quite sharp.

**Declarative programming is about describing the desired outcome without specifying the exact steps or their order.** A programming language is a declarative one if it focuses more on helping the developer express their desired end result. A well-designed declarative programming language is the one that the programmer can trust to do the right thing behind the scenes, without having to worry about what exactly will take place, and in which order.

In particular, it is often the case that the order of statements is not important for a program implemented in a declarative language.

A declarative programming language you are already familiar with is the language of `Makefile`-s used by the `make` utility. In the `Makefile` the developer does not have to use if-statements that check whether a certain target is already built in order to start building the next one. The execution framework will take care of the order of "executing" various parts of the "code" written as the `Makefile`.

Such an approach immediately enables powerful features such as, in the case of `make`, parallelism.

Another declarative language that most programmers have had at least some exposure to is the language of **Perl-Compatible Regular Expressions** (**PCREs**). Don't worry if this does not read naturally to you, but many developers will instantly recognize what `/[_a-zA-Z][_a-zA-Z0-9]*/` stands for. It's a C-style identifier:

1. Begin with one of (a union, `[]`):
  * An underscore `_`,
  * A lowercase latin character `a-z`,
  * Or an uppercase latin character: `A-Z`,
2. Followed by zero, one, or more (the `*` at the end of the second set of `[]`):
  * An underscore `_`,
  * A lowercase latin character `a-z`,
  * An uppercase latin character: `A-Z`,
  * Or a digit `0-9`.

When the programmer requests to check whether a provided string does or does not **match** given a regular expression, this programmer outsources the particular implementation of the matching algorithm to the underlying framework. Most of the frameworks will build a state machine ("compile a regular expression"), so that this state machine can then be fed the characters of the input string, one by one, sequentially. It should be noted that a sufficiently sophisticated regular expression may take quite a while to be applied to a long string, and this was the source of many outages on the Internet before.

Looking back from 2024, regular expressions are a special breed of "write-only" languages, the use of which is often discouraged. A "write-only" language is the one whether producing the correct (or seemingly-correct) piece of code is generally not difficult, while reviewing, extending, and debugging it quickly becomes pure hell. In the industry, we like to err on the other side: it is acceptable for the code to take longer to write as long as it is easy to review, maintain, and extend further.

Nonetheless, in addition to the `Makefile`-s, the language or Regular Expressions is a great example of a declarative programming language that is used widely today.
\\I've noticed that we use hyphens as a suffix most of the times. I don't recommend this method of writing. 

Without going too much into details, **SQL**, short for **Structured Query Language**, is also closer to a declarative language than to an imperative one. Most definitely, both the SQL language specification and its various vendor-specific dialects offer plenty of room for imperative code. But, on the fundamental level, when `SELECT`-ing data from different tables that need to be `JOIN`-ed, the developer does not imperatively instruct the database which `for`-loops to run, and in which order. Instead, the "program" written in SQL is descriptive in nature. Unless we go down to the level of DBAs working on optimizing database indexes, the developer would much prefer to not know what exactly is happening behind the scenes. The developer "just" wants to get the result from the database, and the SQL programming language is so popular exactly for this reason: it serves as a great demarcation line between the side that queries the data and the side that is responsible for storing it efficiently.

On a closing note of this section, there is a lot of talk on the Internet these days on whether HTML is a declarative programming language; or whether HTML is a programming language at all. We would encourage savvy readers to stay away from those arguments, since HTML is first and foremost a markup language, not a programming language; it is closer to YAML than to any true programming. Web 2.0 indeed involved a lot of in-browser programming, but it's JavaScript, not HTML, that is the language where programming happens. However, to be pedantic, **CSS**, the **Cascading Style Sheets** markup definition language, fits the definition of the declarative language extremely well, and can safely be considered as such.

## Functional Programming

Many books can be, were, and will be written on Functional Programming. 

Functional Programming can be defined very narrowly, as the ability to "return" a function. Or it can be defined extremely broadly, from the Category Theory principles, outlining why "a monad is just a monoid in the category of endofunctors" on several hundred pages first. Since this book is about C++ and about writing Data-Oriented code, we will refrain from the Category Theory and use simple language.

<details>
  <summary>Just kidding.</summary>

  The "monad is just a monoid in the category of endofunctors" statement, while accurate, is a common joke. It would rank high on the list of explanations that are factually correct but provide literally zero explanatory value. Sorry, we just could not resist inserting this pun into the chapter.

</details>

We need a short intro though, to get to Data-Oriented Programming. Pragmatically speaking, Functional Programming has several characteristic properties. The ones we dive into in the following sub-chapters are purity, immutability, laziness, and, well, monads.

### Purity

When thinking functionally, functions simply are what takes some inputs and evaluates to some resulting value.

There is no such thing as the function _doing_ anything outside evaluating what its resulting value should be. Speaking in more common terms, side effects are prohibited by design of the very functional programming paradigm, for what is a side effect could affect the world in more ways than the function is allowed to.

Functions without side effects are called pure functions. Functional programming is meant to be pure by design.

This is part of the reason why functional programming is not widely used in the industry. Imagine having no way to write anything to any log file during the evaluation process? That's quite a mind-blowing paradigm shift from what most of us are used to.

### Immutability

Programs implemented in a purely functional style do not have variables.

Non-functional programmers, as they see some `the_answer = 42` construct, would immediately think that `the_answer` is a variable that holds the value 42.

Even if the code reads `const the_answer = 42`, for a non-functional programmer, `the_answer` is still a variable. Even though it is deprived from its core functionality of holding a value that can "vary" over time, it's just too natural for most developers to assume it is the variable.  Moreover, if the programmer in question is most familiar with imperative programming, they would generally assume this variable has some type assigned to it at compilation time, and has some fixed memory location assigned to it during runtime.

On the contrary, within the functional programming paradigm, `the_answer` is not a variable. In fact, there is no such concept as "running" a functional program. Instead, the requested _terms_ are evaluated. If `the_answer` is requested to be evaluated, the whole machine will get into motion, looking back to what is required to understand what `the_answer` is.

Upon request, if `the_answer` was already evaluated before, the result of that evaluation can be used directly. If it is an immediate value, like in our trivial example, the evaluation would only take one step. If `the_answer` was defined as "the sum of the first six primes plus one", evaluating `the_answer` would involve determining what the first six prime numbers are, and then summing them up and adding one on top.

### Laziness

Here's a famous joke. A professor is about to start the first language on functional programming. The opening statement is: "Do you have any questions?"

Laziness, or, more formally, lazy evaluation, is the concept that no work is performed until the result of this work is required to materialize. In other words, without a clear request, no work will even begin. This is why Haskell programmers can comfortably reason in infinite lists: the never-ending part of the list does not "exist" in the first place, since knowing how to traverse it is sufficient.

The concept of laziness naturally imposes an entirely different way of thinking of time. For an imperative program, especially when executing on a single CPU core, it is clear to the developer what operation happens before what others. For instance, a trivial program that first asks for the user's name and then prints "Hello, John!", would, naturally, _first_ ask for the user's name, and _then_ print the greeting.

This "before-after" relationship will naturally be preserved when programming in the functional paradigm. But it will not be the order of statements in the code. Rather, the relationship of "before-after" is codified by the "evaluation depends upon" relationship. Ironically, in this regard, functional programming is very similar to declarative programming.

### I/O via Monads

To dive a bit deeper, there is no direct way to "print into the terminal" in the functional paradigm of programming. Clearly, printing into the terminal will alter the state of the world outside this very program, and such an act would be impure by design.

The clever trick used all the time in functional programming is to wrap the logic of "the state split into before and after" into a function that is being invoked on the "before" state and yields the altered "after" state.

For example, by design, in the functional programming paradigm, if there are three functions, `f1`, `f2`, and `f3`, there is no way to guarantee that `f1()`, `f2()`, and `f3()` will be called in this exact order. But if the signatures of these functions are `f1(world_before_f1)`, `f2(world_before_f2)`, and `f3(world_before_f3)`, then `f3(f2(f1(original_world_state)))` would do exactly this: evaluate `f1`, then `f2`, and then `f3`, effectively "passing" the "evolving" world state between their calls.

In programming languages that are functional in nature, there is no shortage of syntactic sugar to make sure this very code will read along the lines of `f1 >>= f2 >>= f3`, in the left-to-right natural order, and with no explicit need to capture the "state of the world" and to pass it along further.

But this is the price to pay. If a piece of functional programming code needs to interface with something external, such as the terminal, or a context of the network call, then this external piece would no longer be "the same" after the interaction, thus breaking the purity requirement. Therefore, some codified version of this externality, such as interfacing with the terminal, or any I/O input, would have to be part of the input for this code, and the now-altered state of this externality will have to be part of the output. In Haskell this mechanism is even called the `IO` monad, look online or ask your favorite AI agent for illustrative examples.

However academic this may sound, it is important these days to understand functional programming, if only as the paradigm. Each of the three concepts captured above -- purity, immutability, laziness -- is often useful, if only as the inspiration for how a certain piece of code can be designed better.

The more senior you will grow, the more likely it is that it will be you who is designing libraries, and then even **domain-specific languages** (**DSLs**) for the broader team. People before you have tried this in most shapes and forms imaginable. Knowing what the distilled wisdom of functional programming is will allow you to stand on the shoulders of giants while making your dent in the field.

Armed with this knowledge on functional programming, we can now go on to Object-Oriented Programming.

## Object-Oriented Programming

The Object-Oriented Programming paradigm, often shortened to OOP, may well be the most misunderstood one of all.

Most programmers today, if asked gently what the OOP is about, would answer along the lines of the "encapsulation, polymorphism, inheritance" concepts, and the `public` / `protected` / `private` access modifiers. If we look at OOP from its early days, nothing could be further from the truth.

The original OOP literally boils down to a few very simple ideas:

1. There exist **objects**.
2. Objects communicate with each other via **messages**.
3. Each object has its **state**, and it's the object alone that is responsible for managing this state over time.
4. Optionally, but importantly: objects process the messages sent to them **sequentially**.

Let's unpack this.

There exist *objects*. One could say they are instances of some classes, but that would undermine the concept. For instance, a _singleton object_ may well outlive the binary, and well-designed **Object-Relational Mappings** (**ORMs**) may well make this extended lifetime of an object transparent to the engineer writing the code. An object exists not in the realm of a particular piece of code, interface, factory, code-level singleton, or even a running binary. An object exists in the space of the business logic of the application. When and how this object is persisted, restored, replicated, etc. should not be the concern of the developer in a true Object-Oriented world. Instead, developers should focus exclusively on correctly implementing the business logic of how objects change their state over time, in response to external events, be they user actions, scheduled tasks, or other triggers.

Objects communicate with each other via **messages**. This means far more than just "objects have private members that only the methods of this object can access". This means that the object is inherently a black box as seen by all other objects. This means that the only way to communicate with this object, if only to get its current state, is to send it a message and then, *asynchronously*, expect a response.

Each object has its **state**, and it's the object alone that is responsible for managing this state over time. This is the easiest point to internalize given the above two. Worth adding is that, although this is not the subject of the book you are reading, mutating the state of the object is a nontrivial problem in a distributed setting. That is, if machines can fail, and/or the network connections between them can be down, ensuring that logically tied objects do not go out of sync is a major problem.

Much like functional programming, with its approach to purity and immutability, helps address some of the problems locally, within the lifetime of an individual binary on a particular machine, the object-oriented programming paradigm offers plenty of ways to ensure consistency over time.

Optionally, but importantly: objects process the messages sent to them **sequentially**. In simple terms this means that the caller, i.e. the side that is sending messages to the object, is not responsible for possible concurrency issues. Just assume each object has its own inbound message queue attached to it. This analogy might be crude, but it drives the point home quite well. Also, all the problems with queues -- such as handling backpressure, batching, flushing, de-duplicating, etc. -- are very visible when the code is implemented within the object-oriented programming paradigm. Moreover, a well-designed framework will offer plenty of powerful ways to solve some of their problems, mitigate others, and alert on what requires human attention.

Thankfully, the original OOP begins taking shape again in the modern world. When a system architect is talking about actor models, event sourcing, and domain-driven design -- they are appealing to the roots of Object-Oriented Programming, even though they may not verbalize it this way specifically. And programming language support to enable this programming paradigm is also improving year by year, so that we may well see the "proper", full-blown OOP to make a comeback.

For the purposes of this book though, the roots and the state of the art of Object-Oriented Programming was important to only be outlined. Data-Oriented Programming is a different beast altogether.

## Data-Oriented Programming

It is difficult to put into one section what the entire book is about. In a nutshell though, data-oriented programming is about -- *while borrowing the most effective parts of the other paradigms* -- enabling the developer to:

* Utilize the power of the underlying hardware, to enable high-performance low-latency applications.
* Avoid expensive abstractions, while still keeping the code as high-level and as manageable as possible.
* Enable the programmer to write the code starting from the data, its state and mutations, as opposed to treating business logic operations as first-class citizens, while viewing data as a "must-deal-with" second-order concept.

Bottom up, data-oriented programming is about structuring the code such that the developer must first of all think of the underlying data, its structure, and representation. Top down, data-oriented programming is a design pattern that forces the system architect to think of data mutations as the source of all business logic, with the code implementing this business logic being the service layer of the data that is being mutated -- not the other way around.

In other words, data, both in its high-level model and in its low-level representation, is the **subject**, not the **object** of the operations that are taking place in the system. Connecting the dots in the way that this mantra becomes the path of least resistance is, in a nutshell, what data-oriented programming is about. And this is what we drill down further in this book.

## Recap

Programming paradigms have existed for as long as Software Engineering, and now Data Engineering, exist as well-versed scientific and applied disciplines.

With the obligatory disclaimer that Data-Oriented Programming is a novel term the meaning of which will be evolving over time, it is a major step in a fascinating direction. In many ways, Data-Oriented Programming and C++ have been created for one another. It's a match made in heaven, and, no matter what the future brings, understanding how to leverage machine-orientedness while remaining in the realm of high-level C++ will likely remain a superpower for years to come. Not to mention it's fun to work with too.

Another way to look at it is that Data-Oriented Programming, when applied to C++, is very much a clever way to bring the best parts of the philosophy of C straight into C++. In other words, coding in a data-oriented paradigm in C++ is the way to have the cake and eat it at the same time. In the chapters to come we will explore how exactly this cake is not a lie, for example.

Also, never forget that programming languages and paradigms exist, first and foremost, to foster long-term collaboration of multiple people of diverse skill levels and backgrounds. Don't be a skeptic right away if you think you know a better way forward. You may well indeed, and it's not about you -- it's about the broader team that will be debugging and extending and refactoring your code later on. The market of helping individual developers have fun while hacking is just too narrow -- hence most books you'd encounter would instead focus on how to be productive and effective as part of the broader team. This book is no exception, although you having fun is indeed one of our goals while writing it!

Now we are ready for the next chapter, in which we truly begin going deep that Data-Orientedness stands for. And this begins with understanding how memory works.
