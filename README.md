# student-management-system
Student Management System using Flask and SQLite

from flask import Flask, render_template, request, redirect, url_for import sqlite3

app = Flask(name)

Database Setup

def init_db(): conn = sqlite3.connect('students.db') c = conn.cursor() c.execute('''CREATE TABLE IF NOT EXISTS students ( id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT NOT NULL, roll_no TEXT UNIQUE NOT NULL, course TEXT, marks INTEGER )''') conn.commit() conn.close()

Home Page

@app.route('/') def index(): conn = sqlite3.connect('students.db') c = conn.cursor() c.execute("SELECT * FROM students") students = c.fetchall() conn.close() return render_template('index.html', students=students)

Add Student

@app.route('/add', methods=['POST']) def add_student(): name = request.form['name'] roll_no = request.form['roll_no'] course = request.form['course'] marks = request.form['marks']

conn = sqlite3.connect('students.db')
c = conn.cursor()
c.execute("INSERT INTO students (name, roll_no, course, marks) VALUES (?, ?, ?, ?)",
          (name, roll_no, course, marks))
conn.commit()
conn.close()
return redirect(url_for('index'))

Delete Student

@app.route('/delete/int:id') def delete_student(id): conn = sqlite3.connect('students.db') c = conn.cursor() c.execute("DELETE FROM students WHERE id=?", (id,)) conn.commit() conn.close() return redirect(url_for('index'))

Update Student

@app.route('/update/int:id', methods=['GET', 'POST']) def update_student(id): conn = sqlite3.connect('students.db') c = conn.cursor() if request.method == 'POST': name = request.form['name'] roll_no = request.form['roll_no'] course = request.form['course'] marks = request.form['marks'] c.execute("UPDATE students SET name=?, roll_no=?, course=?, marks=? WHERE id=?", (name, roll_no, course, marks, id)) conn.commit() conn.close() return redirect(url_for('index')) else: c.execute("SELECT * FROM students WHERE id=?", (id,)) student = c.fetchone() conn.close() return render_template('update.html', student=student)

if name == 'main': init_db() app.run(debug=True)





