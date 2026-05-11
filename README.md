<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <title>Ticket System</title>
    <style>
        body { font-family: Arial; margin: 20px; }
        form { margin-bottom: 20px; }
        input, textarea { display: block; margin: 5px 0; width: 300px; }
        button { padding: 10px; }
        #tickets { margin-top: 20px; }
        .ticket { border: 1px solid #ccc; padding: 10px; margin: 10px 0; }
    </style>
</head>
<body>
    <h1>Ticket System</h1>
    <form id="ticketForm">
        <label for="name">Name:</label>
        <input type="text" id="name" required>
        <label for="email">E-Mail:</label>
        <input type="email" id="email" required>
        <label for="subject">Betreff:</label>
        <input type="text" id="subject" required>
        <label for="description">Beschreibung:</label>
        <textarea id="description" required></textarea>
        <button type="submit">Ticket einreichen</button>
    </form>
    <h2>Eingereichte Tickets</h2>
    <div id="tickets"></div>
    <script>
        const form = document.getElementById('ticketForm');
        const ticketsDiv = document.getElementById('tickets');
        let tickets = JSON.parse(localStorage.getItem('tickets')) || [];
        function displayTickets() {
            ticketsDiv.innerHTML = '';
            tickets.forEach((ticket, index) => {
                const ticketDiv = document.createElement('div');
                ticketDiv.className = 'ticket';
                ticketDiv.innerHTML = `
                    <h3>${ticket.subject}</h3>
                    <p><strong>Name:</strong> ${ticket.name}</p>
                    <p><strong>E-Mail:</strong> ${ticket.email}</p>
                    <p><strong>Beschreibung:</strong> ${ticket.description}</p>
                    <p><strong>Datum:</strong> ${ticket.date}</p>
                `;
                ticketsDiv.appendChild(ticketDiv);
            });
        }
        form.addEventListener('submit', (e) => {
            e.preventDefault();
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const subject = document.getElementById('subject').value;
            const description = document.getElementById('description').value;
            const ticket = {
                name,
                email,
                subject,
                description,
                date: new Date().toLocaleString()
            };
            tickets.push(ticket);
            localStorage.setItem('tickets', JSON.stringify(tickets));
            displayTickets();
            form.reset();
        });
        displayTickets();
    </script>
</body>
</html>
