# Chapter 0: Hello, C++!

Welcome to this book!

This first chapter is introductory, but don't gallop over right away just yet. Because:

* If you have used C and/or C++ a while back, but were hesitant to take a stab at the modern-day version of it, this chapter will first and foremost be of encouragement. Because modern-day C++ is both more type-safe and more fun to work in than it has ever been. Including cross-language integrations, where I'd argue that befriending C++ with Python — the language that is arguably most popular for data processing today — is easier than most other cross-language integration.

* If Python or JavaScript or Golang were your languages of choice, by the end of this chapter you will be pleased to learn that in the world of C++ a lot of intricacies of toolchain versions and package management can be out of scope of daily work. The C++ Standardization Committee makes sure that the evolution of the language is slow, incremental, and backwards-compatible in the vast majority of cases. Standard compliance is high. Whether you are on a Mac, Windows, Linux, or on a more niche platform, chances are, C++17 is fully supported by the default toolchain available out of the box, and C++20 is either already there or is one installation command away. Also, C++ has literally no package management hell, although this can arguably be attributed to the fact that people hardly need custom packages for C++.

* If you are just starting your journey into programming with data, this chapter will serve as a gentle push that C++ is not that much more difficult than Python to operate. Surely, it's a compiled language, from which it follows immediately that means that no fully-functional REPL shell is available right away. But compilation is fast these days, and it doesn't take much configuration to make iterations swift. As a matter of fact, an end-to-end iteration of edit - build - run - repeat is often faster in C++ than in other languages, especially as data [re-]load times and output presentation are factored in.

* If you are not at all familiar with C and/or C++, the next few pages will hopefully convince you to give the language a shot.

* And, last but not least, as of 2024 it is demonstrably straightforward to get started with C++ even if the only instrument you have access to is the browser!

## Conventions

A few conventions for this book. Code snippets come in multiline blocks of fixed-size font, such as:

```
int square(int x) {
  return x * x;
}
```

When a command is meant to be run in the terminal, it is prefixed by a dollar sign. Unless they are C++ directives such as `#include`, pound signs are comments.

```
$ cat >binary.cc <<EOF
#include <iostream>
using namespace std;
int main() {
  cout << "Hello, C++!" << endl;
}
EOF

$ g++ -o binary binary.cc && ./binary
```

For the purposes of making it easier to follow the code snippets, they are designed to be as free of clutter as possible. In particular, this implies that:

* The `#include` directives are omitted for clarity most of the time. Although the first time something from a particular standard header is used, it will be mentioned, both in the text of the book and in the accompanying code snippet. For the sake of clarity: `cout` and `endl` are defined in `#include <iostream>`, and this very `#include` line will be dropped from this point on.

* The code snippets provided in the book do not use namespaces, and they assume that the `using namespace std;` statement is present before the code in the snippet. This practice is highly discouraged in production code, but the upside is that the reader is spared from reading through numerous `std::` constructs. Ultimately, your understanding of what happens under the hood is what matters, and freeing the brain from clutter by striving for "minimally reproducible examples" is almost always a better route.

The Github repository to find the above examples and much more can be found at: **TODO:LINK**. It also has all the triggers actions configured, such that, unless anything major happens to Github, forking this repository and making commits to its `dev` branch should get you started in no time, straight from the Web browser. 

## The Build Process

Now, while there will be a dedicated chapter on compilation units, here's a short introduction for the reader who is only familiar with interpreted (and JIT-compiled) languages. 

1. The source file alone can not be run. It has to be built first, to produce the binary file. The binary file is what is run afterwards.

2. Compilation can be performed with various options. For instance, an important one is to differentiate between debug builds and optimized builds. Debug builds run slower, but offer benefits during development; most notably faster compilation times and better integration with step-by-step debugging.

3. In addition to source files and binary files there also exist object files. If your source file is small and self-contained, it can be built directly into a binary file. For larger projects, and/or for faster incremental builds, it is often advisable to separate the code into several compilation units. Compilation units are compiled separately; they can often be cached and re-used, and their builds can be parallelized. If this intermediate step involving object files is used, the process of putting together several object files into a binary is commonly referred to as linking.

Speeding up compilation by breaking up the code into compilation units in a clever way is subject to a separate chapter. Even if you only intend to use C++ for small self-contained tools, it is useful to know the details of this process inside out. This is the knowledge that keeps on giving, both within the C++ ecosystem and outside it.

Here is the important piece of wisdom and clarity that modern-day developers often forget. Fundamentally, the process of building a runnable artifact from source files is most often broken down into executing several binaries in the respective order. Modern-day build tools often take pride in shielding the end used from these details. In the case of C++ however, it is valuable to understand these steps.

Without loss of generality, on most platforms, the lower-level C++ build steps can be executed with the command that is called `g++`. Originally, naming-wise, `g++` is the C++ version of `gcc`, where the latter stands for the GNU Compiler Collection. On most Linux distributions, `g++` would indeed stand for the C++ compiler from the GNU Compiler Collection.

However, on a Mac, `g++` is simply the alias for `clang++`, the LLVM-based C++ toolchain supported by various vendors including Apple. Case in point:

```
$ uname -a
Darwin … MacBook-Pro.local 24.0.0 Darwin Kernel Version 24.0.0 …

$ md5sum $(which g++) $(which clang++)
27d06382b49f6a58661bbd6c2575d66f  /usr/bin/g++
27d06382b49f6a58661bbd6c2575d66f  /usr/bin/clang++
```

It is safe to say that GNU-based and LLVM-based C++ toolchains are the two major players today, and that together they mostly dominate the market. The true breakdown between `g++` and `clang++` is somewhat difficult to measure, but, in practice, once you begin using C++ seriously, your code will most likely be built and unit-tested by both `g++` and `clang++`.

The command-line interfaces of `g++` and `clang++` are virtually indistinguishable for the vast majority of use cases, so, unless you are doing something esoteric, you can treat them as the same binary.

Solidly in the third place stands the Microsoft compiler. It is a good one to use from time to time, since the entire stack it is built upon is totally different. In the modern day of Github actions and CI/CD pipelines, it is hard to underestimate the importance of running one's builds and tests on the Microsoft platform as well. The example code provided along with this chapter contains the respective run scripts as well. **TODO(dkorolev)**: Polish them.

It has to be said though, that in addition to Microsoft's own Windows-native C++ stack, the `*nix` versions of `g++` and `clang++` are gaining popularity on Windows. With Windows Subsystem for Linux (WSL) entering the stage several years ago, and with `bash` or `zsh` on the march to replace PowerShell on Windows, it is increasingly popular that the C++ code that is meant to be developed and run only on Windows th be using the `g++` or `clang++` stack.

However, even though it is entirely possible to be a Windows user and live on `g++` / `clang++` alone, it still remains a highly valuable advice that from time to time the Windows-native Microsoft C++ compiler, commonly referred to `cl.exe`, is also what builds one's code. To confirm the code builds successfully, and to confirm its tests pass on when the code is run in a Windows-native way, is no small deal. While the network stack may be different, almost everything else remains the same from the developer experience perspective, making `cl.exe` a great addition to diversifying one's builds. And the fact that `cl.exe` is distinctly different from `g++` and `clang++` makes it far more likely for it to highlight the details that other toolchains have missed.

To begin summarizing this chapter, while `g++` / `clang++` / `cl.exe` alone may well be sufficient to orchestrate most C++ builds, wrapper scripts are often used. I will highlight two in particular here.

## The `make` Utility

Make, or GNU Make, is, in fact, a tool that is not designed with C++, or even C, in mind. It "simply" is the oldest and likely most reliable way to build artifacts in the topologically sorted order: inner dependencies first, then going up the tree, until everything is ultimately built. The "scripting language" for Make are the `Makefile`-s, with a capital `M`, and the tool to run them is, unsurprisingly, `make`.

The syntax of the `Makefile` is deceptively simple. Note that the indentation is meant to be done in tabs, not spaces:

```
output.txt: input.txt
    (date; cat $<) >$@  # NOTE: Indent with a tab, not spaces.
```

This two-lines example defines the `output.txt` target that requires the `input.txt` one to be built first. Syntax-wise, `<$` stands for the first, and, in this case, only dependency to the target, while `$@` is the name of the target itself. Since there is no `input.txt:` section in this `Makefile`, it is assumed to be a file in the current directory; the invocation of `make` will fail if `input.txt` is not found.

If the `input.txt` file contains some:

```
This is the input file.
```

Then, after running the combo of `date` and `cat` in parentheses, the `output.txt` file will be:

```
Sun Oct  6 18:57:15 IST 2024
This is the input file.
```

(**TODO(dkorolev)**: Probably not the IST timestamp for the final version of the book, huh? How about Singapore time?)

The parentheses are just the shell construct in this case. The `Makefile` instead may well have read:

```
output.txt: input.txt
    date >$@     # NOTE: Indent with a tab, not spaces.
    cat $< >>$@  # NOTE: Indent with a tab, not spaces.
```

Also, `make` will not rebuild up-to-date targets unless it is instructed to do so explicitly. What is and what is not up to date is determined by the last modification timestamp of the file. In fact, this functionality alone is so powerful that oftentimes some more sophisticated scripts are best to be orchestrated by a top-level (or mid-level) `Makefile`.

To invoke the build of a particular `${TARGET}`, run:

```
$ make ${TARGET}
```

Just `make` would build the first target defined in the `Makefile`. So, for the example above, just `make` would be identical to `make output.txt`. In fact, most shells these days would tab-complete the target name.

Make targets are designed to be files of the respective name. However, over time, many target names became shortcuts for a specific behavior of the "make flow", not merely targets. For instance `make clean` is, conventionally speaking, not creating an output artefact called `clean`, but rather cleans up the build. Similarly, `make test` would run the tests. More and more "standard" terms are emerging over time, such as `make ci`, `make publish`. To signal `make` that a certain target is not the output-file-based target but merely a hint, this target can be listed as a `.PHONY:` target in the `Makefile`:

```
.PHONY: build clean

build: output.txt

clean:
    rm -f output.txt    # NOTE: Indent with a tab, not spaces.

output.txt: input.txt
    (date; cat $<) >$@  # NOTE: Indent with a tab, not spaces.
```

The example above, however simple, in fact looks like a production-grade `Makefile`.

To complete this brief introduction to `make`, it has many command-line flags, and `-j` is among the most important ones. It defined the parallelism factor, and running `make -j 4` would have `make` run up to four concurrent build processes, as long as the dependencies graph permits this. Providing `-j` without the number would, by convention, use all the available CPU cores. Here's a brief illustrative demo:

```
.PHONY: build clean

build: both.txt

clean:
	  rm -rf both.txt o1.txt o2.txt  # NOTE: Indent with a tab, not spaces.

both.txt: o1.txt o2.txt
	  cat o1.txt o2.txt >$@          # NOTE: Indent with a tab, not spaces.

o%.txt: i%.txt
	  (date; sleep 1; cat $<) >$@    # NOTE: Indent with a tab, not spaces.
```

The syntax should be familiar to the reader by now, except the percent sign, which effectively is the wildcard in file names.

Note that `-j` is best to be used with caution, since, even though `make` would make sure all the dependencies are respected, running a large number of build steps concurrently can and often does use up more RAM than expected, causing swapping and slowing down the system considerably. A habit worth developing would be to first run `make -j 2` to confirm it goes through, then perhaps slowly increasing the parallelism factor. For default runners of Github actions, as of 2024, `make -j 2` is generally best.

This concludes the quick into to `make` and `Makefile`-s. To wrap up this part, it is worth noting that many people — including the writer of this book! — have, over time, developed a habit of using `Makefile`-s just as the scratchpads of what should be executed and in which order. Java/Kotlin, or TypeScript projects, for example, can often be built by `make`, especially if and when figuring out the exact command lines to invoke was part of the exercise. Besides, `make` is a core utility on most if not all `*nix` systems, which makes it possible to invoke from literally everywhere, including `package.json` command lines and Github action runners.

Once you know `Makefile`-s you can't un-know them. Moreover, probably second to PCRE-s ("Perl-compatible regular expressions"), the language of `Makefile`-s may well be the most widely used declarative language in the world.

It is worth reiterating that while the indentation in `Makefile`-s should be done with tabs, not spaces, `Makefile`-s are the only thing in the C++ world where the choice of whitespace matters. Besides, unlike convoluted C++ cases, error messages from `make` are easy to read:


```
$ cat Makefile
foo:
  echo foo  # Uses spaces, not a tab!

$ make
Makefile:2: *** missing separator.  Stop.
```

To recap, `Makefile`-s are the language and `make` is the tool to run it. But not the only one. Of a number of alternatives, the name worth knowing is `ninja`, which is positioned as "same as make, but faster". Indeed, when the total size of the `Makefile`-s involved in the project grows into megabytes, `ninja` is there to the rescue. In fact, `cmake`, which is the subject of the next section (**TODO**: Do we call them sections?), defaults to `ninja` over `make`, as long as `ninja` is found available in the system. (**TODO(dkorolev)**: Test this hypothesis, both on Mac and on Linux.)

Another must-have feature for a good build system that `make` does not have is identifying indirect dependencies. While `make` is fully aware of what is defined in the `Makefile`, it does not know which C/C++ source files use which libraries. Thus, if not used carefully, there is always a naughty risk that the code itself is correct, but the project was not rebuilt in full, and is instead in the limbo tri-state, where some of the libraries linked are the up-to-date ones, while some others have not been updated. This is one of many problems where `cmake` comes to the rescue.

## The `cmake` Utility

Despite an eerily similar name, `cmake` is very different from `make`. Unlike `make` though, `cmake` is far more suitable for C++. To be precise, `cmake` was designed and used as a C-centric tool, far before C++ has entered the scene. Alas, we're living with its legacy now, and, quite frankly, it's a gift that keeps on giving.

Ironically though — and this is not the essential piece of knowledge to use it, but a useful piece of trivia — what `cmake` does behind the scenes is it generates a series of `Makefile`-s. Those auto-generated `Makefile`-s are not what a human is meant to read, let alone modify, but when push comes to shove at low-level debugging while cross-compiling for some unpopular architecture, this knowledge comes in handy.

Similarly to `make`, the `cmake` tool has its own definition language. Paying respect to the legacy, the default source script file for `cmake` is `CMakeLists.txt`. Here's how it looks like for a trivial C++ project of a single source file:

```
cmake_minimum_required(VERSION 3.5)
project(tiny)

set(CMAKE_CXX_STANDARD 17)

add_executable(tiny binary.cc)
```

Now, with the case of `make`, the destination directory or directories for build artifacts to be put under is encoded in the `Makefile` itself. When using `cmake` though, the language is more abstract, and it's up to the caller of `cmake` to specify those directories. Specifically, the user should run `cmake` twice:

1. For the first time to configure the build environment, once, and then
2. To build the artifacts, or, to be precise, to re-build the artifacts whose build is affected by the source files that were modified.

The second command requires the `—build` flag for `cmake`. In addition to this flag, the directory specified for the first command should also be provided.

The first command does not require the `—build` flag.

The shortest possible way to execute the `cmake`-build is:

```
$ cmake .          # NOTE: Not recommended!
$ cmake —build .  # NOTE: Not recommended!
```

This approach is not recommended due to a large number of various files created by the first command. There's simply too much of them to `.gitignore`, and they obscure the view. A better way is to specify a directory that is entirely `.gitignore`-d, such as `.debug`:

```
$ cmake -B .debug .     # Configure the build.
$ cmake —build .debug  # Run the build.
```

The first command, the configuration step, will need to be re-done if the `CMakeLists.txt` file changes. In particular, it needs to be re-done when source files and/or targets are added to the project, or when build parameters for various binaries or libraries have changed.

**TODO(dkorolev):** Talk more about language servers?

While this looks no simpler than a trivial one-liner `Makefile`, using `CMakeLists.txt` opens many doors. The major one of them is dependency management. Generally speaking, figuring out what parts of a C project need to be re-build is not a simple task. When this problem was first solved, there were no language servers, and static code analysis was in its infancy. In the world of C, this problem was solved by having the C compiler produce supplementary outputs detailing which header files does each source file depend on. While the details here are tricky to cover, for the introductory chapter it would suffice to say that `cmake` makes care of these problems for the developer.

The bottom line is that while for a single-source-file toy example a simple Makefile may well be the best choice, once the project begins to grow, it outgrows pure `Makefile`-s quickly. And, unless you are part of an established team with its own mature and versatile build system such as `bazel`, `CMakeLists.txt` is generally the way to go.

**TODO(dkorolev)**: Link to a complete end-to-end project.

There are two major reasons for why `CMakeLists.txt`-based builds are still powerful in 2024:

1. `CMakeLists.txt` is the only truly cross-platform way to define C/C++ targets. If you are looking for the lowest common denominator, and if you need or want your code to build on a wide range of platforms, `cmake` is not only your best friend; it likely is your only option.

2. Nontrivial yet basic important integrations, most notably gRPC / protocol buffers and Python bindings via `pybind`, come packaged with `CMakeLists.txt`, much like many other components. Moreover, if you develop your own C++ libraries for the broader team to use, then, unless you're part of a big company with its own build system, `CMakeLists.txt` is your best bet.

One downside of `cmake` to be aware of is that it offers a Turing-complete language that tends to get long and obfuscated. Not `autoconf`-level obfuscated, but enough to potentially be considered a security threat. This is why VS Code, among other IDEs, will always ask when opening a `cmake`-based project: "Do you trust the owners of the files in this directory?". So, be careful with using random `cmake`-based packages from the Internet. Although in the modern-day era, this applies to most, if not all, languages and package managers.

When it comes to how to best structure the directories, regular best practices apply. The output directories should be `.gitignore`-d at best, if you use Git, of course. If you are serious about the setup, it may be wise to configure the build such that the output directory is located on the different volume, so that the directory where the code lives is not polluted with binary artifacts.

Unless your setup is extremely creative, just deleting the output & intermediate directories will be the alternative to cleaning the build. Also, keep in mind that `cmake`-generated directories containing inner-level `Makefile`-s are tied to a particular path: moving them to a different directory will make the `cmake`-based build complain. If your build process requires this, consider compiling and linking slow-building libraries as "library targets" in `cmake`, such that the very path to the headers and the pre-built library file is all that's needed.

Last but not least: it is not difficult to set up a Docker-based build, where all the directories are mounted to the same predictable mount points, such that intermediate build artifacts can be reused as the host directory is moved. While the author of this book has gone through this exercise several times, the utility of this work remains questionable, since most developers seem to not need such a fine-tuned setup, and those who do tend to prefer their own ways.

## Setup

After all the theory above, the local setup should be quite straightforward.

If you are a terminal user, use `Makefile` + `make` or `CMakeLists.txt` + `cmake`. It is somewhat easier to run `make` from vim (`:mak`), while from the IDE a `CMakeLists.txt` file would be preferred.

In the Github repository accompanying this book there are example setups to build & run & debug the currently selected source file from VS Code. This often proves to be handy.

Since the everpresent motto of this book is "you can do it yourself!", here is the **TODO(dkorolev):** Link to the Github repo which one can fork, replace `github.com` by `github.dev`, make code changes in the browser and see the modified code run by Github actions in real time:

* A manually built single-source-file example: Link. **TODO**
* Single source file built via a `Makefile`.
* Single source file build via `CMakeLists.txt`.
* Several source files, built manually.
* Several source files, built via a `Makefile`,
* Several source files, built via `CMakeLists.txt`.

Last but not least: IDE setup. TODO(dkorolev): 

## Recap

This first chapter was a brief introduction to the world of building and running C++ code. By this point:

* beginner C++ developers are, hopefully, not discouraged to proceed, and
* more experienced people may have calibrated on the degree to which the structure of this book sticks to first principles.

With this in mind, off we go!