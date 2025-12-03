# Influencer Recommender System

## Quick Start

### 1. Install Dependencies

```bash
cd /home/vlad/Work/fj-recommendations/Mydata
pip install -r requirements.txt
```

### 2. Start Jupyter in Browser

```bash
cd /home/vlad/Work/fj-recommendations/Mydata
jupyter notebook
```

This opens your browser at `http://localhost:8888`. Click on `influencer_recommender.ipynb` to open it.

**Alternative - JupyterLab (modern interface):**
```bash
pip install jupyterlab
jupyter lab
```

### 3. Run the Notebook

- Click **Cell > Run All** (or press `Shift+Enter` cell by cell)
- Wait for all cells to execute (~1-2 minutes)

---

## Usage

After running all cells, use the main function:

```python
# Get top 100 recommendations for a campaign
recs = recommend_influencers(campaign_id=2993, top_n=100)
print(recs)
```

### Parameters

| Parameter | Description |
|-----------|-------------|
| `campaign_id` | Campaign ID from your database |
| `top_n` | Number of recommendations (default: 100) |

### Output Columns

| Column | Description |
|--------|-------------|
| `id` | Influencer ID |
| `account` | Social media handle |
| `name` | Display name |
| `followers` | Follower count |
| `engagement` | Engagement rate |
| `past_acceptances` | Times accepted in past campaigns |
| `historical_rate` | Historical acceptance rate |
| `recommendation_score` | Ranking score (higher = better) |

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
3. **Ranking**: Sorts by recency-weighted past acceptance history

### Key Insight

Past acceptance is the strongest predictor of future acceptance (0.77 correlation). The system prioritizes influencers who have been accepted in recent campaigns.

---

## Performance

Evaluated on 100 newest campaigns:

| Top N | Recall |
|-------|--------|
| 50 | 10.2% |
| 100 | 17.5% |
| 200 | 25.9% |
