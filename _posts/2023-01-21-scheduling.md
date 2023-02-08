---
layout: post
title:  "How Does Multitasking Actually Work"
date:   2023-01-21
categories: programming
---
THIS IS A WIP AND I MAY NOT RETURN TO IT FOR A WHILE

# Multitasking in my Mock OS
One of the projects I was most invested in making when I was a programmer was a mock OS using SmileBASIC on the 3DS called **Jewel**. I didn't really have the knowledge on how to do real multitasking at all, so I just made it a single application mock OS for the longest time.

There soon became a trend of people making mock OSs with multitasking. One that I was especially worried about was one called ZOS, it used a very strict compiler-esque scheme for programs so that they would be able to support multitasking. I wanted to stay competitive with these other OSs, so I completely rewrote my OS and also added a compiler scheme -- one that was able to compile average programs more easily I hoped, and inserted jumps back to the main mock OS whenever I deemed it appropriate. Knowing what I know now, I could've probably done it in much better places (or implement an entirely different scheme entirely), but I did what felt like worked at the time.

I didn't know it at the time, but the method of multitasking that I did at the time was **cooperative multitasking**, one where the OS would never preemptively interrupt the process, it would all be the responsibility of the process to relieve control to the OS.

In Jewel, every time that I felt that a yield was necessary, I would insert something like the following into the program. 

```basic
JMP$[P%]=@ABC123:RETURN:@ABC123
```

