
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Campus Event Portal</title>
  <style>
    * { box-sizing: border-box; font-family: system-ui, sans-serif; margin: 0; padding: 0; }
    body { background: #f4f6f9; color: #333; padding: 20px; }
    header { background: #1e3a8a; color: white; padding: 20px; border-radius: 8px; margin-bottom: 20px; text-align: center; }
    .container { max-width: 900px; margin: auto; }
    
    /* Add Event Form */
    .form-card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); margin-bottom: 25px; }
    .form-card h2 { margin-bottom: 15px; font-size: 1.2rem; }
    .form-group { display: flex; gap: 10px; flex-wrap: wrap; }
    input, select, button { padding: 10px; border: 1px solid #ccc; border-radius: 5px; font-size: 1rem; }
    input, select { flex: 1; min-width: 180px; }
    button { background: #2563eb; color: white; border: none; cursor: pointer; font-weight: bold; }
    button:hover { background: #1d4ed8; }

    /* Event Grid */
    .event-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(260px, 1fr)); gap: 20px; }
    .event-card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); display: flex; flex-direction: column; justify-content: space-between; }
    .badge { display: inline-block; background: #dbeafe; color: #1e40af; padding: 4px 8px; border-radius: 4px; font-size: 0.8rem; font-weight: bold; margin-bottom: 10px; width: fit-content; }
    .event-card h3 { margin-bottom: 8px; }
    .event-card p { font-size: 0.9rem; color: #555; margin-bottom: 6px; }
    .register-btn { margin-top: 15px; width: 100%; background: #10b981; }
    .register-btn:hover { background: #059669; }
    .registered { background: #6b7280 !important; cursor: default; }
  </style>
</head>
<body>

<div class="container">
  <header>
    <h1>🎓 Campus Event Portal</h1>
    <p>Discover, host, and register for upcoming college events</p>
  </header>

  <!-- Organizer Section: Create Event -->
  <div class="form-card">
    <h2>Post a New Campus Event</h2>
    <form id="eventForm" class="form-group">
      <input type="text" id="title" placeholder="Event Title" required>
      <input type="date" id="date" required>
      <input type="text" id="venue" placeholder="Venue (e.g., Main Auditorium)" required>
      <select id="category">
        <option value="Tech">Tech</option>
        <option value="Cultural">Cultural</option>
        <option value="Sports">Sports</option>
        <option value="Workshop">Workshop</option>
      </select>
      <button type="submit">Add Event</button>
    </form>
  </div>

  <!-- Student Section: Event Listings -->
  <div class="event-grid" id="eventList"></div>
</div>

<script>
  // Sample initial data
  let events = [
    { id: 1, title: "Annual Hackathon", date: "2026-12-05", venue: "CS Block Lab 1", category: "Tech", registered: false },
    { id: 2, title: "Inter-College Music Fest", date: "2026-12-12", venue: "Open Air Theater", category: "Cultural", registered: false },
    { id: 3, title: "AI & Web Dev Workshop", date: "2026-12-18", venue: "Seminar Hall B", category: "Workshop", registered: false }
  ];

  const eventList = document.getElementById("eventList");
  const eventForm = document.getElementById("eventForm");

  function renderEvents() {
    eventList.innerHTML = "";
    events.forEach(event => {
      const card = document.createElement("div");
      card.className = "event-card";
      card.innerHTML = `
        <div>
          <span class="badge">${event.category}</span>
          <h3>${event.title}</h3>
          <p><strong>Date:</strong> ${event.date}</p>
          <p><strong>Venue:</strong> ${event.venue}</p>
        </div>
        <button class="register-btn ${event.registered ? 'registered' : ''}" onclick="toggleRegister(${event.id})">
          ${event.registered ? '✓ Registered' : 'Register Now'}
        </button>
      `;
      eventList.appendChild(card);
    });
  }

  function toggleRegister(id) {
    events = events.map(ev => ev.id === id ? { ...ev, registered: !ev.registered } : ev);
    renderEvents();
  }

  eventForm.addEventListener("submit", (e) => {
    e.preventDefault();
    const newEvent = {
      id: Date.now(),
      title: document.getElementById("title").value,
      date: document.getElementById("date").value,
      venue: document.getElementById("venue").value,
      category: document.getElementById("category").value,
      registered: false
    };
    events.unshift(newEvent);
    eventForm.reset();
    renderEvents();
  });

  renderEvents();
</script>

</body>
</html>
