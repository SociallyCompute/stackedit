
# Requirements for MHS User, Class & Teacher Identification

## Current Status:

The version of MHS login implemented for the spring 2018 field test required students to enter name, teacher name and class identifiers. These identifiers were associated with a save file on the computer used for MHS and also were associated with log data sent to our analytics servers.

This system made it easy for students to start playing MHS but led to numerous problems with students using multiple names for themselves and for their teachers. In almost all cases we see the problem as being middle school kids lack of attention to detail being a bad fit with a system that allowed “open” enrollment. The results were many duplicate instances of kids and teachers and a compromised data set.

Similarly, the storage of a save file for each instance of a student being on the local computer led to problems when schools experienced problems with computers and needed to move kids to another computer. The student needed to restart the game.

As we prepare for a next field test (field test 2 January 2019) with an even larger number of teachers and students we need a login system that will be easy to implement but also authenticate each login to the appropriate student, teacher and class.

Long term we also seek to solve the lock in to a specific local computer problem so the planning for this issue should be included in the new design.

Long term we also seek to simplify the role of the teacher and student by importing login info from learning management systems.

The use of “long term” refers to things that would be nice to have long term but not necessarily a part of the login system for field test 2

In working with school districts for field test 1 we identified that some districts will not want real student names and emails used in MHS so accommodations for pseudonyms needs to be included.


## Requirements:
### Field Test 2, Winter 2018/19:

1.	**Teacher login requirements:**
	  1.  Teacher creates account
	  3.	Teacher can change their own password
	  6.	Teacher creates class roster(s)
	  8.	Teacher has view of student password
	  9.	Teacher can edit student identifier or password
	  10.	Teacher can access dashboard for class
	  11.	Teacher can deactivate student account (so student can not log into MHS)
1. **Student login requirements**
	1.	Account is created on behalf of the student and managed by teacher and MHS
	2.	Student or teacher can change student password
	3.	Suspend all you know about best security practices. That's not what this is.
1. **System Requirements:**
	  1. Upon player (student) authentication, the system validating a student login shall return a triplet of ids (teacherid, classid, studentid) for use in our logging framework. This will include the following API Endpoints:
	```
	    Description: Batch Return of an array of classes with an array of students for each class when passed a teacherid.  
	    Endpoint: /api/mhslogin/:teacherid/
	    Return: {teacherid: "<teachername", [{classid: "<classname"}, [{studentid: "studentname"}, ... n] ...n ]}

	    Description: Human readable teacher name given teacher id
	    Endpoint: /api/teacher/:teacherid
	    Return: {teachername: "<teachername>"}

	    Description: Human readable class name given class id
	    Endpoint: /api/class/:classid
	    Return: {classname: "<classname>"}

	    Description: Human readable student name given student id
	    Endpoint: /api/student/:studentid
	    Return: {studentid: "<studentname>"}

	    Description: Triplet of teacherid, classid, and studentid used for logging. Doubles as game user authentication.
	    Endpoint: /api/mhslogin/:studentemail/:token
	    Return: {teacherid: "<teacherid>", classid: "<classid>", studentid: "<studentid>"}

		Description: A list of all your bank accounts and credential information
		Endpoint: /api/operationHawaii/:yourname/:allCredentials
		Return: {an array of account credential information, account numbers, passwords, and answers to personal challenge questions, like your favorite beer fo
	```
	  2. Login technology is addressable through an API call.
	  3. .NET and Unity have necessary technology to leverage the available API's (seems obviously true, but lets make sure.)
	  4. A map between teacher, student and class ID's and the associated human readable "name" will be maintained in the same database as the user credentials.
		    - "Names" will be retrievable through an API like those described above
		    - ID's will be sufficiently obtuse so as not to be easily guessable
		    - Any student named "Drop Table" is excluded from participation in the field test.
	  5. Metadata requirements: 
			1. Teacher
				- id
				- Name
				- Username
				- Password (salted or hashed)
				- School
				- City
				- State
				- (really, anything "standard" is fine I think)
			2. Classroom 
				- id
				- Name
			3. Student
				- id
				- Student Name
				- MHS Username
				- MHS Password (salted or hashed) [Note I have distinguished the "MHS Password" from your ordinary A&E password]
				- MHS Teacher (Must match a teacher account that exists)
				- MHS Classroom (Must match a classroom that the teacher created)

### Future Field Tests:
1. Student can log into MHS from any capable computer with MHS installed
2. Login system will accept a breathalyzer interface for teacher logins

## Use Case:

1. New Teacher volunteers to participate in the second field test of MHS.
2. New Teacher follows URL and creates an account with an email address, password, and some profile info (school district, etc.)
3. Teacher creates class rosters on MHS server.
  - Teacher or tech coordinator export class roster (of student identifier and [optionally] password) from school learning management system (e.g., Canvas, Blackboard, etc.)
4. Teacher or MHS staff upload spreadsheet into MHS class rosters.  This sets up student accounts, linked to teachers and classes, in MHS.
5. Students login to MHS game using a username and password they either know because the school policy manages this, and that's what we uploaded, OR, the teacher provides each student with a username and password.
6. MHS game will not begin without validating username and password.
  - If account is verified server passes class and teacher name to game and student starts or continues play based on save file for that student
  - Failure scenarios:
    -	If validation fails, student will get a message: "Invalid username/password. Please speak to your teacher for an account or to create a new password.""
    - If validation fails, teacher will be able to either:
      - Create an account for a new or forgotten student, who will then feel stigmatized for most of the rest of their lives
      - Update the student password and share it with them.
7. Teacher credentials will enable them to access their classes on the MHS Dashboard.

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg3NDk5NjAwOSwtMTY1OTE4Mzk1MCwxNz
Y4MTg2NTk1LC0xMDYzODcxNjg0LC0xMDI5Mjg1MTk0LC0yMTAy
OTk3NDA1XX0=
-->