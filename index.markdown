---
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

Hi, I'm Chris, and this is a bit about me. My research interests are
primarily in heterogeneous computing systems of all kinds 
(persistent, approximate, analog, etc) and in the compiler's role in 
using these components to accelerate programs.

will be involved in this proces of acceleration.

Below is an extended version of my CV. If you have any
questions, feel free to contact me.

# Education

University of Rochester, May 2020
- Honors B.S. in Computer Science
- 3.84 Major GPA, 3.69 Cumulative GPA

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
  Systems group, more on that below. 
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
Briefly put, I improved loop idiom optimization in Cray's LLVM based
compiler by writing my own pass and augmenting existing passes to
recognize more complex memset, memcpy, and memmove idioms.

At the beginning of the Summer, I (naively) thought this would be a
really simple task because it was easy to do by hand. I was given
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

Being certain that the upstream pass couldn't handle the job, I started
by coding my own solution. I then realized my original premise was wrong
so I put a serious amount of work into re-shaping upstream. After
further trials I came to realize the most efficient solution was going
to a combination of both passes. Not only did implementation prove to be
difficult, but finding the shape of desired solution was a challenge in
itself.  

When I went to Lehigh (below), I had no training in using LLVM, 
and when I left, I left only with what I had learned myself. Cray was a
much different experience because my colleagues were an enormous help in
becoming proficient at LLVM. By the end of the summer I surprised myself
by how much I felt I had learned. 

I presented a [poster][CrayPoster] of my work at the Intern fair
at the end of the Summer and [again][URPoster] at another poster fair for
UR's Computer Science open house.
## Intern, Intelligent and Scalable Systems Research Experience for Undergrads (REU), Lehigh University, Summer 2018

In short, I wrote several LLVM passes to help provide static separation
of memory properties in C++. 

This program is part of a large series of NSF sponsored programs where
undergraduate students work and research during the Summer on projects
all over the nation. I chose to work on a language feature project that
concerned itself with the future complications promised by heterogeneous
systems: with more and more features that a system can offer, the
programmer will become less and less able to take full advantage of
their system. The beauty of languages is that they are able to abstract
away from the hardware and provide a high-level command of what is to
happen. 

The system that I worked most on was transactional memory. 

### Transactional Memory
A transaction is a memory operation that appears to happen instantly.
Here's an analogy to explain:

Imagine you're moving your furniture from one house to another. The 
movers arrive, pack up your favorite recliner, and leave. 
Sometime during the day, you get a back ache so you drive home for 
lunch to relax on your recliner, but find it gone. Well, if it wasn't 
there, it must be at your new house - so you drive there, but it also 
wasn't there! You throw up your hands and collapse in defeat. The 
movers stopped to get lunch on the way...

If the movers moved transactionally, the recliner would move
instantaneously from your first house to your second.

The abstraction of a moving company is that they move things, timing 
is just an implementation detail. In the same way, you program
your computer to complete operations with certain abstractions,
timing also not being one of them. Transactional memory gives the 
program the impression that your furniture is being moved 
instantaneously.

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
// all appear to happen instantaneously.
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

During my time at Lehigh, I also took part in the Pittsburgh
Supercomputing Center's XSEDE HPC summer boot-camp. During the week long
seminar, I learned OpenMP, OpenACC, and MPI and got some experience
working on a supercomputer. 

While the work of the PhD student eventually [got published][PanteaLink], I was
unable to contribute because I left long before the actual publications.

I also gave a [presentation][LehighPrez] while I was there.

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

This was both my first real job (if you don't count camp counselor), and
my first time developing with any expectation of quality or performance.
it was a really great experience where I met people I still keep in
touch with. 

I won't mention much of the test automation because it was all quite dry
and standard. The challenging part of the summer was when I had to make
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
  I've led a workshop for a class. It's been much different than just
  grading and holding office hours. Certainly much harder, but I've
  learned a lot more
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

