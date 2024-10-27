# Chapter 3: Thinking of Memory

## Introduction

This chapter serves as a brief introduction to memory models. After reading this chapter you will:

* Internalize that using memory effectively can have dramatic effects on the performance of the code.
* Understand that certain level of "memory-mastery" is only possible with lower-level languages.
* And, hopefully, appreciate that C++ connects these two worlds, offering the developer the opportunity to build high-level abstractions over low-level-effective data structures.

## Computation is what Computers Do

In the beginning of the current incarnation of the era of computation there was the word.

The `word` is still a commonly-used term in systems programming, and it simply means a 16-bit chunk of memory, two bytes. Four bytes would be a `dword`, a double word.

Today, if a programmer uses such a term, it generally means we are talking about very low-level programming, where what these small chunks of memory would be used as are individual bits, often parsed as collections of enable/disable flags, or other bitwise operations. It would not make much sense to ask whether the `word` is signed or unsigned, big-endian or little-endian. It would not make much sense to ask whether a `dword` is a floating point number.

Also, I am shamelessly counting the current era of computation loosely from Kondar Zuse and Claude Shannon, which is the 1940s. One could argue that Alan Turing and his Enigma should be included too, but I'd say for the purposes of our conversation Enigma is closer to an abacus or an arithmometer, albeit a very sophisticated term. The current era of computation is about **programming** computers, where this process of programming is performed by humans, and where what counts is the art and craft of continuously developing and shipping software, rather than designing one particular computer for one particular task.

## A Bit of Computer Architecture

The word in memory, that was there at the begining of computation, could generally be of one of two very distinct meanings:

* The *contents* of memory, or
* The *program code* to be executed.

The contents of memory, in simple terms, is the operational data set that the program works with. It's the values of variables. It's those arrays and buffers and more sophisticated data structures. It's data files loaded into memory. It's kernel-level stuff that has to do with threads, processes, scheduling, journaling, and other subsystems that keep the machine up and running. It's the stack used by the CPU while it executes the program.

The program code is very special. On the vast majority of architectures, before the program can be *executed* by the CPU it needs to be loaded into main memory.

This distinction of two functions of memory is the major differentiator between the von Neumann architecture and the Harvard architecture. The von Neumann architecture is what is described above: dual-purpose memory, where code and data can and do intertwine. The Harvard architecture, still popular for certain microcontrollers, separates *program memory* from the memory that this program can use while excuting.

Another interesting aspect of von Neumann archicture is that it enables what is called the *self-modifying code*. The topic of self-modifying code is fairly advanced, and it, most likely, is not what our average reader would need to be exposed to. Yet, for certain low-latency high-throughput tasks, it may be essention to not only JIT-compile ("just-in-time compile") a piece of code, but also to place it into a specific memory location, effectively overwriting the piece of program logic that was at this location before. Self-modifying code is non-portable and extemely painful to debug, so we will not be touching it in this book. JIT-compiling C++ code though, so that we can inject a piece of logic that did not exist in binary code before the program was started, as a sequence of native CPU instructions -- this is something we will dedicate room in the next chapters, so stay tuned.

## CPU Caches

Now that we went deeper into computer architecture, it is time to get introduced to the worst offender of otherwise-effective C++ code: cache misses.

<details>
  <summary>A bit of a deep dive.</summary>

  To be fair, it is not *always* cache misses that are the worst offender.

  In multithreaded code lock contention can be the most important issue to work around. Although, granted, if the code is idle on a mutex, its CPU usage would be far under 100% per core, and lock contention issues are generally easy to isolate. Fixing them often requires major redesigns, but at least they are not hiding in plain sight. Pro tip: unless absolutely required, prefer lock-free data structures, and use `atomic`-s over mutexes. We will cover this in the upcoming chapters.

  Also, lack of buffering is another major pain point. Novice programmers can be caught reading gigabytes byte by byte, which is clearly a non-starters.

  Branch prediction can also play tricks when it comes to optimizing peak performance. I have personally seen a piece of code where changing `if (!A) { X } else { Y; }` into `if (A) { Y; } else { X; }` made the code 3x faster.

  Nonetheless, nothing can slow down an otherwise well engineered pieces of code as dramatically and as unexpectedly as cache misses.
</details>

The thing is, compared to disk or network, memory is *really* fast to access. Except not quite. In fact, accessing memory at random, that is, accessing memory at an address that the CPU does not *expect* is quite slow compared to acessing memory in a local and predictable way.

This is because the CPU has caches. If you plan to work in high-frequency trading, or on compiler optimization, you would need to understand now CPU caches work really, really well. For mere mortals, just knowing a few basic things would get us most of the way there. These basic things are:

* There are several layers of caches available to the CPU.
* Generally speaking, the most innermost layers are the fastest to access, but smallest in size.
* Caches loosely follow the LRU ("least recently used") pattern: if some memory page is accessed often, it it kept warm in the cache, and as soon as this page is no longer required it is preempted in favor of a more-needed page.
* Caching *can* be local to CPU cores, and keep in mind that some CPU cores are closer to each other than others. This is why thread affinity (hard-binding kernel threads to CPU cores) is so imporant for latency-sensitive code.
* The CPU is generally trying to look ahead ("instructions prefetching") and try to do its best to guess what parts of the main memory are best to cache for the immediate future. Therefore, cache misses are not as expensive as they could be in theory, but they still are pretty bad.

What often comes as a shock even to experienced programmers is how expensive cache missed are. Consider the following code, let's call it `fast.cc`. All it does is initialize a billion bytes of memory:

```
#include <cstdint>

constexpr int N = 1000;

uint8_t X[N][N][N];

int main() {
  uint8_t z = 0;
  for (int i = 0; i < N; ++i) {
    for (int j = 0; j < N; ++j) {
      for (int k = 0; k < N; ++k) {
        X[i][j][k] = z;
        ++z;
      }
    }
  }
}
```

Here is the result of running this code on the M3 MacBook Pro:

```
$ g++ -std=c++17 -O3 fast.cc && time ./a.out
./a.out  0.04s user 0.06s system 44% cpu 0.227 total
$ g++ -std=c++17 -O3 fast.cc && time ./a.out
./a.out  0.04s user 0.06s system 43% cpu 0.231 total
$ g++ -std=c++17 -O3 fast.cc && time ./a.out
./a.out  0.04s user 0.06s system 43% cpu 0.228 total
```

The `-O3` flag is to enable optimization.

It takes the MacBook approximately one fourth of a second to write one gigabyte into memory. Makes perfect sense for a 4GHz machine using a single CPU core.

What would happen if the order of indexes is swapped? Call this `slow.cc`:

```
#include <cstdint>

constexpr int N = 1000;

uint8_t X[N][N][N];

int main() {
  uint8_t z = 0;
  for (int i = 0; i < N; ++i) {
    for (int j = 0; j < N; ++j) {
      for (int k = 0; k < N; ++k) {
        X[k][j][i] = z;
        ++z;
      }
    }
  }
}
```

The only change is that instead of `[i][j][k]`, indexing the array in the effectively natural order of how its elements are laid out in memory, this code iterates over the elements in the `[k][j][i]` order. Such an order is a disaster for the CPU, since it has no idea where to look for.

And here is the result of running this code:

```
$ g++ -std=c++17 -O3 slow.cc && time ./a.out
./a.out  9.99s user 0.12s system 98% cpu 10.287 total
$ g++ -std=c++17 -O3 slow.cc && time ./a.out
./a.out  9.99s user 0.09s system 98% cpu 10.210 total
$ g++ -std=c++17 -O3 slow.cc && time ./a.out
./a.out  10.04s user 0.08s system 98% cpu 10.260 total
```

As we can clearly see, `slow.cc` is over forty times slower than `fast.cc`.

<details>
  <summary>Another interesting experiment...</summary>
  Another interesting experiment worth running may well me looking at how much power does a computer require, per second or per hour, when running `fast.cc` vs. `slow.cc`.

  My guess would be that for the vast majority of architectures `slow.cc` will require more power. Because the very CPU is not that hungry for those watts of power when performing trivial arithmetic such as incrementing and comparing numbers. It's memory access that needs more power output. And `slow.cc` is a big offender, while `fast.cc` is, well, as fast and as obedient as one could be with respect to not requiring extra memory seeks.

  On a laptop, such an experiment can be modelled quite easily by measuring the amount of time it takes for the machine to drain the battery from 100% to, say, 75%.
</details>

## TBD

## Summary

