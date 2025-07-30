from flask import Flask

const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const PORT = 3000;

app.use(bodyParser.json());

let attendanceRecords = []; // In a real application, this would be a database

// Check-in endpoint
app.post('/attendance/check-in', (req, res) => {
    const { employeeId, timestamp } = req.body;
    const record = { id: Date.now(), employeeId, checkIn: timestamp, date: new Date(timestamp).toISOString().split('T')[0] };
    attendanceRecords.push(record);
    res.status(201).json({ message: 'Check-in recorded successfully', record });
});

// Check-out endpoint
app.post('/attendance/check-out', (req, res) => {
    const { employeeId, timestamp } = req.body;
    const recordIndex = attendanceRecords.findIndex(r => r.employeeId === employeeId && !r.checkOut && r.date === new Date(timestamp).toISOString().split('T')[0]);

    if (recordIndex !== -1) {
        attendanceRecords[recordIndex].checkOut = timestamp;
        res.status(200).json({ message: 'Check-out recorded successfully', record: attendanceRecords[recordIndex] });
    } else {
        res.status(404).json({ message: 'No active check-in found for this employee today.' });
    }
});

// Get daily attendance
app.get('/attendance/:employeeId/daily', (req, res) => {
    const { employeeId } = req.params;
    const { date } = req.query; // e.g., ?date=2025-07-30
    const dailyRecords = attendanceRecords.filter(r => r.employeeId === employeeId && r.date === date);
    res.status(200).json(dailyRecords);
});

app.listen(PORT, () => {
    console.log(`Attendance API running on http://localhost:${PORT}`);
});









Restful API provide
Flexibility and Scalability: RESTful APIs allow for building a flexible and scalable system that can be expanded to incorporate new features or integrations as needed.
Real-time Data Access: Real-time data from attendance API can be crucial for various applications like payroll integration, project management, or even for clients to showcase the efforts of the workforce.
Reduced Administrative Burden: Automating attendance tracking through APIs reduces manual work, freeing up time for HR and management to focus on more strategic tasks.
Enhanced Security and Accuracy: Secure APIs with authentication and authorization mechanisms can help ensure data privacy and prevent unauthorized access or manipulation. 


Developing the REST API
Endpoints for Attendance:
Record Attendance (POST):
Receive user/employee ID, check-in/out time, and optionally location data.
Update the Attendance Record table.
Get Attendance Records (GET):
Retrieve attendance history based on user ID, date range, etc.
Update Attendance (PUT/PATCH):
Allow authorized users (e.g., administrators) to edit attendance details.

Authentication and Authorization:
Implement a secure login system for different user roles (e.g., employees, managers, administrators).
Use JSON Web Tokens (JWT) for authentication and role-based access control.

Considerations for specific features
Biometric Integration: Connect the system with biometric devices (fingerprint scanners, facial recognition) via API for automatic attendance recording.
Mobile App Integration: Develop mobile apps for employees to mark their attendance and for managers to track it.
Real-time Tracking: Utilize technologies like webhooks or websockets for instant updates on attendance activities.


Database:
Relational Databases (e.g., MySQL, PostgreSQL, SQLite) offer structured storage and relationships between data.
