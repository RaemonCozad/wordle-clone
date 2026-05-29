# 🟩 Wordle Clone

A fully functional Wordle clone built with vanilla HTML, CSS, and JavaScript — no frameworks, no dependencies, no build tools.

🔗 **[Play it live →](https://yourusername.github.io/wordle-clone/wordle.html)**

---

## 📸 Preview

<!-- Add a screenshot or GIF here -->
<!-- Tip: Record a short GIF using ScreenToGif (Windows) or Kap (Mac) and drag it into this section on GitHub -->

![Wordle Clone Screenshot](screenshot.png)

---

## ✨ Features

- 🟩 **Color-coded feedback** — correct, present, and absent letter states
- ⌨️ **On-screen + physical keyboard** support
- 🔄 **Animated tile reveals** — smooth flip animations on each guess
- 💬 **Toast notifications** — "Not in word list", "Not enough letters", win messages
- 📊 **Stats tracking** — games played, win %, current streak, max streak
- 📈 **Guess distribution chart** — see which row you win on most
- 📤 **Share button** — copies an emoji grid to your clipboard
- 🌙 **Dark / light mode** toggle with localStorage persistence
- 📱 **Fully responsive** — works on mobile and desktop
- ♾️ **Unlimited play** — new random word every game

---

## 🛠️ Tech Stack

| Technology | Usage |
|---|---|
| HTML5 | Structure and semantic markup |
| CSS3 | Animations, CSS variables, responsive layout |
| Vanilla JavaScript | Game logic, DOM manipulation, localStorage |

**No frameworks. No libraries. No build step.** Just open `wordle.html` in a browser.

---

## 🚀 Run Locally

```bash
# Clone the repo
git clone https://github.com/yourusername/wordle-clone.git

# Open in your browser — that's it!
open wordle.html
```

No `npm install`. No server. No config. Just a file.

---

## 🧠 How I Built It

### The evaluation algorithm

The trickiest part was correctly handling duplicate letters. For example, if the answer is `CRANE` and you guess `ABBEY`, the first `B` should be marked absent — not present — because `B` doesn't appear in the answer at all.

I solved this with a two-pass approach:

1. **First pass** — mark all exact matches (correct) and null them out in the target
2. **Second pass** — check remaining letters for presence (present), consuming each target letter only once

```js
function evaluate(guess) {
  const result = Array(5).fill('absent');
  const target = targetWord.split('');
  const g = guess.split('');

  // Pass 1: exact matches
  g.forEach((l, i) => {
    if (l === target[i]) { result[i] = 'correct'; target[i] = null; g[i] = null; }
  });

  // Pass 2: wrong position
  g.forEach((l, i) => {
    if (!l) return;
    const idx = target.indexOf(l);
    if (idx !== -1) { result[i] = 'present'; target[idx] = null; }
  });

  return result;
}
```

### Animations without a library

All animations — tile flip, bounce on win, shake on invalid word, pop on keypress — are pure CSS `@keyframes`. I timed the flip reveal with `setTimeout` delays per tile (300ms each) so they cascade left to right.

### Keyboard state priority

Keys on the on-screen keyboard update to reflect the best result seen for that letter. A key already marked `correct` (green) won't be downgraded to `present` (yellow) by a later guess. I tracked this with a priority map: `correct: 3, present: 2, absent: 1`.

---

## 📁 Project Structure

```
wordle-clone/
├── wordle.html     # Entire game — HTML + CSS + JS in one file
└── README.md
```

---

## 🔮 Future Improvements

- [ ] Daily word mode (same word for everyone each day)
- [ ] Hard mode (must use revealed hints in subsequent guesses)
- [ ] Larger word dictionary
- [ ] Accessibility improvements (screen reader support, high contrast mode)

---

## 📄 License

MIT — free to use, modify, and distribute.
