# FAA Wildlife Strike Analysis — Chart Notebook

Regenerates the five charts used in `FAA Wildlife Strike Analysis - Working
Draft (Sept 23).docx` directly from the raw dataset, in one notebook.

## Setup

1. Make sure `FAA Strike Analysis Working.xlsx` is in `data/`. It is required
   because the notebook reads both the `Component Analysis` and `Strike
   Analysis` sheets. The file is about 224 MB and is excluded by `.gitignore`
   because GitHub rejects files over 100 MB. Share this workbook separately
   with the team, keeping this exact filename and folder. `data/us-states.geojson`
   is included and is needed for the map.

2. Create the virtual environment and install the required libraries:

   ```bash
   python3 -m venv .venv

   # macOS / Linux
   source .venv/bin/activate
   # Windows
   .venv\Scripts\activate

   pip install -r requirements.txt
   ```

3. (Optional but recommended) Register the venv as a Jupyter kernel so it
   shows up by name instead of the system default:

   ```bash
   python -m ipykernel install --user --name faa-wildlife --display-name "FAA Wildlife Strike (venv)"
   ```

4. Run the notebook in VS Code:

    - Install the Microsoft **Python** and **Jupyter** extensions.
    - Open this project folder in VS Code, then open
       `FAA_Wildlife_Strike_Charts.ipynb`.
    - In the notebook toolbar, select **Kernel** (or **Select Kernel**) and
       choose **Python Environments** > `.venv/bin/python`.
    - Use **Run All** in the notebook toolbar. The first data-load cell is the
       slow step; later runs use the cache in `data/cache/`.

    If `.venv/bin/python` does not appear, choose **Select Another Kernel** >
    **Python Environments**, or use **Python: Select Interpreter** from the
    Command Palette and select `.venv/bin/python`.

5. Alternatively, launch Jupyter in a browser and select the
   `FAA Wildlife Strike (venv)` kernel:

   ```bash
   jupyter notebook FAA_Wildlife_Strike_Charts.ipynb
   # or: jupyter lab
   ```

   Run all cells top to bottom. The data-load cell is the slow step (~1
   minute, mostly the 356k-row `Strike Analysis` sheet); it caches to
   `data/cache/*.pkl` so later runs are fast until the source workbook
   changes.

## What's in here

```
FAA_Wildlife_Strike_Charts.ipynb   the notebook (already executed once —
                                    open it to see the charts without
                                    re-running anything)
requirements.txt                   required libraries and minimum versions
data/FAA Strike Analysis Working.xlsx
                                    source workbook (required, shared separately)
data/us-states.geojson             state boundaries for the choropleth
data/cache/                         cached sheets (optional; speeds up reruns)
figures/                            the five PNGs the notebook last produced
```

Each of the five sections in the notebook computes its own aggregate
straight from the `Component Analysis` and `Strike Analysis` sheets — no
hidden preprocessing — and saves one chart to `figures/`. A shared style
block near the top keeps colors and type consistent across all five so they
read as one system.

## Notes on scope

Both source sheets include some rows outside the four flight phases the
report focuses on (Take-off Run, Climb, Approach, Landing Roll) — tagged
`Critical Phase == "Other"` / `Flight Group == "Other"`. Every aggregation
in the notebook explicitly filters those out before computing a rate, so
the numbers match the report. If you extend the notebook, keep that filter
when you add anything new.
