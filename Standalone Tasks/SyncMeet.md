## Task ID: SyncMeet

#### `Full Stack Web Development`, `WebRTC`, `Real-Time Communication`, `Authentication`

Mentors: [Aman Nagpal](https://github.com/StackedUpAman) ([+91 7082073890](https://wa.me/917082073890))

Difficulty: `Medium`

### Description

Build **SyncMeet**, a real-time **collaboration workspace** where authenticated users can create and join rooms to communicate and collaborate. The platform should combine **live video, audio, screen sharing, participant management, and real-time chat** into a single interactive workspace.

The challenge focuses on building the underlying real-time workflow using **WebRTC and WebSockets**, while managing rooms, participants, and live interactions reliably.

**Steps to Complete the Challenge:**

1. **Authentication & User Management**
   - Implement user registration and login.
   - Protect room-related routes and display the authenticated user's identity inside the room.
   - You may use JWT, session-based authentication, or any equivalent approach.

2. **Room Creation & Joining**
   - Allow authenticated users to create and join rooms using a unique room ID or shareable link.
   - Assign the room creator as the **Host**.
   - Provide a simple lobby/waiting screen before entering the room.

3. **WebRTC Audio/Video & Media Controls**
   - Implement real-time audio and video communication using **WebRTC**.
   - Use **WebSockets or Sockets.io** for signaling.
   - Provide controls for microphone, camera, and leaving the room.
   - Display connected participants in a responsive video layout.

4. **Participant Management & Screen Sharing**
   - Display participants and their current media states in real-time.
   - Allow the Host to remove participants and manage the room.
   - Implement browser-based **screen sharing** and display the shared screen to other participants.

5. **Real-Time Meeting Chat**
   - Add room-based real-time text chat using **WebSockets or Socket.iO**.
   - Display the sender and timestamp for each message.
   - Ensure messages remain isolated to their respective rooms.

6. **Connection Handling**
   - Handle participants joining, leaving, refreshing, or disconnecting from the room.
   - Keep room and participant state synchronized across connected clients.
   - Clean up disconnected users and empty rooms appropriately.

**Bonus Feature (Optional)**

*Implementing any* ***two*** *of the following features will make the task count as* `Hard`

### AI Meeting Assistant

Add an AI-powered meeting assistant that can generate a **meeting transcript and summary**.

- Convert meeting audio into text using a speech-to-text service.
- Use an LLM to generate a summary, key points, decisions, and action items.

### Collaborative Whiteboard

Add a **real-time collaborative whiteboard** where participants can draw together.

- Synchronize drawing operations between participants using WebSockets.
- Bonus points for shapes, text, undo/redo, or participant cursors.

### Live Polls

Add **real-time polls** that can be created by the Host.

- Participants can vote and see live results.
- The Host should be able to close the poll and display the final result.

### Meeting Recording

Allow users to **record the meeting locally**.

- Provide controls to start and stop recording.
- Allow users to preview and save the recorded session.

### Waiting Room

Add a **Host-controlled waiting room**.

- Participants request access before entering the room.
- The Host can accept or reject incoming participants.

**Useful Resources:**

- [WebRTC Documentation](https://webrtc.org/getting-started/overview)
- [MDN WebSocket API](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)
- [Socket.IO Documentation](https://socket.io/docs/v4/)
- [React Documentation](https://react.dev/)
- [Express.js Documentation](https://expressjs.com/)