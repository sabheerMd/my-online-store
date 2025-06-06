document.addEventListener('DOMContentLoaded', () => {
    const eventCardsContainer = document.querySelector('.event-cards-container');
    const eventNameSelect = document.getElementById('eventNameSelect');
    const registrationForm = document.getElementById('event-registration-form');
    const registeredStudentsList = document.querySelector('.registered-students-list');
    const noRegistrationsMessage = document.getElementById('no-registrations-message');

    // Sample Event Data (in a real app, this would come from a backend/database)
    const events = [
        {
            id: 'hackathon-2025',
            name: 'Annual College Hackathon 2025',
            date: 'October 26, 2025',
            time: '9:00 AM - 6:00 PM',
            location: 'Innovation Lab, Block C',
            description: 'Solve real-world problems using technology. Teams of 3-4 encouraged.'
        },
        {
            id: 'ideathon-spring-2025',
            name: 'Spring Ideathon: Sustainable Solutions',
            date: 'November 15, 2025',
            time: '10:00 AM - 4:00 PM',
            location: 'Auditorium, Main Building',
            description: 'Brainstorm and present innovative ideas for a sustainable future.'
        },
        {
            id: 'coding-challenge-dec',
            name: 'Winter Coding Challenge',
            date: 'December 5, 2025',
            time: '2:00 PM - 5:00 PM',
            location: 'Computer Science Lab 1',
            description: 'Test your algorithmic skills in this challenging coding competition.'
        }
    ];

    // Simulate registered students data (in a real app, this would be persistent)
    let registeredStudents = [];

    // Function to render events
    function renderEvents() {
        eventCardsContainer.innerHTML = ''; // Clear existing cards
        events.forEach(event => {
            const eventCard = document.createElement('div');
            eventCard.classList.add('event-card');
            eventCard.innerHTML = `
                <h3>${event.name}</h3>
                <p><strong>Date:</strong> ${event.date}</p>
                <p><strong>Time:</strong> ${event.time}</p>
                <p><strong>Location:</strong> ${event.location}</p>
                <p>${event.description}</p>
            `;
            eventCardsContainer.appendChild(eventCard);

            // Populate the registration form's select dropdown
            const option = document.createElement('option');
            option.value = event.id;
            option.textContent = event.name;
            eventNameSelect.appendChild(option);
        });
    }

    // Function to render registered students
    function renderRegisteredStudents() {
        registeredStudentsList.innerHTML = ''; // Clear existing list
        if (registeredStudents.length === 0) {
            noRegistrationsMessage.style.display = 'block';
            registeredStudentsList.appendChild(noRegistrationsMessage);
        } else {
            noRegistrationsMessage.style.display = 'none';
            registeredStudents.forEach(student => {
                const studentItem = document.createElement('div');
                studentItem.classList.add('registered-student-item');
                studentItem.innerHTML = `
                    <p><strong>Event:</strong> ${student.eventName}</p>
                    <p><strong>Name:</strong> ${student.studentName}</p>
                    <p><strong>Student ID:</strong> ${student.studentId}</p>
                    <p><strong>Email:</strong> ${student.studentEmail}</p>
                    ${student.studentPhone ? `<p><strong>Phone:</strong> ${student.studentPhone}</p>` : ''}
                `;
                registeredStudentsList.appendChild(studentItem);
            });
        }
    }

    // Handle form submission
    registrationForm.addEventListener('submit', (e) => {
        e.preventDefault(); // Prevent default form submission

        const selectedEventId = eventNameSelect.value;
        const selectedEvent = events.find(event => event.id === selectedEventId);

        if (!selectedEvent) {
            alert('Please select a valid event.');
            return;
        }

        const studentName = document.getElementById('studentName').value.trim();
        const studentId = document.getElementById('studentId').value.trim();
        const studentEmail = document.getElementById('studentEmail').value.trim();
        const studentPhone = document.getElementById('studentPhone').value.trim();

        if (studentName === '' || studentId === '' || studentEmail === '') {
            alert('Please fill in all required fields (Name, Student ID, Email).');
            return;
        }

        // Basic email validation
        if (!studentEmail.includes('@') || !studentEmail.includes('.')) {
            alert('Please enter a valid email address.');
            return;
        }

        const newRegistration = {
            eventId: selectedEvent.id,
            eventName: selectedEvent.name,
            studentName,
            studentId,
            studentEmail,
            studentPhone
        };

        registeredStudents.push(newRegistration);
        alert(`Successfully registered ${studentName} for ${selectedEvent.name}!`);
        registrationForm.reset(); // Clear the form
        renderRegisteredStudents(); // Update the registered students list
    });

    // Initial rendering when the page loads
    renderEvents();
    renderRegisteredStudents();
});
