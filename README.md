import React, { useState, useEffect } from "react";
import axios from "axios";

function App() {

  // ---------------- States ----------------
  const [doctor, setDoctor] = useState({ doctorId: "", doctorName: "", specialization: "" });
  const [patient, setPatient] = useState({ patientId: "", patientName: "", age: "" });
  const [appointment, setAppointment] = useState({
    appointmentId: "",
    doctorId: "",
    patientId: "",
    appointmentDate: ""
  });

  const [appointments, setAppointments] = useState([]);

  const BASE_URL = "http://localhost:8080/api/hospital";

  // ---------------- Load Appointments ----------------
  useEffect(() => {
    fetchAppointments();
  }, []);

  const fetchAppointments = async () => {
    const res = await axios.get(`${BASE_URL}/appointments`);
    setAppointments(res.data);
  };

  // ---------------- Doctor ----------------
  const addDoctor = async () => {
    await axios.post(`${BASE_URL}/doctor`, doctor);
    alert("Doctor added");
    setDoctor({ doctorId: "", doctorName: "", specialization: "" });
  };

  // ---------------- Patient ----------------
  const addPatient = async () => {
    await axios.post(`${BASE_URL}/patient`, patient);
    alert("Patient added");
    setPatient({ patientId: "", patientName: "", age: "" });
  };

  // ---------------- Appointment ----------------
  const bookAppointment = async () => {
    await axios.post(`${BASE_URL}/appointment`, appointment);
    alert("Appointment booked");
    setAppointment({ appointmentId: "", doctorId: "", patientId: "", appointmentDate: "" });
    fetchAppointments();
  };

  return (
    <div style={{ padding: "20px" }}>
      <h1>🏥 Hospital Management</h1>

      {/* -------- Doctor -------- */}
      <h2>Add Doctor</h2>
      <input placeholder="Doctor ID" value={doctor.doctorId}
        onChange={e => setDoctor({ ...doctor, doctorId: e.target.value })} />
      <input placeholder="Doctor Name" value={doctor.doctorName}
        onChange={e => setDoctor({ ...doctor, doctorName: e.target.value })} />
      <input placeholder="Specialization" value={doctor.specialization}
        onChange={e => setDoctor({ ...doctor, specialization: e.target.value })} />
      <button onClick={addDoctor}>Add Doctor</button>

      <hr />

      {/* -------- Patient -------- */}
      <h2>Add Patient</h2>
      <input placeholder="Patient ID" value={patient.patientId}
        onChange={e => setPatient({ ...patient, patientId: e.target.value })} />
      <input placeholder="Patient Name" value={patient.patientName}
        onChange={e => setPatient({ ...patient, patientName: e.target.value })} />
      <input placeholder="Age" value={patient.age}
        onChange={e => setPatient({ ...patient, age: e.target.value })} />
      <button onClick={addPatient}>Add Patient</button>

      <hr />

      {/* -------- Appointment -------- */}
      <h2>Book Appointment</h2>
      <input placeholder="Appointment ID" value={appointment.appointmentId}
        onChange={e => setAppointment({ ...appointment, appointmentId: e.target.value })} />
      <input placeholder="Doctor ID" value={appointment.doctorId}
        onChange={e => setAppointment({ ...appointment, doctorId: e.target.value })} />
      <input placeholder="Patient ID" value={appointment.patientId}
        onChange={e => setAppointment({ ...appointment, patientId: e.target.value })} />
      <input type="date" value={appointment.appointmentDate}
        onChange={e => setAppointment({ ...appointment, appointmentDate: e.target.value })} />
      <button onClick={bookAppointment}>Book</button>

      <hr />

      {/* -------- Appointment List -------- */}
      <h2>All Appointments</h2>
      <table border="1" cellPadding="5">
        <thead>
          <tr>
            <th>ID</th>
            <th>Doctor ID</th>
            <th>Patient ID</th>
            <th>Date</th>
          </tr>
        </thead>
        <tbody>
          {appointments.map(a => (
            <tr key={a.appointmentId}>
              <td>{a.appointmentId}</td>
              <td>{a.doctorId}</td>
              <td>{a.patientId}</td>
              <td>{a.appointmentDate}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

export default App;
