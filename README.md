# MySQL
CREATE DATABASE IF NOT EXISTS Uni;
CREATE TABLE IF NOT EXISTS uni.students (student_id INT PRIMARY KEY, firstname VARCHAR(50), lastname VARCHAR(50), DOB DATE, Major VARCHAR(50), GPA DECIMAL(3,1));
INSERT INTO uni.students (student_id, firstname, lastname, DOB, Major, GPA) values 
(1,'Alex','Smith','2000-01-01','Computer_Science','3.5'),
(2,'Bella','Johnson','2000-02-02','Business','3.6'),
(3,'Charlie','Brown','2000-03-03','Physics','3.7'),
(4,'Diana','Davis','2000-04-04','Mathematics','3.8'),
(5,'Ethan','Miller','2000-05-05','Biology','3.9'),
(6,'Frank','Taylor','2001-06-06','Engineering','3.2'),
(7,'Grace','Lee','2001-07-07','Business','3.3'),
(8,'Hannah','Walker','2001-08-08','Physics','3.4'),
(9,'Isaac','Carter','2001-09-09','Mathematics','3.5'),
(10,'Jack','Anderson','2001-10-10','Biology','3.6'),
(11,'Katie','Moore','2001-11-11','Engineering','3.7'),
(12,'Leo','Hall','2001-12-12','Computer_Science','3.8'),
(13,'Mia','Allen','2002-01-13','Economics','3.9'),
(14,'Nathan','Scott','2002-02-14','Engineering','3.1'),
(15,'Olivia','Adams','2002-03-15','Mathematics','3.2'),
(16,'Paul','Baker','2002-04-16', 'Business','3.3'),
(17, 'Quinn','Martin','2002-05-17', 'Physics','3.4'),
(18, 'Rachel','White','2002-06-18', 'Computer_Science','3.5'),
(19, 'Sam','Harris','2002-07-19', 'Biology','3.6'),
(20, 'Tina','King','2002-08-20', 'Economics','3.7'); 
CREATE TABLE IF NOT EXISTS uni.courses (Courseid INT PRIMARY KEY, CourseName VARCHAR(50), Department VARCHAR(50));
INSERT INTO uni.courses (Courseid, CourseName, Department) values
(101, 'Intro_to_CS','Computer_Science'),
(102, 'Microeconomics','Economics'),
(103, 'General_Physics','Physics'),
(104, 'Calculus_I','Mathematics'),
(105, 'Biology_101','Biology'),
(106, 'Software_Engineering','Computer Science'),
(107, 'Macroeconomics','Economics'),
(108, 'Thermodynamics','Physics'),
(109, 'Linear_Algebra','Mathematics'),
(110, 'Genetics','Biology');

CREATE TABLE IF NOT EXISTS uni.instructors (Instructorid INT PRIMARY KEY, InstructorName VARCHAR(50), Department VARCHAR(50));
INSERT INTO uni.instructors (Instructorid, InstructorName, Department) values
(201, 'Dr.Adams','Computer_Science'),
(202, 'Dr.Baker','Economics'),
(203, 'Dr.Clark','Physics'),
(204, 'Dr.Davis','Mathematics'),
(205, 'Dr.Evans','Biology'),
(206, 'Dr.Foster','Engineering'),
(207, 'Dr.Grant','Business'),
(208, 'Dr.Hayes','Physics'),
(209, 'Dr.Irving','Mathematics'),
(210, 'Dr.Jenkins','Economics');
CREATE TABLE IF NOT EXISTS uni.enrollments (Enrollmentid INT PRIMARY KEY, Studentid INT, Courseid INT, Semester VARCHAR(50), Grade VARCHAR(1));
INSERT INTO uni.enrollments (Enrollmentid, Studentid, Courseid, Semester, Grade) values
(301, 1, 101, 'Fall 2023','A'),
(302, 3, 102, 'Fall 2023','B'),
(303, 3, 103, 'Fall 2023','C'),
(304, 1, 104, 'Fall 2023','D'),
(305, 1, 105, 'Fall 2023','F'),
(306, 2, 106, 'Fall 2023','A'),
(307, 11, 107, 'Fall 2023','B'),
(308, 8, 108, 'Fall 2023','C'),
(309, 9, 109, 'Fall 2023','D'),
(310, 10, 110, 'Fall 2023','F'),
(311, 11, 101, 'Spring 2024','A'),
(312, 8, 102, 'Spring 2024','B'),
(313, 13, 103, 'Spring 2024','C'),
(314, 14, 104, 'Spring 2024','D'),
(315, 18, 105, 'Spring 2024','F'),
(316, 16, 106, 'Spring 2024','A'),
(317, 17, 107, 'Spring 2024','B'),
(318, 7, 108, 'Spring 2024','C'),
(319, 19, 109, 'Spring 2024','D'),
(320, 20, 110, 'Spring 2024','F');

USE uni;
CREATE VIEW CourseSummary AS
SELECT 
c.coursename, 
c.courseid, 
COUNT(e.courseid) AS totalenrollments
FROM courses c
JOIN enrollments e ON e.courseid = c.courseid
GROUP BY e.courseid;
SELECT * FROM CourseSummary;

CREATE VIEW InstructorSchedule AS
SELECT 
i.instructorname,
i.department,
c.coursename
FROM instructors i
JOIN courses c ON c.department = i.department
ORDER BY i.instructorname;
SELECT * FROM Instructorschedule;

ALTER VIEW CourseSummary AS
SELECT
    c.courseid,
    c.courseName,
    e.semester,
    COUNT(e.StudentID) AS TotalEnrollment
FROM
    courses c
JOIN enrollments e ON c.courseid = e.courseid
GROUP BY c.courseid, c.courseName, e.Semester
ORDER BY C.courseid, e.Semester;
SELECT * FROM CourseSummary;

ALTER VIEW InstructorSchedule AS
SELECT 
i.instructorname,
i.department,
c.coursename,
e.semester
FROM instructors i
JOIN courses c ON c.department = i.department
JOIN enrollments e ON e.courseid = c.Courseid
WHERE e.semester = 'Spring 2024';
SELECT * FROM instructorschedule;

CREATE VIEW EligibleGraduates AS
SELECT 
	student_id,
	firstname,
    	lastname
FROM students
WHERE GPA >= 3.7
WITH CASCADED CHECK OPTION;
SELECT * FROM EligibleGraduates;

ALTER VIEW EligibleGraduates AS
SELECT 
	student_id,
	firstname,
   	lastname,
    GPA
FROM students
WHERE GPA >= 3.7
WITH CASCADED CHECK OPTION;
SELECT * FROM EligibleGraduates;

RENAME TABLE CourseSummary TO CourseEnrollmentSummary;

WITH Semester_Courses AS (
SELECT 
	c.coursename,
	c.courseid,
    e.semester
FROM courses c
JOIN enrollments e ON e.courseid = c.courseid
)
SELECT * FROM Semester_Courses;
