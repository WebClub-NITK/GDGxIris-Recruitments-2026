## Task ID: SyncPlay

#### `Flutter`, `Firebase`

Mentors: [Patel Pal Bharat](https://github.com/palpatel224) ([+91 9265254960](https://wa.me/919265254960))

Difficulty: `Easy-Medium`

## Overview

Build a **music listening application** using **Flutter**.

The goal of this task is to evaluate your understanding of **basic Flutter application development**, including UI development, navigation, state management, handling lists, user interactions, asynchronous operations, and integration with external services.

The application should allow users to browse a list of songs, play music, manage favourites, and create/join a listening room where the music playback is **synchronized between users**.

Firebase can be used to handle the basic room and synchronization functionality.

---

## Required Features

### 1. Home Screen

Create a home screen displaying a list of available songs.

Each song should display:

- Song title
- Artist name
- Album/cover image

Users should be able to:

- Browse the songs
- Search for a song
- Tap a song to open the music player

You may use a **predefined/static list of songs**.

---

### 2. Music Player

Implement a basic music player.

The player should support:

- Play
- Pause
- Seek
- Display current playback position
- Display total duration
- Progress bar

The player should display:

- Song title
- Artist
- Album artwork

The currently playing song should remain available when navigating between screens.

You may use local audio files or publicly available audio sources.

You may use packages such as `just_audio` or any other suitable Flutter package.

> You do not need to implement a music streaming service.

---

### 3. Favourites

Users should be able to mark songs as favourites.

Required functionality:

- Add a song to favourites
- Remove a song from favourites
- View all favourite songs

The favourites can be maintained **in application memory**. Persistent storage is not required.

---

### 4. Listening Room

Implement a simple listening-room feature.

Users should be able to:

#### Create a Room

- Enter a room name
- Create a room
- Generate/display a unique room code

#### Join a Room

- Enter a room code
- Join an existing room
- Display the room name
- View the current song being played in the room

The room information and playback state can be stored using **Firebase Firestore or Firebase Realtime Database**.

---

### 5. Music Synchronization

The main feature of the listening room is **synchronized music playback**.

Users inside the same room should share the same playback state.

The following actions should be synchronized:

- Play
- Pause
- Change song
- Seek

The objective is to demonstrate that you understand how to:

- Store shared application state
- Listen for real-time updates
- Update the local UI based on remote state
- Control the audio player based on those updates

You may use **Firebase Firestore listeners, Firebase Realtime Database, or another reasonable real-time solution**.

---

### Task Difficulty

The task has two difficulty levels depending on the implementation:

- **Easy:** Complete the application without implementing real-time music synchronization. The listening room can be simulated locally, with the room state maintained within the application.
- **Medium:** Implement **real-time music synchronization** between users in the same listening room using Firebase or another suitable real-time solution.

The synchronization functionality should allow changes such as **play, pause, seek, and song changes** to be reflected across users in the same room.

### Useful Resources

- [Flutter Documentation](https://docs.flutter.dev/)
- [Dart Documentation](https://dart.dev/)
- [Firebase Flutter Documentation](https://firebase.google.com/docs/flutter/setup)
- [Firebase Firestore](https://firebase.google.com/docs/firestore)
- [Firebase Realtime Database](https://firebase.google.com/docs/database)
- [just_audio](https://pub.dev/packages/just_audio)

---

## Important Notes

- You may use static/mock song data.
- You may use local audio files or publicly available audio sources.
- Firebase can be used for room creation and real-time synchronization.
- You do **not** need to build a custom backend.
- Perfect/millisecond-level audio synchronization is **not required**.
- You are free to design the UI as you like.
