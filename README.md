# ♟️ Chess Evolution Engine: A Comparative Performance Analyzer

Decode your strategic journey. Move beyond raw ratings and win-loss ratios with an algorithmic mirror that tracks your real-time skill progression, transforming match files into a living narrative of your strategic growth.

### Why I Built This: Problems Solved

* **Surface-Level Metrics**: Traditional platforms only show a fluctuating rating number or a flat win/loss record, failing to explain *why* games are won or lost.
* **Blind Spots in Play**: Players struggle to identify whether their structural failures stem from weak openings, careless middlegame blunders, or poor endgame conversions.
* **Tracking True Progression**: Subjective feelings of improvement lack hard data; this engine establishes a mathematical baseline to prove longitudinal skill growth.
* **Quantifying Consistency**: Isolating individual fluctuations helps players see if their performance changes drastically depending on the match phase.
* **Actionable Feedback**: Transforming abstract engine evaluations into clear structural tiers gives players precise metrics to target during practice.

### Core Capabilities & Engine Functions

* **Automated PGN Ingestion**: Seamlessly crawls project directories to parse raw match texts into structured Python objects.
* **Identity & Color Tracking**: Automatically detects the user's username handle to map out specific match outcomes, colors, and results.
* **Stockfish Engine Integration**: Interfaces with the UCI Stockfish engine to run deep, multi-depth position evaluations before and after moves.
* **Centipawn Noise Filtering**: Clamps values and neutralizes positive horizon noise to isolate actual human decision quality.
* **Phase Segmentation**: Automatically divides every game into three distinct structural chapters: Opening, Middlegame, and Endgame.
* **Longitudinal Baselines**: Aggregates historical gameplay metrics to compute statistical norms (medians and quartiles) for comparison.
* **Performance Classification**: Sorts every single action into precise tiers—from Brilliant maneuvers down to devastating Blunders.
* **Visual Trend Tracking**: Employs data visualization libraries to map consistency shifts, improvement trends, and blunder reductions.

### Datasets & File Architecture Guide

To evaluate your progress accurately, this engine relies on a comparative data split. **Training data** comprises your historical archive of older games, which is processed through functions like `create_training_baseline` to establish a mathematical baseline of what your "normal" performance looks like. **Testing data** represents your latest matches, which are evaluated and compared directly against that baseline using utility functions like `compare_data` to detect real-time shifts, improvements, or regressions.

* **Training PGN Files (`chess-train-50games-1`)**:
* Contains the raw historical archive of your older games.


* Used at the beginning of the pipeline to build your baseline performance metrics and historical behavior model.




* **Testing PGN Files (`chess-test-1`)**:
* Records your latest, most recent games that you want to evaluate.


* Passed through the engine to compare against your established training baseline and check for progress or regression.




* **Training CSV Analysis (`chess_analysis_50games-csv`)**:
* Pre-computed or intermediate tabular records used after game storage data extraction.


* Initialized alongside the `classifier` function to feed pre-analyzed historical match rows directly into the comparative analytics modules.




* **Testing CSV Data (`testing-data-1`)**:
* Supplementary tabular logs for latest match evaluations.


* Handled by the classifier logic alongside training exports to evaluate recent gameplay patterns side-by-side.





### Setup & Local Adaptation Guide

* **Template Datasets**: Placeholder PGN and CSV files are included in the repository structure for seamless testing.
* **Username Configuration**: Update the target username variable from `T_ranger2008` to match your personal Chess.com or Lichess account.


* **File Path Adjustments**: Adapt the absolute Kaggle directory paths (`/kaggle/input/...`) to relative local paths if executing the code outside the cloud environment.


* **Stockfish Binary Precautions**: Cloud setups use Linux package managers (`apt-get`), whereas local Windows or macOS runs require a manual binary download with an explicitly declared `stockfish_path`.


* **Dataset Public Visibility**: If you replicate this workspace on Kaggle, ensure all custom-uploaded PGN and CSV files are toggled to **Public** so user environment mounts function correctly.
* **Local Python Dependencies**: Verify that required libraries (`pandas`, `matplotlib`, `python-chess`) are fully installed in your local terminal before launching the notebook.



---

### Deep Dive: Function-by-Function Breakdown

* `input_games(folder_path)`: Scans a target directory recursively for `.pgn` files, reads them line-by-line via the `chess.pgn` module, and compiles them into a unified list of match objects.


* `get_game_info(games, your_username)`: Extracts game headers (White, Black, Result), performs a case-insensitive check against your unique username handle, maps your specific color, and computes your individual game result (Win/Loss/Draw).


* `kst analyze_game(game, engine, your_name, depth=17)`: Simulates move-by-move evaluation across an entire match using a fixed analysis depth, printing position scores before and after every turn.


* `analyze_my_moves(game, engine, your_name, depth=17)`: Isolates analysis exclusively to your turns, letting opponent moves pass while calculating evaluation metrics specifically for your decisions.


* `analyze_my_moves_data(game, engine, your_name, depth=17)`: Runs the core evaluation loop for your moves, clamps scores to eliminate horizon noise, computes score deltas, and packs the structured metrics into a dictionary list.


* `game_stats(game, engine, your_name, depth=17)`: Aggregates move data for a single game, computing summary statistics like average evaluation change, neutral move percentages, and specific move tier counts (best, good, inaccuracy, mistake, blunder).


* `game_storage_data(games, engine, your_name, depth=17)`: Iterates through bulk game collections to build comprehensive tabular datasets mapping every single game, move index, SAN notation, and score change.


* `classifier(folder_path)`: Recursively searches a directory for analysis CSVs, sorting them automatically into training pools (based on naming conventions starting with `chess_analysis`) and testing pools for safe data ingestion.


* `game_level_analysis(data)`: Groups bulk dataframe rows by game identifier, splits each match into three structural phases (Opening, Middlegame, Endgame), and calculates phase-specific percentages for improved or worsened moves.


* `create_training_baseline(training_games)`: Processes historical game metrics across all phases to calculate robust statistical benchmarks, determining typical performance medians and interquartile ranges (25th and 75th percentiles).


* `compare_data(training_baseline, testing_game)`: Evaluates a new test match against your established historical baseline, classifying individual metrics as "Within usual range," "Below usual range," or "Above usual range."


* `trend_analysis(training_games)`: Splits historical training games in half to compute mean shifts across metrics, identifying whether your play is trending positively or negatively over time.


* `plot_training_trends(training_games)`: Generates interactive or static `matplotlib` line graphs tracking metrics like average evaluation change, blunder rates, and accuracy improvements across sequential game numbers.
