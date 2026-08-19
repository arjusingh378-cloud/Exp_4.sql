// FOR CREATING TABLE //

CREATE DATABASE hospital_demo;
USE hospital_demo;

CREATE TABLE department (
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(50) UNIQUE NOT NULL
);

CREATE TABLE doctor (
    doctor_id INT PRIMARY KEY,
    doctor_name VARCHAR(50) NOT NULL,
    email VARCHAR(50) UNIQUE,
    phone_no VARCHAR(15) UNIQUE,
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES department(dept_id)
);

CREATE TABLE patient (
    patient_id INT PRIMARY KEY,
    patient_name VARCHAR(50) NOT NULL,
    age INT CHECK (age > 0),
    gender VARCHAR(10),
    phone_no VARCHAR(15) UNIQUE
);

CREATE TABLE room (
    room_id INT PRIMARY KEY,
    room_type VARCHAR(30) NOT NULL,
    room_no INT UNIQUE,
    patient_id INT,
    FOREIGN KEY (patient_id) REFERENCES patient(patient_id)
);

CREATE TABLE appointment (
    appointment_id INT PRIMARY KEY,
    patient_id INT,
    doctor_id INT,
    appointment_date DATE,
    reason VARCHAR(100),
    FOREIGN KEY (patient_id) REFERENCES patient(patient_id),
    FOREIGN KEY (doctor_id) REFERENCES doctor(doctor_id)
);

// FOR ADDING ELEMENTS INTO THE TABLE //

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

// FOR OPENING THE OLD TABLE //

SHOW TABLES;
DESCRIBE doctor;
SHOW CREATE TABLE appointment;
SELECT * FROM appointment;

// FOR SHOWING THE CREATED TABLE OUTPUTS //
  
SELECT * FROM department;
SELECT * FROM doctor;
SELECT * FROM patient;
SELECT * FROM room;

SELECT * FROM appointment;
