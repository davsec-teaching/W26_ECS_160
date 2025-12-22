## ECS 160: Software Engineering

This course is focused on modern software engineering. Two key observations drive the course syllabus. 
First, over the past few years (or even decades), software engineering has become
less about writing code and more about system design at scale. Therefore, the course will focus
equally on how to design and test individual components, as well as, how to build and orchestrate systems at scale. 
We will focus on design patterns and component-level testing, but also on microservices, event-driven software design, 
and "No-ops" orchestration frameworks.

The second observation is that while 
frameworks and technologies rapidly evolve, the fundamental concerns driving this evolution remain more consistent.
For example, modern software systems are interconnected, making network performance an important concern in these 
systems. This has resulted in various orchestration frameworks such as Kubernetes optimizing the _data path_---the
network communication mechanism between different nodes. A discussion of the design of such frameworks is incomplete without
understanding the system components driving these design choices. To this end, the course will cover a handful
of operating systems and distributed systems concepts briefly, and will focus on _concepts_ more than APIs. 
The homework assignments will, however, provide some hands-on experience with 
the latest technologies such as Apache Cassandra, Spring Boot, Kafka, and Kubernetes.

### Prerequisites

The course has a formal prerequisite of ECS 140A. Additionally, the following are the _soft_ prequisites.

The student is expected to have some amount of programming maturity. Knowledge
of basic OO concepts, such as inheritance, runtime polymorphism, exception handling,
and so on, and some experience developing non-trivial applications, will be assumed of the student. The
student is also expected to have experience with socket programming and should be familiar with IP addresses and ports. 
Basic knowledge of libraries and system calls will also be assumed. And finally,
the student should be familiar with modern software engineering tools such as git, debuggers, IDEs (VSCode, IntelliJ), etc.
Knowledge of databases (SQL or otherwise) will not be assumed, but will be helpful.


### Basic information

| **Information**          | **Details**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| **Instructor**      | Tapti Palit (tpalit@ucdavis.edu)                                                 |
| **TA**              | Gabe Bai (gabbai@ucdavis.edu)                     |
| **Lectures**        | MWF 09:00 AM - 09:50 PM in Veihmeyer Hall 212              |
| **Discussion**      | W 12:10 PM - 1:00 PM in Wellman Hall 234                   |
| **Piazza**     | https://piazza.com/ucdavis/winter2026/ecs160winter2026/home               |
| **Instructor Office Hours**    |  W 2-4 PM (Zoom*)               |
| **TA Office Hours** | TBD |

_* Location TBD_
### Schedule

The course schedule can be found [here](Schedule.md). 

The slides and homeworks will be posted well ahead of the lecture day, but can be updated any time to fix errors, improve clarity, and so on. Since the course website
is hosted on Github, you can check the commit history to know what was updated.

Lectures and discussions will be recorded.

### Grading

Grading will be broken down as follows. 

| **Grading component**          | **Weightage**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| **Midterm**      | 30%                                                 |
| **Final**        | 35%                     |
| **Homework Assignments**  | 20%              |
| **In-class quiz**      | 10%                         |
| **Readings** | 5%              |

- There will be four homework assignments. The assignments must performed individually, and are designed to give the student hands-on experience with current frameworks and toolchains.
- In-class quizes will consist primarily of multiple-choice questions. We will have 5 quizzes in class - each worth 2% of the grade.
- We will have 7 readings on Perusall. Students should read the allotted reading and comment directly on the document uploaded. Points will be given for comments, responses, and upvoting others comments.
 Each student will get a grade out of 1 on their engagement for each reading. The bottom 2 grades for each student will be dropped.
- The midterm and final will be closed book. You will be allowed a handwritten single-sided A4-size "cheat sheet" for the midterm and a handwritten double-sided A4-sized "cheat sheet" for the final exam. Both the midterm and the final will include all topics taught in class, the homework assignments, and assigned readings.
- The final will be cumulative and include all topics taught during the course.

The grade cutoffs will be as follows. Grades cutoffs might be lowered further, but you will always get _at least_ the grade mentioned in the table below if you score the required percentage. 

| **Percentage**          | **Grade**                                                                 |
|----------------------|---------------------------------------------------------------------------------|
| **93%**      | A                     |
| **90%**      | A-                     |
| **87%**      | B+                     |
| **83%**      | B                     |
| **80%**      | B-                     |
| **77%**      | C+                     |
| **73%**      | C                     |
| **70%**      | C-                    |
| **67%**      | D+                    |
| **63%**      | D                     |
| **60%**      | D-                    |

## Textbook

There is no required textbook. But the following textbooks and resources are recommended for reference. The links to the UC Davis online resource has been added where available.

- Design Patterns: Elements of Reusable Object-Oriented Software, by Gamma Erich, Helm Richard, Johnson Ralph, Vlissides John, Grady Booch [link](https://search.library.ucdavis.edu/permalink/01UCD_INST/1hjlc2p/cdi_nii_cinii_1130000795942501632)
- Parallel Programming Patterns: Timothy G. Mattson, Beverly A. Sanders, Berna L. Massingill [link](https://search.library.ucdavis.edu/permalink/01UCD_INST/9fle3i/alma9914814160006531)
- Designing Data-Intensive Applications: Martin Kleppmann [link](https://search.library.ucdavis.edu/permalink/01UCD_INST/9fle3i/alma9914814325706531)
- Kafka - the definitive guide: Neha Narkhede, Gwen Shapira, Todd Palino [link](https://search.library.ucdavis.edu/permalink/01UCD_INST/1hjlc2p/cdi_askewsholts_vlebooks_9781492043058)
- Kubernetes in Action - Marko Luksa [link](https://search.library.ucdavis.edu/permalink/01UCD_INST/1hjlc2p/cdi_skillsoft_books24x7_bks000147117)
- Effective Software Testing - Mauricio Aniche [link](https://search.library.ucdavis.edu/permalink/01UCD_INST/1hjlc2p/cdi_askewsholts_vlebooks_9781638350583)

## Policies

### Collaboration 

Assignments will be completed individually. You are welcome to discuss _approaches_ and _design_ with your classmates in different groups, but you are forbidden from discussing code and sharing code. 
You are, however, not permitted to discuss the assignments with anyone outside of the class.

### Using AI
- Using AI to understand concepts, exceptions, or compilation errors is permitted
- Using AI as a "search engine" is allowed
- Using any sort of AI-integration in IDEs is not permitted (Copilot, Cursor, etc)
- Turning in AI-generated code is not permitted

### Email policy
- All questions regarding coursework (homework, exams, quizzes, etc.) should be posted on Piazza
- When sending emails, please include as the email subject `[W26 ECS 160]` followed by the actual subject line
- Please do not use AI when writing emails or Piazza posts

### Late policy
Homework assignments are due 11:59 PM on the day of the deadline. Reading reflections are due before class. Any missed quiz or mid-term can be made up within two weeks if documentation is provided. Final exams can be made up at the discretion of the instructor.

## Student support

UC Davis is dedicated to supporting the mental health and well-being of all students. 
If you or someone you know is feeling overwhelmed, depressed, or in need of assistance, confidential mental health services are available through [Student Health and Counseling Services](https://shcs.ucdavis.edu/).
Please also feel free to contact me by scheduling separate office hours if you would like to discuss any such factors affecting your class performance. Additional accommodations might also be available to address these factors.
