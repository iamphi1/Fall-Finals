+ Starter Code
+ Guidelines Files
+ HospitalDatabase class in portalDatabase.py file
+ HospitalPortalHandler class in portalServer.py file
+ Change MySQL Server Credentials in Database class
+ run python portalServer.py
+ Access browser: localhost:8000 


Use hospital_portal;

CREATE TABLE patients (
    patient_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    patient_name VARCHAR(45) NOT NULL,
    age INT NOT NULL,
    admission_date DATE,
    discharge_date DATE
);

	INSERT INTO Patients (patient_name, age, admission_date, discharge_date)
VALUES( "NANCY KIND", 60, "2023/12/08", "2023/12/14"),
	  ( "JAMES ANDERSON", 33, "2023/04/09", "2023/11/28"),
      ( "MALIK MOUSE", 80, "2023/01/23", "2023/10/24");
      
SELECT * FROM patients;

CREATE TABLE appointments (
    appointment_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    patient_id INT NOT NULL,
    doctor_id INT NOT NULL,
    appointment_date DATE NOT NULL,
    appointment_time TIME NOT NULL,  
    FOREIGN KEY (patient_id) REFERENCES patients(patient_id),
    FOREIGN KEY (doctor_id) REFERENCES doctors(doctor_id)
);

SELECT * FROM appointments;

DELIMITER //

CREATE PROCEDURE AppointmentSchedule(
	IN patient_appointment_id INT,
    IN doctor_appointment_id INT,
    IN Schedule_appointment_date DATE,
    IN Schedule_Appointment_time DATETIME
    )

BEGIN
	INSERT INTO Appointments (patient_id, doctor_id, appointment_date, appointment_time)
VALUES (patient_appointment_id, doctor_appointment_id, Schedule_appointment_date, Schedule_Appointment_time);

END //

DELIMITER ;

DELIMITER //

CREATE PROCEDURE PatientsDischarge(
	IN patient_appointment_id INT
    )
    
BEGIN
	UPDATE PATIENTS
    SET discharge_date = CURRENT_DATE()
    WHERE Patient_id = Patient_appointment_id;
    
END//

DELIMITER ;

CREATE TABLE doctors (
    doctor_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    doctor_name VARCHAR(45) NOT NULL,
    specialty VARCHAR(45) NOT NULL
);

	INSERT INTO Doctors (doctor_name, specialization)
VALUES ("Prince James", "Pediatrician"),
		("Anne Rich", "Dermatologist"),
        ("Eric Adams", "Gynecologist");

SELECT * FROM doctors;

CALL AppointmentSchedule (2,2, "2023/12/04", '12:00');



SELECT * FROM Patients;


CREATE VIEW ViewAppointment 
AS
SELECT
   doctors.doctor_id,
    doctors.doctor_name,
    doctors.specialty,
    appointments.appointment_id,
    appointments.appointment_date,
    patients.patient_id,
    patients.patient_name
FROM
    doctors
    JOIN appointments ON doctors.doctor_id = appointments.doctor_id
    JOIN patients ON appointments.patient_id = patients.patient_id;

SELECT * FROM ViewAppointment
    