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
    <script>
        const owner = 'bruelweg-it';
        const repo = 'bruelweg-it.github.io';
        const token = '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';
        const form = document.getElementById('ticketForm');
        async function createTicketFile(ticket) {
            const content = `Name: ${ticket.name}\nEmail: ${ticket.email}\nSubject: ${ticket.subject}\nDescription: ${ticket.description}\nDate: ${ticket.date}`;
            const encodedContent = btoa(unescape(encodeURIComponent(content)));
            const filename = `ticket-${Date.now()}.txt`;
            const path = `tickets/${filename}`;
            try {
                const response = await fetch(`https://api.github.com/repos/${owner}/${repo}/contents/${path}`, {
                    method: 'PUT',
                    headers: {
                        'Authorization': `token ${token}`,
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        message: `Add ticket: ${ticket.subject}`,
                        content: encodedContent
                    })
                });
                if (!response.ok) {
                    const error = await response.json();
                    throw new Error(`Failed to create ticket: ${error.message}`);
                }
                form.reset();
                alert('Ticket erfolgreich eingereicht!');
            } catch (error) {
                console.error('Error creating ticket:', error);
                alert('Fehler beim Speichern des Tickets: ' + error.message);
            }
        }
        form.addEventListener('submit', async (e) => {
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
            await createTicketFile(ticket);
        });
    </script>
</body>
</html>
