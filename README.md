SELECT c.CourseName,
       s.StudentID,
       s.Name,
       e.Grade
FROM Enrollments e
JOIN Students s
    ON e.StudentID = s.StudentID
JOIN Courses c
    ON e.CourseID = c.CourseID
WHERE e.Grade = (
    SELECT MAX(e2.Grade)
    FROM Enrollments e2
    WHERE e2.CourseID = e.CourseID
)
ORDER BY c.CourseName;


SELECT c.CourseName,
       COUNT(CASE WHEN e.Grade >= 40 THEN 1 END) AS Passed,
       COUNT(*) AS Total_Students,
       ROUND(
           COUNT(CASE WHEN e.Grade >= 40 THEN 1 END) * 100.0 / COUNT(*),
           2
       ) AS Pass_Rate
FROM Courses c
JOIN Enrollments e
    ON c.CourseID = e.CourseID
GROUP BY c.CourseID, c.CourseName;


SELECT s.StudentID,
       s.Name,
       ROUND(AVG(e.Grade),2) AS Average_Grade
FROM Students s
JOIN Enrollments e
    ON s.StudentID = e.StudentID
GROUP BY s.StudentID, s.Name
ORDER BY Average_Grade DESC
LIMIT 1;


SELECT s.StudentID,
       s.Name,
       COUNT(e.CourseID) AS Courses_Enrolled
FROM Students s
JOIN Enrollments e
    ON s.StudentID = e.StudentID
GROUP BY s.StudentID, s.Name
HAVING COUNT(e.CourseID) > 1;
