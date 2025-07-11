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
PYTHON CODE FOR FUNCTIONAL DESIGN ( DB CONNECTIVITY) 
Reusable Database Connectivity Function: 
def get_db_connection(): 
    ... 
    conn = psycopg2.connect( 
        host=os.getenv("DB_HOST"), 
        database=os.getenv("DB_NAME"), 
        user=os.getenv("DB_USER"), 
        password=os.getenv("DB_PASSWORD") 
    ) 
    return conn 
 
 
PL/SQL Function Call: 
CREATE OR REPLACE FUNCTION update_download_count() 
RETURNS TRIGGER AS $$ 
BEGIN 
    UPDATE papers 
    SET download_count = download_count + 1 
    WHERE id = NEW.paper_id; 
 
    RETURN NEW; 
END; 
$$ LANGUAGE plpgsql; 
 
 
CREATE TRIGGER trigger_update_downloads 
AFTER INSERT ON downloads 
FOR EACH ROW 
EXECUTE FUNCTION update_download_count(); 
Trigger is used via this line: 
cur.execute("INSERT INTO downloads (username, paper_id) VALUES (%s, %s)", ...) 
Data Access: 
Upload, Delete, Retrieve, Filter — all examples of structured access 
Upload: Allows faculty to upload a paper, select department, semester, course and upload PDF 
file. This saves the PDF as binary data to the papers table. Then it displays uploaded papers 
below the form: 
cur.execute("""INSERT INTO papers (title, department_id, course_id, semester, uploaded_by, 
pdf) VALUES (%s, %s, %s, %s, %s, %s)""", (...)) 
Delete(Functionality for faculty to delete papers): 
cur.execute("DELETE FROM papers WHERE id = %s AND uploaded_by = %s", ...) 
Filter: used on papers table to access the paper after sorting through the department, semester and 
course:  
cur.execute(""" 
SELECT p.id, p.title, c.name, p.semester, p.upload_date 
FROM papers p 
JOIN course c ON p.course_id = c.id 
WHERE c.department_id = %s AND p.semester = %s AND c.id = %s 
ORDER BY p.upload_date DESC 
""", (...)) 
In case the semester selected is a semester of the first year( semester 1 or semester 2) then all the 
first year courses are available to choose from to view the papers:  
if semester in ["Semester 1", "Semester 2"]: 
cur.execute("SELECT id, name FROM course WHERE semester = %s AND department_id IS 
NULL ORDER BY name", (semester,)) 
else: 
cur.execute("SELECT id, name FROM course WHERE department_id = %s AND semester = 
%s ORDER BY name", ...)
