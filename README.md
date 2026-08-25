lThis SQL script builds a simple hospital management database. It starts by creating a database called *hospital_demo* and then defines several tables to organize hospital data. The **department** table stores unique department IDs and names. The **doctor** table records doctors with their ID, name, email, phone number, and the department they belong to. The **patient** table keeps patient details such as ID, name, age, gender, and phone number, with a check to ensure age is positive. The **room** table assigns patients to rooms, storing room ID, type, and number, while linking each room to a patient. The **appointment** table tracks patient visits, storing appointment ID, patient ID, doctor ID, date, and reason, with foreign keys connecting patients and doctors. After defining the structure, the script inserts sample data into each table—for example, departments like Cardiology and Neurology, doctors with their contact details, patients with age and gender, rooms assigned to patients, and appointments scheduled with reasons. Finally, it shows commands to display tables, describe their structure, and query data to view the stored records. In short, this code sets up a relational database that models hospital operations, including departments, doctors, patients, rooms, and appointments, and demonstrates how to populate and view the data.
Yes. For your hospital database, you can show normalization from UNF → 1NF → 2NF → 3NF like this.

1. Unnormalized Form (UNF)

Initially, suppose all hospital information is stored in one table:

CREATE TABLE hospital_UNF (
    patient_id INT,
    patient_name VARCHAR(50),
    age INT,
    gender VARCHAR(10),
    patient_phone VARCHAR(15),

    doctor_id INT,
    doctor_name VARCHAR(50),
    doctor_email VARCHAR(50),
    doctor_phone VARCHAR(15),

    dept_id INT,
    dept_name VARCHAR(50),

    room_id INT,
    room_type VARCHAR(30),
    room_no INT,

    appointment_id INT,
    appointment_date DATE,
    reason VARCHAR(100)
);


This creates redundancy because department, doctor, patient, and room information may be repeated.

2. First Normal Form (1NF)

Condition: All attributes must contain atomic values, with no repeating groups.

CREATE TABLE hospital_1NF (
    appointment_id INT PRIMARY KEY,
    patient_id INT,
    patient_name VARCHAR(50),
    age INT,
    gender VARCHAR(10),
    patient_phone VARCHAR(15),

    doctor_id INT,
    doctor_name VARCHAR(50),
    doctor_email VARCHAR(50),
    doctor_phone VARCHAR(15),

    dept_id INT,
    dept_name VARCHAR(50),

    room_id INT,
    room_type VARCHAR(30),
    room_no INT,

    appointment_date DATE,
    reason VARCHAR(100)
);


Now every column contains a single atomic value.

3. Second Normal Form (2NF)

Condition: Table must be in 1NF and there should be no partial dependency.

Separate information based on its primary key.

Patient
CREATE TABLE patient_2NF (
    patient_id INT PRIMARY KEY,
    patient_name VARCHAR(50),
    age INT,
    gender VARCHAR(10),
    phone_no VARCHAR(15)
);

Doctor
CREATE TABLE doctor_2NF (
    doctor_id INT PRIMARY KEY,
    doctor_name VARCHAR(50),
    email VARCHAR(50),
    phone_no VARCHAR(15),
    dept_id INT,
    dept_name VARCHAR(50)
);

Room
CREATE TABLE room_2NF (
    room_id INT PRIMARY KEY,
    room_type VARCHAR(30),
    room_no INT,
    patient_id INT
);

Appointment
CREATE TABLE appointment_2NF (
    appointment_id INT PRIMARY KEY,
    patient_id INT,
    doctor_id INT,
    appointment_date DATE,
    reason VARCHAR(100)
);

4. Third Normal Form (3NF)

Condition: Table must be in 2NF and there should be no transitive dependency.

For example, in doctor_2NF:

doctor_id → dept_id
dept_id   → dept_name


Therefore, dept_name should be moved to a separate department table.

Final 3NF Database
CREATE DATABASE hospital_3NF;
USE hospital_3NF;

-- =========================
-- DEPARTMENT TABLE
-- =========================

CREATE TABLE department (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50) UNIQUE NOT NULL
);

-- =========================
-- DOCTOR TABLE
-- =========================

CREATE TABLE doctor (
    doctor_id INT PRIMARY KEY,
    doctor_name VARCHAR(50) NOT NULL,
    email VARCHAR(50) UNIQUE,
    phone_no VARCHAR(15) UNIQUE,
    dept_id INT,

    FOREIGN KEY (dept_id)
        REFERENCES department(dept_id)
);

-- =========================
-- PATIENT TABLE
-- =========================

CREATE TABLE patient (
    patient_id INT PRIMARY KEY,
    patient_name VARCHAR(50) NOT NULL,
    age INT CHECK (age > 0),
    gender VARCHAR(10),
    phone_no VARCHAR(15) UNIQUE
);

-- =========================
-- ROOM TABLE
-- =========================

CREATE TABLE room (
    room_id INT PRIMARY KEY,
    room_type VARCHAR(30) NOT NULL,
    room_no INT UNIQUE,
    patient_id INT,

    FOREIGN KEY (patient_id)
        REFERENCES patient(patient_id)
);

-- =========================
-- APPOINTMENT TABLE
-- =========================

CREATE TABLE appointment (
    appointment_id INT PRIMARY KEY,
    patient_id INT,
    doctor_id INT,
    appointment_date DATE,
    reason VARCHAR(100),

    FOREIGN KEY (patient_id)
        REFERENCES patient(patient_id),

    FOREIGN KEY (doctor_id)
        REFERENCES doctor(doctor_id)
);

Insert Data
INSERT INTO department VALUES
(1, 'Cardiology'),
(2, 'Neurology'),
(3, 'Orthopedics');

INSERT INTO doctor VALUES
(201, 'Dr. Sharma', 'sharma@hospital.com', '9876543210', 1),
(202, 'Dr. Mehta', 'mehta@hospital.com', '9876543211', 2),
(203, 'Dr. Rao', 'rao@hospital.com', '9876543212', 3);

INSERT INTO patient VALUES
(101, 'Rahul', 25, 'Male', '9123456780'),
(102, 'Priya', 30, 'Female', '9123456781'),
(103, 'Amit', 40, 'Male', '9123456782');

INSERT INTO room VALUES
(301, 'General', 101, 101),
(302, 'Private', 102, 102),
(303, 'ICU', 103, 103);

INSERT INTO appointment VALUES
(401, 101, 201, '2026-08-20', 'Chest pain'),
(402, 102, 202, '2026-08-21', 'Headache'),
(403, 103, 203, '2026-08-22', 'Knee pain');

Check the Normalized Tables
SHOW TABLES;

SELECT * FROM department;

SELECT * FROM doctor;

SELECT * FROM patient;

SELECT * FROM room;

SELECT * FROM appointment;

Normalization flow
UNF
 ↓
All hospital information in one table
 ↓
1NF
 ↓
Atomic values / no repeating groups
 ↓
2NF
 ↓
Separate patient, doctor, room, appointment information
 ↓
3NF
 ↓
Remove transitive dependency
 ↓
Final tables:
    Department
    Doctor
    Patient
    Room
    Appointment


Final result: Your hospital database is in 3NF.
