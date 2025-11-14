# Circle - Frontend (About App)
Circle is a simple app that helps you stay connected with the important people in your life. It reminds you when you haven’t reached out to friends in a while and makes it easy to check in, send a message, or set reminders — so friendships don’t fade over time.

This folder contains a simple React + Vite frontend demo for Circle, a friendship maintenance app.

**✨ What the Product Does (Plain, Descriptive Terms)**

Circle keeps track of how long it’s been since you last spoke to a friend. It then helps you stay in touch by:

- Showing your friends in a simple card layout
- Highlighting who you’ve not contacted recently
- Giving quick actions like “Message” or “Remind Me Later”
- Helping you build healthier, more consistent communication habits

_In short: Circle makes it easier to maintain friendships in a busy world._

**💛 Why It Matters (More Depth)**
Life gets busy, and even important relationships can fade when we forget to check in. Circle solves this problem by making friendship maintenance intentional and easy.

_It helps users:_
- Strengthen real-life connections
- Avoid losing touch with important people
- Build healthier social habits
- Feel more emotionally supported

**_Circle isn’t another social network — it’s a tool for relationship wellness.
**_

Many people lose touch with friends—not because they don’t care, but because life gets busy. Circle solves this by giving gentle reminders and an easy way to reach out.

It matters because it:
- Reduces unintentional friend drift
- Strengthens relationships through consistent check-ins
- Supports emotional well-being by keeping people socially connected
- Helps users maintain meaningful bonds without stress or forgetfulness

People don’t need another complex social app — they need a simple tool that helps them stay close to the people who matter.

**🎯 Target Users / Persona**

_Primary Persona: The Busy but Caring Friend_
Age: 18–45
Feels overwhelmed with work, school, or life
Values meaningful friendships but struggles to keep track of everyone
Wants simple reminders and low-effort ways to stay connected
Prefers quick actions over long planning

_Secondary Persona:_
People managing long-distance friendships
Individuals wanting to improve their social habits
Anyone trying to reconnect with their network

**👥 Who It’s For (Clear User Personas)**
1. The Busy Professional
People juggling work, life, and responsibilities who struggle to check in with friends regularly.

2. The Socially Mindful Person
Users who value meaningful relationships and want a gentle system to help maintain them.

3. The Forgetful Communicator
People who genuinely care but often forget to call, text, or catch up with friends.

4. Community Builders
Mentors, group organizers, or support-circle leaders who want to stay connected with members.

**🧩 Key Features (Structured List)**
1. Friend Cards

- Displays each friend with name, photo, and last time contacted
- Shows urgency indicators (e.g., “It’s been 20 days”)

2. Last Contact Tracking

- Automatically tracks days since last interaction
- Highlights people you’re overdue to reach out to

3. Quick Action Buttons

> Message – Opens your default messaging option
> Remind Me – Schedules a reminder to reconnect later

4. Clean Minimal UI

- Lightweight, simple interface
- Current version uses mock data (no backend yet)

5. Fast React + Vite Frontend

- Works quickly and feels responsive
- Shows a working layout for how the full product will look

**🔑 Key Features (Well-Structured List)**
1. Friend Cards
A clean card layout showing each friend, their photo, and key connection details.

2. Last Contact Tracker
See how long it has been since your last call/text/meet-up with any friend.

3. “Remind Me” Alerts
Set reminders so you never forget to check in.

4. Quick Action Buttons
Message, call, or follow up with one tap from each friend’s card.

5. Mock Data UI Preview
The current frontend uses temporary data to demonstrate the full experience visually.

6. Minimal, Fast UI
Built with React + Vite for a lightweight, smooth interface.

**🖼️ Figma Mockup**

<img width="346" height="669" alt="image" src="https://github.com/user-attachments/assets/33a35cb2-4637-496b-8d82-bef019018052" />.


🗂 What’s Inside This Frontend Folder

This folder contains the frontend demo built with React + Vite.

Quick start:

1. Install dependencies: `npm install`
2. Start dev server: `npm run dev`

The UI is intentionally minimal — it demonstrates friend cards, last-contact tracking, and actions like "Remind me" and "Message". The backend is not yet implemented; the UI uses mock data.


**Circle App Navigation Map**
This diagram shows how a user would flow through the app to access its core features.
flowchart TD
    A[App Launch] --> B[Home Screen / Dashboard]

    B --> C[Bottom Navigation Bar]

    C --> B
    C --> D[Circles Screen]
    C --> E[Connect Screen]
    C --> F[Activities Screen]
    C --> G[Vacations Screen]

    subgraph HomeScreen [Home Screen Actions]
        B --> |Tap Circle Card| H[Circle Details]
        B --> |Tap Quick Action| I[Log Activity]
        B --> |Tap Reminder| E
    end

    subgraph CirclesSection [Circles Section]
        D --> |Tap 'Create New'| J[Create New Circle]
        D --> |Tap a Circle| H
    end

    subgraph ConnectSection [Connect Section]
        E --> |Tap a Friend| K[Contact Friend]
        E --> |Tap 'Set Reminder'| L[Set Custom Reminder]
    end

    subgraph ActivitiesSection [Activities Section]
        F --> |Tap 'Plan New'| M[Plan Activity Flow]
        F --> |Tap Activity| N[Activity Details]
    end

    subgraph VacationsSection [Vacations Section]
        G --> |Tap 'Create New'| O[Vacation Planner Flow]
        G --> |Tap Vacation| P[Vacation Details]
    end

    subgraph CircleDetails [Circle Details Screen]
        H --> Q[Message Circle]
        H --> M
        H --> O
        H --> R[View Members & Check-ins]
    end

    %% Styling for clarity
    classDef primary fill:#4a86e8, color:white
    classDef secondary fill:#6aa84f, color:white
    classDef tertiary fill:#e69138, color:white
    
    class B,A primary
    class D,E,F,G secondary
    class H,M,O tertiary

Visual Guide & Key Screens
Here’s a wireframe-style layout of the main navigation screens a user would see.

1. Main App Layout with Bottom Navigation
+------------------------------------+
|                                    |
|  🏠 HOME / DASHBOARD              |
|                                    |
|  +------------------------------+  |
|  |  👋 Upcoming Reminders       |  |
|  |  - Call Sarah - Tomorrow     |  |
|  |  - Hike with Crew - Sat      |  |
|  +------------------------------+  |
|                                    |
|  📝 Recent Activity               |
|  • You checked in with Alex       |
|  • Mark added a beach trip idea   |
|                                    |
|  🔄 Your Circles                  |
|  +----------+  +----------+       |
|  | Family   |  | Adventure|       |
|  | 👨👩👧   |  | Crew     |       |
|  | Last: 2d |  | Last: 1w |       |
|  +----------+  +----------+       |
|                                    |
|  [ + Check-in ] [ 💬 Message ]    |
|                                    |
+------------------------------------+
| 🏠 | 👥 | 🔄 | 🎯 | ✈️  | 👤     |  <-- Bottom Nav
+------------------------------------+
    |    |    |    |    |    |
    |    |    |    |    |    Profile
    |    |    |    |    Vacations
    |    |    |    Activities
    |    |    Connect
    |    Circles
    Home

2. Key Screen Examples
Circles Screen:
+------------------------------------+
| 👥 Circles              [+ Button] |
|                                    |
|  🔍 Search Circles...             |
|                                    |
|  • Close Friends (5)              |
|    👤👤👤 Last: 2 days ago        |
|                                    |
|  • Family (4)                     |
|    👤👤👤👤 Last: 1 day ago       |
|                                    |
|  • College Buddies (8)            |
|    👤👤👤👤... Last: 3 weeks ago  |  <-- "Nudge" indicator
|                                    |
+------------------------------------+

Connect Screen (The "Nudge" Center):
+------------------------------------+
| 🔄 Connect                         |
|                                    |
| ⏰ Time to Reach Out:              |
|                                    |
|  👤 David - College Buddies       |
|    "Haven't spoken in 3 weeks"    |
|    [💬 Message] [📅 Remind]       |
|                                    |
|  👤 Maria - Close Friends         |
|    "Haven't connected in 2 weeks" |
|    [✅ Check-in] [💬 Message]     |
|                                    |
|  Your Scheduled Reminders:         |
|  ✓ Call Sarah - Tomorrow 10 AM    |
|  ✓ Plan weekend hike - This Fri   |
|                                    |
+------------------------------------+


Activities Planning Flow:
+------------------------------------+
| 🎯 Activities         [Plan New 🔽]|
|                                    |
|  Upcoming:                         |
|  🏞️ Hike at Redwood Park - Sat    |
|  with Adventure Crew               |
|                                    |
|  💡 Suggested Activities:          |
|  🚶‍♂️ Nature Walk    ☕ Coffee     |
|  🎳 Bowling        🎮 Game Night  |
|  🏀 Basketball     🍽️ Dinner     |
|                                    |
|  Past Activities:                  |
|  May 12: Coffee with Sarah ✅     |
|  May 5: Game Night with Crew ✅   |
|                                    |
+------------------------------------+

_**This structure allows users to:**_

- Start quick from the Home screen.
- Navigate deeply into any feature via the bottom bar.
- Flow naturally between related features (e.g., from a Circle to planning a Vacation).
- Receive proactive nudges via the Connect screen that drive engagement with the app's core purpose.

Files:
- `src/` — React source files
- `index.html` — Vite entry
- `package.json` — scripts and deps
