# Influencer Recommender System

## Quick Start

### 1. Create Conda Environment (Python 3.11)

LightFM requires Python 3.11 or earlier. Create a dedicated environment:

```bash
conda create -n recommender python=3.11
conda activate recommender
```

### 2. Install Dependencies

```bash
cd /home/vlad/Work/fj-recommendations/Mydata
pip install -r requirements.txt
conda install -c conda-forge lightfm
```

### 3. Start Jupyter in Browser

```bash
jupyter notebook
```

This opens your browser at `http://localhost:8888`. Click on `influencer_recommender.ipynb` to open it.

**Alternative - JupyterLab (modern interface):**
```bash
jupyter lab
```

### 4. Run the Notebook

- Click **Cell > Run All** (or press `Shift+Enter` cell by cell)
- Wait for all cells to execute (~2-3 minutes)

---

## Usage

After running all cells, use the main function:

```python
# Ensemble method (recommended) - combines all signals
recs = recommend_influencers_v3(campaign_id=2993, top_n=100, method='ensemble')

# Other methods available:
recs = recommend_influencers_v3(campaign_id=2993, top_n=100, method='history')       # Past acceptance only
recs = recommend_influencers_v3(campaign_id=2993, top_n=100, method='collaborative') # Similar campaigns
recs = recommend_influencers_v3(campaign_id=2993, top_n=100, method='lightfm')       # Matrix factorization
```

### Parameters

| Parameter | Description |
|-----------|-------------|
| `campaign_id` | Campaign ID from your database |
| `top_n` | Number of recommendations (default: 100) |
| `method` | `'ensemble'`, `'history'`, `'collaborative'`, or `'lightfm'` |

### Output Columns

| Column | Description |
|--------|-------------|
| `id` | Influencer ID |
| `account` | Social media handle |
| `name` | Display name |
| `followers` | Follower count |
| `engagement` | Engagement rate |
| `past_acceptances` | Times accepted in past campaigns |
| `history_score` | Score from past acceptance history |
| `collab_score` | Score from similar campaigns |
| `lightfm_score` | Score from matrix factorization model |
| `final_score` | Combined ranking score (higher = better) |

---

## Data Files

All data is in the `data/` folder:

| File | Description |
|------|-------------|
| `Campaigns.csv` | Campaign info (2,383 campaigns) |
| `Influencers.csv` | Influencer profiles (63,932 influencers) |
| `Interactions.csv` | Campaign applications (366,352 interactions) |
| `Briefs.csv` | Campaign briefs |
| `Groups.csv` | Campaign targeting groups |
| `User_Info.csv` | User demographics |
| `Group_*.csv` | Group filter definitions |

---

## How It Works

1. **Group Filters**: Applies campaign targeting (tier, country, location, category, gender, age)
2. **Network Filter**: Matches influencers to campaign network (Instagram=1, TikTok=8)
3. **Scoring**: Combines multiple signals:
   - **History**: Recency-weighted past acceptance count
   - **Collaborative**: Acceptance in similar campaigns (by description + targeting)
   - **LightFM**: Hybrid matrix factorization with influencer features

### Recommendation Methods

| Method | Description | Best For |
|--------|-------------|----------|
| `history` | Ranks by past acceptance history | Known high-performers |
| `collaborative` | Ranks by acceptance in similar campaigns | Discovering new influencers |
| `lightfm` | Matrix factorization with WARP loss | Cold-start handling |
| `ensemble` | Combines all signals (40% history, 30% collab, 30% lightfm) | Best overall (recommended) |

---

## Performance

Evaluated on 100 newest campaigns (run notebook to see actual results):

| Method | Top 50 | Top 100 | Top 200 |
|--------|--------|---------|---------|
| History (baseline) | ~10% | ~17% | ~26% |
| Collaborative | TBD | TBD | TBD |
| LightFM | TBD | TBD | TBD |
| Ensemble | TBD | TBD | TBD |
