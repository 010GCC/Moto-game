# Moto Rush

A browser-based motorcycle distance game with a global leaderboard powered by Firebase Firestore.

Play it live: https://010gcc.github.io/Moto-game/

---

## How to Play

| Key | Action |
|-----|--------|
| `G` | Hold to accelerate |
| `SPACE` | Jump |
| `M` | Mute/unmute music |

- Land at a smooth angle to keep your speed
- Bad landings lose speed; very bad landings crash
- See how far you can go!

---

## Project Files

- `index.html` — entire game (HTML + CSS + JS in one file)
- `README.md` — this file

---

## Firebase Setup (Firestore)

The global leaderboard uses **Firebase Firestore**. The config is already embedded in `index.html`.

### Firestore Security Rules

In the Firebase console → Firestore Database → Rules, set:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /scores/{document} {
      allow read: if true;
      allow create: if request.resource.data.keys().hasAll(['name', 'distance', 'date'])
                    && request.resource.data.name is string
                    && request.resource.data.name.size() <= 12
                    && request.resource.data.distance is number
                    && request.resource.data.date is number;
    }
  }
}
```

This allows anyone to read scores and submit new ones, but prevents malformed data.

---

## GitHub Pages Deployment

The game is served via GitHub Pages from the `main` branch root.

To update and deploy:
```bash
cd /root/moto-game
# make edits to index.html
git add index.html
git commit -m "your message"
git push origin main
```

GitHub Pages auto-deploys within ~1 minute of pushing.

---

## Session Notes (for continuity)

- **Repo:** `git@github.com:010GCC/Moto-game.git`
- **Branch:** `main`
- **SSH key:** `/root/.ssh/id_ed25519` (authenticated as `010GCC`)
- **Firebase project:** `moto-rush-f47f2`
- **Firestore collection:** `scores` (fields: `name`, `distance`, `date`)
- **Leaderboard:** Global top 10, real-time updates via `onSnapshot` listener
- **Player name:** Stored in `localStorage` key `motoRushName`

### Commit history
1. Initial commit — base game
2. Add local leaderboard with top 10 scores
3. (current) Replace localStorage leaderboard with Firebase Firestore global scores
