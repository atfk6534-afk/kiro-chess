# ♟ KiroChess — Free Chess Analysis

A free, open-source chess game analysis tool that works like Chess.com's game review — fully powered by **Stockfish** engine running in your browser.

## ✨ Features

- 📊 **Evaluation Bar** — Live +/- score with smooth animation
- 🤖 **Stockfish Engine** — Depth 18, MultiPV 3 lines per position
- 🚨 **Move Classification** — Brilliant · Great · Best · Excellent · Good · Inaccuracy · Mistake · Blunder
- 📈 **Accuracy Score** — Chess.com-style % accuracy for both players
- 🔍 **Step-by-step Review** — Navigate with arrows or keyboard
- 💬 **Human Explanations** — Why each mistake or blunder is bad
- 🎯 **Best Move Arrows** — Green arrow = engine best, Red = your blunder
- 🔄 **Board Flip** — View from either side
- ⌨️ **Keyboard Navigation** — Arrow keys to navigate moves

## 🚀 Getting Started

### Prerequisites
- Node.js 18+
- npm 9+

### Install & Run

```bash
git clone https://github.com/YOUR_USERNAME/kirochess.git
cd kirochess
npm install
npm run dev
```

Open [http://localhost:5173](http://localhost:5173)

### Build for Production

```bash
npm run build
npm run preview
```

The `dist/` folder can be deployed to **Vercel**, **Netlify**, **GitHub Pages**, or any static host.

## 🌐 Deploy to Vercel (Free)

1. Push to GitHub
2. Go to [vercel.com](https://vercel.com)
3. Import your repo
4. Click Deploy — done!

> ⚠️ **Important**: The app uses SharedArrayBuffer for Stockfish WASM. Vercel automatically sets the required COOP/COEP headers. If deploying elsewhere, add these headers:
> ```
> Cross-Origin-Opener-Policy: same-origin
> Cross-Origin-Embedder-Policy: require-corp
> ```

## 📁 Project Structure

```
kirochess/
├── public/
│   └── stockfish-worker.js     # Stockfish Web Worker
├── src/
│   ├── components/
│   │   ├── ChessBoard.jsx       # Board renderer with SVG arrows
│   │   ├── EvalBar.jsx          # Animated evaluation bar
│   │   ├── EvalGraph.jsx        # Clickable evaluation graph
│   │   ├── MoveList.jsx         # Move list with classification dots
│   │   ├── MoveAnnotationPanel.jsx  # Analysis detail panel
│   │   ├── AccuracySummary.jsx  # Accuracy rings + move breakdown
│   │   └── PGNInput.jsx         # PGN paste modal
│   ├── hooks/
│   │   └── useStockfish.js      # Stockfish engine hook
│   ├── lib/
│   │   └── chess.js             # Chess utilities, classification
│   ├── pages/
│   │   ├── HomePage.jsx         # Landing page
│   │   └── AnalysisPage.jsx     # Main analysis page
│   ├── styles/
│   │   └── global.css           # CSS variables + global styles
│   ├── App.jsx
│   └── main.jsx
├── index.html
├── vite.config.js
└── package.json
```

## 🔧 How Analysis Works

1. PGN is parsed using **chess.js** into a list of FEN positions
2. Each FEN is sent to **Stockfish** running in a Web Worker
3. Stockfish returns `info` lines with `score cp` and `pv` (principal variation)
4. Scores are converted to white's perspective and compared move-to-move
5. Centipawn loss determines move classification (blunder ≥ 150cp loss, etc.)
6. Accuracy is calculated using `100 * e^(-0.004 * cpLoss)` (Chess.com formula)

## 🧠 Move Classification Thresholds

| Classification | CP Loss |
|---|---|
| Best | 0 (engine's top choice) |
| Excellent | ≤ 10 cp |
| Good | ≤ 25 cp |
| Inaccuracy | ≤ 50 cp |
| Mistake | ≤ 100 cp |
| Blunder | > 100 cp |

## 📦 Tech Stack

- **React 18** — UI framework
- **Vite** — Build tool
- **chess.js** — Chess logic, PGN parsing, FEN generation
- **Stockfish 16** — Chess engine (loaded via CDN in Web Worker)
- **React Router** — Page routing
- No CSS frameworks — pure CSS variables

## 📄 License

MIT — free to use, fork, and modify.
