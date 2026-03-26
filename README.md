# Mate-in-One Finder with Z3

A Jupyter notebook that finds **mate-in-one** moves in chess positions using **Z3**, Microsoft's formal logic constraint solver. Given a board position in FEN notation, the solver symbolically reasons over all candidate moves to determine which one — if any — delivers checkmate in a single move.

---

## How It Works

1. The chessboard is parsed from a FEN string into an 8×8 grid
2. All pseudo-legal moves for the active side are generated
3. The board is encoded as a dictionary of **Z3 integer constants** `(row, col) → IntVal`
4. Z3 introduces a symbolic variable `move_idx` representing the index of the mating move
5. Constraints are added to assert that after the chosen move:
   - The opponent king is **in check**
   - The opponent has **no legal reply** (checkmate)
6. If Z3 finds a satisfying assignment → the mating move is returned and displayed

---

## Project Structure

```
mate-in-one-finder/
├── mate_in_one_finder.ipynb   # Main notebook
├── fen_db.csv                 # Database of chess positions in FEN format (~5000 positions)
└── README.md
```

### `fen_db.csv` columns

| Column | Description |
|--------|-------------|
| `fen`  | Board position in [FEN notation](https://en.wikipedia.org/wiki/Forsyth%E2%80%93Edwards_Notation) |
| `best` | The known best (mating) move in UCI format (e.g. `c2c4`) |

---

## Getting Started

### Prerequisites

- Python 3.8+
- Jupyter Notebook or JupyterLab

### Installation

```bash
git clone https://github.com/<your-username>/mate-in-one-finder.git
cd mate-in-one-finder
pip install z3-solver chess pandas
```

### Run

```bash
jupyter notebook mate_in_one_finder.ipynb
```

The notebook will **randomly pick a FEN** from `fen_db.csv` and attempt to find the mate-in-one move. You can also manually specify a FEN string directly in the notebook (see the last cell).

---

## Notebook Sections

| Section | Description |
|---------|-------------|
| 1. Imports | Library setup |
| 2. Board Utilities | FEN parser and coordinate helpers |
| 3. Pseudo-Legal Move Generation | Move generator for all piece types |
| 4. Z3 Board Encoding | Maps pieces to Z3 integer constants |
| 5. Check Detection via Z3 | Builds attack constraints for the king |
| 6. Mate-in-One Search | Core Z3 solver logic |
| 7. Board Display | SVG board rendering via `python-chess` |
| 8. Run | Entry point — random or manual FEN |

---

## Dependencies

| Package | Purpose |
|---------|---------|
| [`z3-solver`](https://github.com/Z3Prover/z3) | Constraint solving / formal logic |
| [`chess`](https://python-chess.readthedocs.io/) | Board representation and SVG rendering |
| [`pandas`](https://pandas.pydata.org/) | Loading FEN database from CSV |

---

## Example

```
Position: 7Q/3Bk3/2P1p3/4P2P/7b/5K2/B7/1b6 w - - 3 78
Mate-in-one found: h8e8 
```

---

## License

MIT License — feel free to use, modify, and distribute.
