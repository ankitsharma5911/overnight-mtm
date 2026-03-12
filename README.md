# OVERNIGHT_MTM

Backtesting workflow for an overnight MTM options strategy using weekly windows, parameter sweeps, and chunked output generation.

## What This Project Contains

- `OVERNIGHT_MTM.ipynb`: Main strategy logic and batch execution loop.
- `Run Code.py`: Utility runner that can process notebook-based backtests with local/temp script conversion and path handling.
- `Parameter_OVERNIGHT_MTM.csv`: Parameter grid used for strategy runs.
- `Parameter_OVERNIGHT_MTM_MetaData.csv`: Meta-level controls (index/date ranges/run flags/time config).
- `OVERNIGHT_MTM_output/`: Generated output files (parquet/csv depending on workflow).
- `OVERNIGHT_MTM_dashboard/`: Dashboard-ready summarized CSV outputs by index.

## Strategy Flow (High Level)

1. Load strategy parameters and metadata.
2. Build weekly date groups from metadata.
3. For each active metadata row:
4. Create a `WeeklyBacktest` object for the selected week.
5. Iterate parameter rows in chunks.
6. Run `OVERNIGHT_MTM(...)` for each parameter set.
7. Save chunk outputs and aggregate total PnL fields.

## Prerequisites

- Python 3.10+ recommended.
- Access to pickle data folders (example: `P:/PGC Data/PICKLE/`).
- Required Python packages:
  - `pandas`
  - `polars`
  - `pyarrow`
  - `tqdm`
  - `nbformat`
  - `nbconvert`
  - `psutil`
  - `pgcbacktest` (internal/private dependency)

Install common dependencies:

```bash
pip install pandas polars pyarrow tqdm nbformat nbconvert psutil
```

## How To Run

### Option 1: Run the notebook

1. Open `OVERNIGHT_MTM.ipynb`.
2. Verify paths in the first code cell:
   - `pickle_path`
   - `parameter_path`
   - `meta_data_path`
   - `output_csv_path`
3. Run all cells.

### Option 2: Run the script utility

```bash
python "Run Code.py"
```

This runner includes helpers for:

- Selecting files/folders via GUI dialogs.
- Resolving pickle path by metadata/index and DTE profile.
- Converting notebook code into a temporary Python script for execution workflows.

## Output Notes

- Backtest chunk outputs are written into `OVERNIGHT_MTM_output/`.
- Dashboard CSV summaries are under `OVERNIGHT_MTM_dashboard/` by index (for example NIFTY/SENSEX folders).

## Common Adjustments

- Change `maxre` in notebook logic to alter number of re-entries.
- Tune `chunk_size` to balance memory usage and speed.
- Control active runs via `run` flag in `Parameter_OVERNIGHT_MTM_MetaData.csv`.

## Troubleshooting

- If `pgcbacktest` import fails, ensure internal package is installed and available in the active environment.
- If pickle path is invalid, update it in the notebook first cell or use the script workflow that auto-resolves path from metadata.
- If output files are missing, verify `run` flags and date ranges in metadata.
