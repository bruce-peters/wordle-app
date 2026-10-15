# Wordle Clone

A clone of the word game Wordle, built with React (Create React App) and hosted on Firebase Hosting.

**Play it:** https://wordle-app-i.web.app/

## How it plays

- Guess a random five-letter word in six tries. Type on your keyboard or click the on-screen keys (`Del` deletes, `Enter` submits).
- After each guess the tiles turn green (right letter, right spot), yellow (in the word, wrong spot) or gray (not in the word). The on-screen keyboard keeps track of the letters you have used.
- A guess must be a real word from the allowed-guess list. Otherwise the row shakes and the guess doesn't count.
- A new word is picked every time the page loads. The code also has a daily-word mode (`isDaily` in `src/pages/WordlePage.js`, off by default) that seeds the word with the date.

## Run it

Prerequisites: Node.js and npm. Tested with Node 22.22 / npm 10.9. No `NODE_OPTIONS=--openssl-legacy-provider` flag is needed.

```bash
npm ci
npm start          # dev server at http://localhost:3000
```

Production build:

```bash
npm run build      # outputs to build/
npx http-server build -p 8080   # or any static file server
```

`npm run build` prints some ESLint warnings but finishes. With `CI=true` set (as most CI services do), Create React App treats those warnings as errors and the build fails. Run it with `CI=false npm run build` there.

### Deploy

`firebase.json` serves `build/` as a single-page app, and `.firebaserc` points at the Firebase project `wordle-app-i`. With the [Firebase CLI](https://firebase.google.com/docs/cli) logged in to an account that has access to that project:

```bash
npm run build
firebase deploy --only hosting
```

(Not run here, because it needs the project owner's Firebase login.)

## Project structure

```
src/
  pages/WordlePage.js        game state: keyword, guesses, key handling, win/lose
  components/WordleBoard.js  6x5 grid of WordRow / LetterBox tiles
  components/WordRow.js      green / yellow / gray scoring for one guess
  components/Keyboard.js     on-screen keyboard with letter coloring
  components/Results.js      win / lose panel
  functions/wordList.js      random (optionally seeded) word picker and guess validation
  data/wordList.json         2,315 possible answers
  data/allowedGuesses.json   12,484 accepted guesses
public/                      index.html, icons
firebase.json, .firebaserc   Firebase Hosting config
```
