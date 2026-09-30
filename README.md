# Human Behavioral Experiment

Phosphene pattern learning experiment comparing Symbol vs Pixel encoding.

## Files

```
human_experiment/
├── generate_stimuli.py   # Export phosphene images from trained encoders
├── experiment.html       # jsPsych experiment (open in browser)
├── analyze_results.py    # Statistical analysis of collected data
├── stimuli/              # Generated phosphene images (after running generate_stimuli.py)
│   ├── symbol/           # Symbol-encoded patterns
│   ├── pixel/            # Pixel-encoded patterns
│   └── manifest.json     # Stimulus metadata
├── data/                 # Raw CSV data from participants
└── figures/              # Analysis output figures
```

## Step 1: Generate Stimuli

Run on the server where trained models exist:

```bash
cd human_experiment
python3 generate_stimuli.py
```

This exports 200 phosphene images per condition (8 classes x 25 samples).

## Step 2: Deploy Experiment

Option A - Local (simplest):
```bash
cd human_experiment
python3 -m http.server 8000
# Open browser: http://localhost:8000/experiment.html?condition=symbol&pid=P001
```

Option B - GitHub Pages (for remote participants):
1. Push this folder to a GitHub repo
2. Enable GitHub Pages in repo settings
3. Share the URL with participants

## Step 3: Run Participants

Each participant gets ONE link (between-subject design):

- **Symbol group**: `experiment.html?condition=symbol&pid=P001`
- **Pixel group**: `experiment.html?condition=pixel&pid=P002`

Assign participants randomly. Use odd PIDs for symbol, even for pixel (or any scheme).

Aim for **10-20 per group** (20-40 total). Each session takes ~25 minutes.

After completion, participants download a CSV file automatically. Collect these files.

## Step 4: Analyze

Put all CSV files in `data/`, then:

```bash
python analyze_results.py --data-dir data/
```

Outputs:
- `figures/learning_curves.png` - Learning curves with 95% CI
- `figures/test_accuracy.png` - Box plot with t-test
- `figures/stats.json` - Full statistical results

## Attention Checks

The experiment includes:
- Practice trials (5) to ensure interface understanding
- Progress feedback every 50 trials
- Subjective difficulty rating at end

Exclude participants with:
- Test accuracy < chance (12.5%) AND learning phase accuracy < chance
- Total experiment time < 10 minutes (not paying attention)
- Incomplete data (< 250 trials)

## IRB Notes

This experiment requires ethics approval. Key points for your application:
- Minimal risk (screen-based visual task only)
- Anonymous data (no personal info collected)
- Voluntary participation with right to withdraw
- Normal/corrected vision required (self-reported)
- Duration: ~25-30 minutes
