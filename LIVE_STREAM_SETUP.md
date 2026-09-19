# CrickMate Live Stream V2

This build completes the live camera/watch flow using WebRTC + Firebase Firestore signaling.

## Flow
1. Admin opens `admin-live.html` over HTTPS.
2. Admin logs in, selects a match, taps **START CAMERA**, then **START LIVE**.
3. A live session is created at `tournaments/{tournamentId}/matches/{matchId}/stream/live`.
4. Each viewer gets its own `viewers/{viewerId}` signaling document, so multiple viewers can connect without overwriting one shared answer.
5. The existing live score document is still listened to and shown in the video overlay.

## Important
- Camera access requires HTTPS or localhost.
- Firestore rules must allow the broadcaster to read/write the stream and viewer signaling documents, and allow viewers to read/write their own signaling session. Keep production rules restrictive.
- STUN is included. For reliable connections between arbitrary mobile networks, add a TURN server in both `js/live-camera.js` and `js/watch-live.js` inside `rtcConfig`.
- This is peer-to-peer WebRTC: the broadcaster uploads one copy per viewer. For large audiences, use an SFU/media server rather than direct P2P.

## Test
Open `admin-live.html` on one device and `watch-live.html?tournamentId=...&matchId=...` on another. Use the same Firebase project and the same match.
