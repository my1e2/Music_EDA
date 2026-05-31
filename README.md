# Module 1 Assignment — Musical Data Understanding and Visualization (1950–2019)

> Course: Exploratory Data Analysis  
> Module: 1  
> Dataset: Music Dataset: Lyrics and Metadata from 1950 to 2019  

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Dataset](#dataset)
3. [Requirements](#requirements)
4. [Installation and Setup](#installation-and-setup)
5. [How to Run](#how-to-run)
6. [Analysis and Visualizations](#analysis-and-visualizations)
   - [Visualization 1: Genre Distribution Bar Chart](#visualization-1-genre-distribution-bar-chart)
   - [Visualization 2: Evolution of Lyrical Themes Over Time](#visualization-2-evolution-of-lyrical-themes-over-time)
   - [Visualization 3: Genre vs Lyrical Theme Heatmap](#visualization-3-genre-vs-lyrical-theme-heatmap)
   - [Visualization 4: Lyric Word Cloud](#visualization-4-lyric-word-cloud)
   - [Visualization 5: Songs by Genre per Decade](#visualization-5-songs-by-genre-per-decade)
7. [Feature Reference](#feature-reference)
8. [Key Observations](#key-observations)
9. [Limitations and Future Work](#limitations-and-future-work)

---

## Project Overview

This module assignment performs foundational exploratory data analysis on a large music dataset spanning 1950 to 2019. The goal is to understand the distribution of musical genres over time, trace how lyrical and audio themes evolved across seven decades, and visually map the relationships between genre identity and thematic content. Five distinct visualizations are produced, ranging from basic distribution charts to a smoothed multi-line trend plot and an annotated heatmap.

---

## Dataset

**File:** `tcc_ceds_music.csv`  
**Source:** Mendeley Data (V3)  
**Citation:**  
Moura, L., Fontelles, E., Sampaio, V., & França, M. (2020). *Music Dataset: Lyrics and Metadata from 1950 to 2019*. Mendeley Data, V3. DOI: [10.17632/3t9vbwxgr5.3](https://doi.org/10.17632/3t9vbwxgr5.3)  
**Kaggle mirror:** [https://www.kaggle.com/datasets/saurabhshahane/music-dataset-1950-to-2019/data](https://www.kaggle.com/datasets/saurabhshahane/music-dataset-1950-to-2019/data)

The dataset contains song-level records with audio signal features, NLP-derived lyrical context scores, and metadata including genre, release year, artist name, and full lyric text.

---

## Requirements

- Python 3.x
- No external APIs or network access required after dataset download

```bash
pip install pandas matplotlib seaborn wordcloud
```

| Package      | Purpose                                                       |
|--------------|---------------------------------------------------------------|
| `pandas`     | Data loading, grouping, rolling averages, and decade column   |
| `matplotlib` | Base plotting for bar charts, line plot, and word cloud       |
| `seaborn`    | Heatmap and count plot                                        |
| `wordcloud`  | Word cloud generation from aggregated lyric text              |

---

## Installation and Setup

1. Download `tcc_ceds_music.csv` from Mendeley Data or Kaggle and place it in the same directory as the notebook.
2. Open `Module1Assignment.ipynb` in Jupyter Notebook or JupyterLab.
3. Run all cells from top to bottom.

---

## How to Run

Open the notebook and execute the single code cell. All five visualizations render inline in sequence. No functions are defined — the code runs procedurally in one cell.

---

## Analysis and Visualizations

### Visualization 1: Genre Distribution Bar Chart

**Type:** Vertical bar chart  
**Figure size:** 10x6  
**Color scheme:** Seven custom pastel hex colors, one per genre bar

Genre counts are obtained using `df['genre'].value_counts()`. The chart is annotated with value labels on top of each bar using `ax.bar_label()` with `fmt='%.0f'` to suppress floating-point display and `padding=2` for readability. X-tick labels are rotated 45 degrees.

This visualization provides an immediate overview of how evenly or unevenly genres are represented in the dataset, which is foundational context for all subsequent genre-level analyses.

---

### Visualization 2: Evolution of Lyrical Themes Over Time

**Type:** Multi-line trend plot  
**Figure size:** 25x20  
**Lines:** 22 (one per theme/feature column)

The 22 theme and audio feature columns are defined in `theme_columns`. The dataset is grouped by `release_date`, the mean value for each theme is computed per year, and a 5-year centered rolling average is applied using `.rolling(5, center=True).mean()`. The `center=True` argument means each year's smoothed value incorporates two years before and two years after it, producing a truer representation of gradual rises and falls while filtering out year-to-year noise.

Each theme is drawn as a distinct line on the same plot with `linewidth=2.5`. The legend is placed outside the plot area to the upper right using `bbox_to_anchor=(1.05, 1)` to prevent overlap with the data. A light grid (`alpha=0.3`) aids readability across the large figure.

**Features plotted:** `dating`, `violence`, `world/life`, `night/time`, `shake the audience`, `family/gospel`, `romantic`, `communication`, `obscene`, `music`, `movement/places`, `light/visual perceptions`, `family/spiritual`, `like/girls`, `sadness`, `feelings`, `danceability`, `loudness`, `acousticness`, `instrumentalness`, `valence`, `energy`

---

### Visualization 3: Genre vs Lyrical Theme Heatmap

**Type:** Annotated heatmap  
**Figure size:** 16x10  
**Color map:** `YlOrRd` (Yellow-Orange-Red)

Average values for each of the 22 theme columns are computed per genre using `df.groupby('genre')[theme_columns].mean()`. The resulting DataFrame is passed directly to `sns.heatmap()` with `annot=True` to display the numeric value in each cell, `fmt='.3f'` to show three decimal places, and a color bar labeled "Average Theme Intensity".

This visualization allows direct comparison of how each genre's identity is characterized by its thematic profile. For example, genres with high `obscene` or `violence` scores versus those with high `family/gospel` or `sadness` scores become visually distinct through color intensity.

---

### Visualization 4: Lyric Word Cloud

**Type:** Word cloud image (rendered via matplotlib `imshow`)  
**Figure size:** 10x6  
**Background:** Black  
**Color map:** Rainbow  
**Max words:** 150

All lyric strings are concatenated into a single space-separated string using `" ".join(lyric for lyric in df['lyrics'].astype(str))`. The `.astype(str)` call handles any non-string or null entries. A `WordCloud` object is instantiated with the specified settings and `.generate()` is called on the combined lyric string.

The cloud is rendered using `plt.imshow()` with `interpolation='bilinear'` for smooth rendering, and `plt.axis('off')` removes the axis lines since the image does not function as a conventional plot.

The word cloud gives an intuitive, at-a-glance picture of which words dominate across all 28,000+ songs in the dataset spanning seven decades.

---

### Visualization 5: Songs by Genre per Decade

**Type:** Grouped count plot  
**Figure size:** 16x10  
**Color palette:** `Set3`

A `decade` column is engineered from `release_date` using integer floor division: `(df['release_date'] // 10) * 10`. This maps each year to its corresponding decade (e.g., 1967 becomes 1960). A `seaborn.countplot()` is then produced with `decade` on the x-axis and `genre` as the hue grouping.

Value labels are added above every bar using `ax.bar_label()` with `fontsize=8`. The legend is placed outside the chart area to prevent overlap. This visualization reveals both the growth of the dataset over time and the changing dominance of different genres across decades.

---

## Feature Reference

The 22 columns analyzed as themes fall into two categories:

**NLP Lyrical Context Scores (values in [0.0, 1.0]):**

| Column                     | Description                                            |
|----------------------------|--------------------------------------------------------|
| `dating`                   | Romantic relationship / dating lyrical content         |
| `violence`                 | Violent or aggressive lyrical themes                   |
| `world/life`               | Existential or philosophical content                   |
| `night/time`               | Temporal or nighttime setting references               |
| `shake the audience`       | Content designed to excite or shock                    |
| `family/gospel`            | Family values or gospel-related content                |
| `romantic`                 | Romantic love and affectionate sentiment               |
| `communication`            | Themes of conversation and interpersonal expression    |
| `obscene`                  | Explicit or vulgar lyrical content                     |
| `music`                    | Self-referential content about music                   |
| `movement/places`          | Travel, location, and physical movement references     |
| `light/visual perceptions` | Imagery involving light, color, and visuals            |
| `family/spiritual`         | Spiritual practices or family-spiritual content        |
| `like/girls`               | Admiration or attraction content                       |
| `sadness`                  | Expressions of grief, sorrow, or loneliness            |
| `feelings`                 | General emotional expressiveness                       |

**Audio Signal Features:**

| Column            | Description                                              |
|-------------------|----------------------------------------------------------|
| `danceability`    | Suitability for dancing based on rhythm and beat         |
| `loudness`        | Overall track loudness in dB                             |
| `acousticness`    | Confidence measure of acoustic instrumentation           |
| `instrumentalness`| Probability of no vocal content                          |
| `valence`         | Musical positiveness (high = happy, low = sad/tense)     |
| `energy`          | Perceptual intensity and activity level                  |

---

## Key Observations

- Genre representation in the dataset is unequal. Certain genres such as pop and rock appear more frequently than blues or reggae, which is important context for any genre-level average interpretations.
- The rolling average trend lines reveal that audio features like `loudness` and `energy` generally increased over the latter decades of the dataset, consistent with the widely documented "loudness war" in music production.
- The heatmap shows clear genre-level thematic signatures — for example, hip hop carries distinctly higher average `obscene` and `violence` scores, while gospel/country-adjacent genres score higher on `family/gospel` and lower on `obscene`.
- The word cloud reveals function words dominate lyrical content even after joining all lyrics, suggesting stopword removal would be necessary for any further NLP analysis.
- Decade-level counts show an increase in dataset coverage in later decades, reflecting the greater availability of digital music metadata from the 1990s onward.

---

## Limitations and Future Work

- The word cloud does not apply stopword filtering, meaning common English function words ("the", "and", "I") appear prominently and reduce the signal value of the visualization.
- The 5-year rolling average truncates the first and last two years of the trend lines due to insufficient surrounding data points at the edges of the 1950–2019 range.
- Genre is treated as a flat single-label category. Many songs span multiple genres, and a multi-label approach would provide a more accurate genre-theme mapping.
- Future work could apply dimensionality reduction (PCA, t-SNE) to the 22-column theme space to identify whether natural genre clusters emerge from the audio and lyrical features alone.

