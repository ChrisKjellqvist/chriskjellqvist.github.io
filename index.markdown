---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

# Education
## Details

University of Rochester, May 2020
- Honors B.S. in Computer Science
- 3.84 Major GPA, 3.69 Cumulative GPA

## Coursework

- CS: Design of Efficient Algorithms (282), Computational Models of
      Perception and Cognition (229), Low Level Parallel Programming
      (Uppsala), PLDI (254), Operating Systems (256), Artificial
      Intelligence (242), Computer Models and Limitations (280),
      Computational Intro to Statistics (262), Computer Organization
      (252), Computation and Formal Systems (173), Linear Algebra and
      Differential Equations (MTH 165), Data Structures and Algorithms
      (172), Discrete Math (MTH 150)
- ECE: Advanced Computer Archecture (ECE 201), Logic Design (ECE 112)
- Art: Introduction to Painting (112), Introduction to Drawing (111),
       Performance Art (Future) 

## Extracurriculars
       - For the last 3 years, I have been researching with the CS
         Systems group, more on that below. 
       - I currently work with the UR Computational Fluid Dynamics group
         to assist in writing user-defined functions for their
         simulations. 
       - From Fall 2016 to Fall 2018, I ran for the Univeristy's Cross
         Country and Track teams. Unfortunately, a serious injury
         prevented me from continuing.
       - Intermittently from Fall 2016 to the Spring of 2018, I was a
         member of the Solar Splash team. This team competes yearly to
         build a solar-powered boat for endurance and sprint races. I
         was involved in constructing one of the wooden hulls.

# Experience
## Compiler R&D Intern, Cray Inc, Summer 2019
Briefly, I Improved Loop Idiom optimization in Cray's LLVM based
compiler by writing my own pass and augmenting existing passes to
recognize more complex memset, memcpy, and memmove idioms.

At the beginning of the Summer, I (naively) thought this would be a
really simple task because it was easy to do by hand. I was given
the following loop and told that this case needs to be regularly
optimized.

    for(unsigned x = 0; x < x_e; ++x)
      for(unsigned y = 0; y < y_e; ++y)
        *(mem_location++) = 42;`

It turns out that there were many more corner cases than I imagined. I
learned a ton from this experience, from code style, to git, to
presentation skills. I presented a poster of my work at the Intern fair
at the end of the Summer and presented it at another poster fair for
UR's Computer Science open house. \[[Cray
Poster](/assets/CrayPoster.jpg)\] \[[UR Poster](/assets/URPoster.jpg)\]
## Intern, Intelligent and Scalable Systems Research Experience for Undergrads (REU), Lehigh University, Summer 2018
Briefly, I wrote several LLVM passes to help provide static separation
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
  The idea of a transaction is simple. Imagine you're moving your
  furniture from one house to another. The moving people arrive, pack up
  your favorite recliner, and leave. Sometime during the day, you get a
  back ache so you drive home for lunch to relax on your recliner, but
  find it gone. Well, if it wasn't there, it must be at your new house -
  so you drive there. But it also wasn't there! You throw up your hands
  and segfault. The movers stopped to get lunch on the way...
  
  The concept of movers is that the move things, having nothing to do
  with timing. In the same way, your computer can take time to make
  changes (like moving data), but transactional memory gives the program
  the impression that your furniture is being moved instantaneously.

My contribution was in the topic of static separation of 'transactional'
variables. I added a program feature that allowed programmers to tag
variables that were transactional. If they were used outside of a
transaction, then we would know. This could help prevent bugs that lead
to race conditions for instance.

During my time at Lehigh, I also took part in the Pittsburgh
Supercomputing Center's XSEDE HPC summer bootcamp. During the week long
ordeal, I learned OpenMP, OpenACC, and MPI and got some experience
working on a supercomputer. 

[My Presentation](/assets/presentation.pdf).

## Researcher, University of Rochester, Fall 2017 to Present
### Current Project
I am currently working on accelerating
[Memcached](https://www.memcached.org) using a variety of technologies
that my group is developing. The first of which is Hodor, a mechanism
meant to securely provide intra-process communications without the use
of the
OS\[[1](https://www.cs.rochester.edu/u/scott/papers/2019_ATC_Hodor.pdf)\].
I began this project last Fall (I did not do this in the Spring because
I studied abroad) and have continued work on it this semester as well.
We also intend on making memcached persistent using a persistent STM
system and a persistent allocator (both awaiting publication).

###  
