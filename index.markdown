---
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

Hi, I'm Chris, and this is a bit about me. I am acurrently a senior at
the University of Rochester in Rochester, NY and will graduate in May
2020 with an Honors BS in Computer Science. My research interests are
primarily in heterogeneous computing systems of all kinds
(persistent, approximate, analog, etc) and in the compiler's role in
using these components to accelerate programs. Upon completion of my
degree, I am seeking to pursue a Ph.D. in Computer Science with a
specialization in the aforementioned areas.

# Education

University of Rochester, May 2020
- Honors B.S. in Computer Science
- 3.84 Major GPA, 3.69 Cumulative GPA
- Recipient of the Bausch & Lomb Scholarship, which has been renewed
  each year for 4 years

## Coursework
- Future: Topics in Quantum Computing (295), Honors Thesis,
  Undergraduate Problem Seminar), Performance Art
- CS: 3 Semesters of Independent Research,
      Design of Efficient Algorithms (282), Computational Models of
      Perception and Cognition (229), Low Level Parallel Programming
      (Uppsala), PLDI (254), Operating Systems (256), Artificial
      Intelligence (242), Computer Models and Limitations (280),
      Computational Intro to Statistics (262), Computer Organization
      (252), Computation and Formal Systems (173), Linear Algebra and
      Differential Equations (MTH 165), Data Structures and Algorithms
      (172), Discrete Math (MTH 150), Cryptography (Uppsala)
- ECE: Advanced Computer Archecture (ECE 201), Logic Design (ECE 112),
       Accelerating Systems with Programmable Logic Components (Uppsala)
- Art: Introduction to Painting, Introduction to Drawing
- History: Modern Japan (HIS145), Latin America through Soccer (HIS154),
           Postwar Europe (HIS 128)

## Extracurriculars
- For the last 3 years, I have been researching with the CS
  Systems group at the University of Rochester, more details provided
  below under "Programming Experience".
- I currently work with the UR Computational Fluid Dynamics group
  to assist in writing user-defined functions for their
  simulations.
- From Fall 2016 to Fall 2018, I ran for the University's Cross
  Country and Track teams. Unfortunately, a serious injury
  prevented me from continuing.
- Intermittently from Fall 2016 to the Spring of 2018, I was a
  member of the Solar Splash team. This team competes yearly to
  build a solar-powered boat for endurance and sprint races. I
  was involved in constructing one of the wooden hulls.

# Programming Experience

## Compiler R&D Intern, Cray Inc, Summer 2019
The project I was assigned to work on for this internship involved
improving the loop idiom optimization in Cray's LLVM based compiler by
writing my own pass and augmenting existing passes to recognize more
complex memset, memcpy, and memmove idioms. 

At the beginning of the summer, I (naively) thought this would be an
easy task because it was easy to do by hand. I was given
the following loop and told that this case needs to be regularly
optimized.

```cpp
for(unsigned x = 0; x < x_e; ++x)
  for(unsigned y = 0; y < y_e; ++y)
    *(mem_location++) = 42;
```

It turns out that there were many more corner cases than I imagined.
This turned out to be the perfect project because while simple in
intuition, there was a serious amount of thought and challenge that came
in actually implementing my solution.

I originally started by thinking that the easiest path would be writing
a pass that would manually find anything we were targeting - the
upstream pass couldn't handle it. After a month and I finally had
something that worked, I realized that the upstream pass was much more
capable than I expected. It's important that I started this way though
because working on my own pass taught me enough to understand what the
upstream code was doing.

My understanding of the upstream pass distressed me greatly because it
meant I might have to throw away the last month of work. Thankfully for
my sanity, it turned out in the end that we needed both passes to get
everything right. Coming in on the last day to see the successful
results from our performance benchmarks was a surreal experience.  

While I had gained some introductory experience in LLVM through my
internship at Lehigh University during the prior summer, it was limited.
The work I did at Cray definitely helped me make huge strides in my
understanding of LLVM. The biggest reason for my growth at Cray, was the
generous help from my colleagues, who helped me in everything from Git
to LLVM (as you might expect) to recreational math.

I presented a [poster][CrayPoster] of my work at the Intern fair
at the end of the Summer and [again][URPoster] at another poster fair for
UR's Computer Science open house.

## Intern, Intelligent and Scalable Systems Research Experience for Undergrads (REU), Lehigh University, Summer 2018

In this internship, I wrote several LLVM passes to helped provide static
separation of memory with certain properties in C++.

This [program][LehighNSFLink] is part of a large series of NSF sponsored 
programs in which undergraduate students work and research during the
summer on project all over the nation. I chose to work on a language
feature project that concerned itself with the future complications
posed by increasingly heterogeneous systems: with more and more feature
bloat in a system, a programmer become less and less able to fully take
advantage of their system. The beauty of languages is that they are able
to abstract away from the hardware and provide a high-altitude overview of
what is to happen. 

The system that I worked most on was transactional memory.

### Transactional Memory
A transaction is a memory operation that appears to happen instantly,
and can produce a consistent history of actions.  

My contribution was in the topic of static separation of 'transactional'
variables. I added a program feature that allowed programmers to tag
variables that were transactional. If they were used outside of a
transaction, then we would know. This could help prevent bugs that lead
to race conditions, for instance. Here's a bit of code to demonstrate...

```cpp
// You imply this will only be used in transactions
TX_PTR(int) my_int = ...;
// a while later
transactional {
  if (my_int != nullptr)
    *my_int = 24;
  else
    my_int = malloc(...)
}
// We assert that the comparison of `my_int`, the read, and assignment
// will happen transactionally.
```
And while this is all happening a different thread is doing the
following.
```cpp
int a = *my_int; // line 1
*my_int = a * 7; // line 2
```

The tagging of `my_int` implies that any changes to `my_int`, and any
operations that rely on a read from it all happen in an instant. While
the first code block keeps this promise by putting the code in a
transactional block, the second code block makes non transactional
changes. Between line 1 and 2, a transaction from the first code block
could have occurred. Then, when line 2 occurs, it is writing a stale
value read from line 1. Static separation would prevent this from
happening by detection at compile time.

The intended effect is similar to the `const` keyword where const
things can not be made non-const and not passed to anything that does
not explicitely expect its argument to be const.

During my time at Lehigh, I also took part in the Pittsburgh
Supercomputing Center's XSEDE HPC summer boot-camp. During the week long
seminar, I learned OpenMP, OpenACC, and MPI and got some experience
working on a supercomputer.

## Researcher, University of Rochester, Fall 2018 to Present

### Current Project
I am currently working on accelerating [Memcached][MemcachedLink] using a
variety of technologies that my group is developing. The first of which is
[Hodor][HodorLink],
 a mechanism meant to securely provide intra-process communications
 without the use of the OS.
I began this project last Fall (I did not do this in the Spring because
I studied abroad) and have continued work on it this semester as well.
I intend on making memcached persistent using a persistent STM
system and a persistent allocator (both awaiting publication).

### Past Project
During the beginning of my work with the systems group, I worked mostly
with hardware transactional memory and graph algorithms.

## System Developer Intern, [Dewire][dlink], Sundsvall, Sweden, Summer 2017
[dlink]: https://www.dewire.com/
In brief, I helped fill in missing tests in their automated testing suite
for their Android applications. My biggest contribution was definitely
in developing a testing service so that tests could be run faster.

The challenging part of the summer was when I had to make
the testing robot. The intent was to be able to have a testing slave
that would be capable of running all the tests on all of the connected
devices simultaneously. We used many devices for tests because they were
had differing screen sizes and OS versions. One problem is that some
tests **couldn't** run simultaneously while some couldn't. Additionally,
users needed a unique key at runtime.

My solution involved making a small server on the testing device that
each device first queried for an ID. The server also served to
coordinate tests such that tests that could be run simultaneously could,
and tests that needed to run alone were held behind a lock on the
server. I was proud of my work and it seemed to work well. From what
I've been told, my slave is still running and hasn't been touched since
I left!

# Teaching Experience

## Fall 2019

- Programming Language Design & Implementation - This is the first time
  I've led a workshop for a class. 
- Volunteer Tutor - Twice a week I hold office hours for any class I've
  ever taken. This is a challenge because of the sheer breadth of the
  material.

## Spring 2019

I was not in Rochester during the Spring of 2019, as I was studying
abroad in Uppsala, Sweden at the University of Uppsala. One of my
favorate things I noticed about the classes there is that in _every_
class I had to give a presentation.

For my cryptography class, I [presented][UppsalaPrez2] a [paper][PPaper] that
was _way_ out of my comfort zone. I had never parsed such complex math
before, but by the end of it, I had become at least a tad more acclimated to the
area. I didn't prepare a presentation that verbatim explained
the proof, but instead intended the audience to leave with a little
better understanding of what primality is, why it's hard, and the
contribution of the authors.
I went in expecting very little of my presentation, but was mildly
surprised that people seemed interested afterwards and actually asked
some questions. In addition to the presentation, I wrote a
report that summarized the research in more technical detail.

For my FPGA course, I
[presented][UppsalaPrez1] a [paper][IpekPaper] by a UR faculty member,
Engin Ipek. This paper really contributed to my interest in analog
computing systems.


For my Low Level Parallel Programming course, instead of a paper we
[presented][ResultPrez] the results of a course-long project.
This project was given to us as a naive, sequential
implementation of a simulation. Over the course of the semester, I
transformed it into a lean, mean simulation that used CUDA, OpenMP, and
SIMD instructions to accelerate the computation. Some aggressive
optimizations resulted in some satisfying performance speedups (~1000x).

## Fall 2018

- Advanced Computer Architecture TA - I really enjoyed this position. I
  was responsible for grading a lot more individually, developing
  projects for the students and my input was considered in assigning
  grades. The audience for office hours was much different than my
  earlier classes because the material itself was much more challenging
  and oriented more towards grad students. This has been my favorite
  class to TA.

## Spring 2018

- Computer Organization TA
- Computation and Formal System TA

## Fall 2017

- Computation and Formal System TA

[UppsalaPrez1]: /assets/FPGA_pres.pdf
[IpekPaper]: https://www.cs.rochester.edu/~ipek/hpca16.pdf
[UppsalaPrez2]: /assets/crypto_presentation.pdf
[PPaper]: https://www.cse.iitk.ac.in/users/manindra/algebra/primality_v6.pdf
[ResultPrez]: /assets/LLPP_pres.pdf
[HodorLink]: https://www.cs.rochester.edu/u/scott/papers/2019_ATC_Hodor.pdf
[PanteaLink]: https://dl.acm.org/ft_gateway.cfm?id=3328796&type=pdf
[MemcachedLink]: https://www.memcached.og
[LehighPrez]: /assets/presentation.pdf
[CrayPoster]: /assets/CrayPoster.jpg
[URPoster]: /assets/URPoster.jpg
[LehighNSFLink]: https://www.nsf.gov/awardsearch/showAward?AWD_ID=1757787

