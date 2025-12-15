---
Tags: 
Created: 2025-10-01 13:12:12
---
(Links:: [[Security Engineering]])
# Functional Requirements

| Requirements                                                                                                            | functional | security |
| ----------------------------------------------------------------------------------------------------------------------- | ---------- | -------- |
| The system's users are professors, teaching assistants, students and university administrators                          |            | yes      |
| Professors create courses                                                                                               | yes        | yes      |
| Students can take courses                                                                                               | yes        | yes      |
| Professors and TAs grade students of a course                                                                           | yes        | yes      |
| Professors and TAs are considered scientific staff                                                                      | yes        | yes      |
| The system should store users and the following attributes: name, surname, username, password and affiliated department |            |          |
| The above fields can never be empty                                                                                     |            | yes      |
| The username cannot be changed                                                                                          |            | yes      |
| Other information can be changed by the user or by an administrator                                                     | yes        | yes      |
| Users use username and password to login                                                                                | yes        | yes      |
| A department offers several courses                                                                                     |            |          |
| A professor can create and edit courses for their department                                                            | yes        | yes      |
| A student can attend multiple courses                                                                                   | yes        | yes      |
| A course can have multiple students                                                                                     |            |          |
| Administrators and professors set a limit of attending students                                                         | yes        | yes      |
| Students are admitted on first-come-first-serve basis                                                                   | yes        | yes      |
| Students cannot enrol in full courses or those they have already attended                                               |            | yes      |
| A course is taught be a single professor                                                                                |            |          |
| The course’s staff includes the course professor and TAs                                                                |            |          |
| There can only be as many TAs as there are number of students divided by 20 in a course (rounded down)                  |            |          |
| TAs apply to teach course which is accepted or denied by professor                                                      | yes        | yes      |
| Students can receive provisional grades                                                                                 |            |          |
| Provisional grades are assigned by TAs or professor                                                                     | yes        | yes      |
| Professor finalizes grades                                                                                              | yes        | yes      |
| If the course grade if final, the student is no longer attending the course                                             | yes        | yes      |
| Every student has a list of past courses and their final grades                                                         |            | yes      |

## Use Case Diagram
- Draw a use case diagram which captures that a TA can teach a course. Recall that for teaching a course, a TA needs to request a position and the course’s professor must approve the request:
![[Secuirty Engineering Exercises-1.svg|500]]
- Draw another use case diagram which captures that a student can pass a course if he is enrolled in the course and if he has received a final grade. Note that all staff members can enter provisional grades for students, but only the professor can “finalize” these grades, that is approve them such that they are considered final grades:
==TODO: insert diagram==
## Activity Diagram
Detail the requirements of enrolling in a course using an activity diagram:
- A student enrolls in a course by selecting the course from a list of all courses.
- A student cannot enroll in a course if he is already enrolled in this course or if he has already passed this course.
- (Optional) A student cannot enroll in a course which has reached its maximum capacity.
![[Secuirty Engineering Exercises-2.svg|600]]

# Security Requirements
Security requirements specify the security properties (i.e. CIA) of data and also include access control policy (i.e. a user with what role can or cannot perform which action towards what data).
In the view of access control, the notion of "role" plays a very important part. In this example, for simplicity, let us consider the provided system's user roles are the role of the system.

## Data Criticality
| DataType   | Confidentiality | Integrity  | Availability |
| ---------- | --------------- | ---------- | ------------ |
| name       | low             | **high**   | low          |
| surname    | low             | **high**   | low          |
| username   | *moderate*      | **high**   | **high**     |
| password   | **high**        | **high**   | **high**     |
| affiliated | low             | *moderate* | *moderate*   |
| grade      | **high**        | *moderate* | *moderate*   |
| final      | **high**        | **high**   | **high**     |
| limit      | low             | *moderate* | *moderate*   |
| staffs     | low             | *moderate* | *moderate*   |
| attendants | *moderate*      | **high**   | *moderate*   |
| belongsTo  | low             | *moderate* | *moderate*   |
| applicants | *moderate*      | *moderate* | *moderate*   |
| applied    | *moderate*      | *moderate* | *moderate*   |
| teaches    | low             | *moderate* | *moderate*   |
| attends    | *moderate*      | **high**   | *moderate*   |
| student    | *moderate*      | **high**   | *moderate*   |
| course     | *moderate*      | **high**   | *moderate*   |
| offers     | N/A             | *moderate* | *moderate*   |
| has        | low             | *moderate* | *moderate*   |

## Authorization Policy
| DataType   | Confidentiality | Integrity  | Availability |
| ---------- | --------------- | ---------- | ------------ |
| name       |                 |            |              |
| surname    |                 |            |              |
| username   |                 |            |              |
| password   |                 |            |              |
| affiliated |                 |            |              |
| grade      |                 |            |              |
| final      |                 |            |              |
| limit      |                 |            |              |
| staffs     |                 |            |              |
| attendants |                 |            |              |
| belongsTo  |                 |            |              |
| applicants |                 |            |              |
| applied    |                 |            |              |
| teaches    |                 |            |              |
| attends    |                 |            |              |
| student    |                 |            |              |
| course     |                 |            |              |
| offers     |                 |            |              |
| has        |                 |            |              |

## Use Case Diagram
## Misuse Case Diagram

# Privacy Requirements
## Personal data
Various kind of identification means, including names, LegiID, age (birthdate), username are among the typical personal data types. Take an example as the student user, grades and enrollments can also be considered as personal data: this also depends on the usage of data.
## Purpose
Some purpose notice examples:
- If you are a student, we use your Legi identification, name, enrolment status and grades for the purpose of enrolling to new courses.
- We use your username, password and email address for authentication purpose.
- If you are a teaching staff, in addition to the basic personal information, we use your teaching assignment status for the purpose of processing your teaching assistant application.
## Use Case Diagram for Privacy

---
References: