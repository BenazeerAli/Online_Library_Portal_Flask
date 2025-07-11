In academic institutions, access to past year question papers plays a crucial role in student 
preparation and performance. However, traditional paper-based distribution or poorly 
managed digital repositories lead to inefficiency and inaccessibility. 
This project presents a Web-Based Library Portal that enables students to easily search, 
view, and download past year question papers, while allowing faculty members to upload, 
categorize, and manage papers based on department, semester, and course. The portal is 
built using Flask (Python) for the backend and a PostgreSQL database for structured and 
relational data storage. This project demonstrates how a scalable, user-friendly digital archive can enhance 
access to academic resources while also showcasing the practical use of relational 
databases and web technologies in solving real-world academic problems.
The key focus of the system is its robust database design and SQL-driven features. The 
project uses PostgreSQL as the relational database, with a schema that includes users, 
departments, courses, papers, and download logs — each with clear relationships and referential 
integrity. Key problems addressed include: 
● Creating a normalized relational schema with proper foreign keys for structured data 
access across departments, semesters, and course-specific papers. 
● Implementing secure role-based login that restricts upload privileges to faculty while 
allowing students to access and download documents. 
● Handling dynamic filtering of papers by department, semester, and course using 
multi-table JOINs and conditional logic (e.g., common courses for semesters 1 and 2). 
● Storing uploaded PDFs as binary data (BYTEA) within the database and serving them 
securely for download. 
SQL-centric Functionalities: 
1. Popular Downloads Tracking: 
A downloads table tracks all student download actions. This table is populated using a 
trigger linked to a PostgreSQL function, automatically inserting a log entry whenever 
a paper is downloaded. This allows the system to dynamically compute most 
downloaded papers using SQL aggregation. 
2. Leaderboard (Most Active Students): 
A stored function is used to return the top 10 students ranked by number of downloads 
using GROUP BY, ORDER BY, and LIMIT. 
3. Complex Query Usage: 
The project implements: 
○ WITH clauses for building temporary views like popular downloads. 
○ GROUP BY and HAVING for conditional aggregation (e.g., uploads by 
course/semester).
