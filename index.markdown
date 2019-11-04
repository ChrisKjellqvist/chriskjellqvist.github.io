---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---
# Education
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

