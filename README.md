# Berlin Airbnb Hybrid Recommendation System  
### Marketplace Intelligence, Geospatial Analysis, and Personalized Listing Discovery

This project builds a hybrid recommendation system for Airbnb listings in Berlin using public Inside Airbnb data.

The goal is to understand how listing attributes, geospatial context, guest review language, and review-based behavioral proxies can be combined to support more personalized Airbnb listing discovery.

Rather than treating recommendation as a black-box modeling task, this notebook approaches it as a product-oriented data science problem:

**How can a marketplace help travelers find more relevant listings faster while balancing similarity, quality, value, location, and traveler intent?**

---

## Project Overview

Airbnb discovery is a matching problem.

A traveler is not only choosing a place to sleep. They are balancing price, neighborhood, room type, amenities, review quality, availability, location, and the way previous guests describe the stay.

This project combines:

- Exploratory marketplace analysis
- Geospatial analysis
- Review text and sentiment analysis
- Content-based recommendation
- Collaborative filtering proxy using reviewer-listing interactions
- Hybrid recommendation ranking
- Traveler-persona ranking scenarios
- Proxy evaluation without booking or click labels

The final result is an interpretable hybrid recommendation framework designed for portfolio-level product analytics and recommendation system reasoning.

---

## Central Business Question

**How can a hybrid recommendation system personalize Airbnb listing discovery in Berlin using listing features, geospatial context, and review behavior?**

---

## Main Findings

- Berlin Airbnb supply and price patterns vary meaningfully across neighborhoods.
- Location should be treated as ranking context, not simply as distance to the city center.
- Price and value are different signals: expensive areas are not always the strongest value areas.
- Amenities help explain product fit, especially for traveler profiles such as digital nomads, families, budget travelers, and premium guests.
- Guest reviews add useful context beyond structured ratings, although sentiment is treated as a proxy due to multilingual public reviews.
- Reviewer-listing interactions can support collaborative filtering, but only as an imperfect behavioral proxy because public data does not include clicks, searches, saves, impressions, or bookings.
- Hybrid ranking creates a more flexible recommendation framework than pure content similarity by combining listing similarity, behavioral co-occurrence, quality, popularity, sentiment, value, and persona-specific priorities.

---

## Project Structure

```text
berlin-airbnb-hybrid-recommendation-system/
│
├── berlin-airbnb-hybrid-recommendation-system.ipynb
├── README.md
├── requirements.txt
├── .gitignore
│
├── images/
│   ├── price_distribution.png
│   ├── median_price_by_neighborhood.png
│   ├── room_type_distribution_and_rating.png
│   ├── superhost_rating_comparison.png
│   ├── amenity_quality_association.png
│   ├── value_for_money_neighborhood_ranking.png
│   ├── availability_vs_price.png
│   ├── reviews_per_month_by_neighborhood.png
│   ├── correlation_heatmap.png
│   ├── sentiment_category_distribution.png
│   ├── top_positive_review_terms.png
│   ├── sentiment_vs_rating.png
│   ├── average_sentiment_by_neighborhood.png
│   ├── review_length_vs_rating.png
│   ├── listing_density_map.png
│   ├── price_map.png
│   └── neighborhood_choropleth_summary.png
│
├── outputs/
│   └── berlin_listing_density_map.html
│
└── DATA/
    └── raw data files are stored locally and not committed to GitHub
```

---

## Dataset

The project uses public Airbnb data from Inside Airbnb for Berlin.

The raw data files are not included in this repository due to file size limitations.  
To run the notebook locally, download the Berlin dataset from Inside Airbnb and place the files inside a local `DATA/` folder.

Expected files include:

```text
DATA/
├── listings.csv.gz
├── calendar.csv.gz
├── reviews.csv.gz
├── listings.csv
├── reviews.csv
├── neighbourhoods.csv
└── neighbourhoods.geojson
```

The notebook also handles common local filename variations such as `.csv.csv` or `.csv.gz.csv`.

---

## Methodology

The notebook is structured as an end-to-end data science project:

1. **Business Understanding**  
   Defines the marketplace and recommendation problem.

2. **Environment Setup**  
   Loads libraries, global settings, visual style, and reusable helper functions.

3. **Recommendation Framing**  
   Defines the recommendation logic and the main product goal.

4. **Data Collection**  
   Loads listing, review, calendar, and neighborhood data from local Inside Airbnb files.

5. **Data Cleaning and Feature Preparation**  
   Creates defensible features for price, location, amenities, quality, popularity, value, availability, and review behavior.

6. **Exploratory Marketplace Analysis**  
   Studies price distribution, neighborhood differences, room type mix, amenities, availability, review velocity, and feature relationships.

7. **Review Text and Sentiment Analysis**  
   Uses review text to extract sentiment proxies, positive review language, semantic review vectors, and guest-experience context.

8. **Geospatial Marketplace Analysis**  
   Maps listing density, price patterns, and neighborhood-level recommendation context.

9. **Content-Based Recommendation System**  
   Builds a similar-stays recommender using listing attributes, text features, amenities, location, price, and review-derived signals.

10. **Collaborative Filtering Proxy**  
    Uses reviewer-listing interactions as an implicit behavioral signal.

11. **Hybrid Recommendation System**  
    Combines content similarity, collaborative signal, rating, popularity, sentiment, and value into an interpretable ranking framework.

12. **Traveler Personas**  
    Adjusts ranking weights for different traveler profiles.

13. **Proxy Evaluation**  
    Compares recommendation behavior using diversity, novelty, rating, and similarity metrics.

14. **Business Insights**  
    Connects results back to Airbnb discovery, ranking design, and marketplace strategy.

15. **Final Conclusion**  
    Answers the central question and summarizes the value and limitations of the project.

---

# Visual Analysis

## 1. Marketplace and Listing Signals

### Price Distribution

Most Berlin listings are concentrated below the premium price tail, making price bands important for recommendation logic.

![Price Distribution](images/price_distribution.png)

---

### Highest-Priced Neighborhoods

Neighborhood-level price variation shows why location should remain a core ranking feature.

![Median Price by Neighborhood](images/median_price_by_neighborhood.png)

---

### Room Type Supply and Rating Signals

Room type affects both supply structure and traveler expectations, so private rooms and entire homes should not always be treated as direct substitutes.

![Room Type Distribution and Rating](images/room_type_distribution_and_rating.png)

---

### Superhost Review Signals

Superhost listings show stronger observed review signals, but the badge should complement ratings and reviews rather than replace them.

![Superhost Rating Comparison](images/superhost_rating_comparison.png)

---

### Amenity and Rating Association

Amenity signals help describe listing fit beyond price, location, and room type.

![Amenity Quality Association](images/amenity_quality_association.png)

---

### Value-for-Money Neighborhood Signals

Value-oriented neighborhoods are identified using a blend of guest rating and inverse log price.

![Value for Money Neighborhood Ranking](images/value_for_money_neighborhood_ranking.png)

---

### Availability, Price, and Quality

Availability varies across both low-priced and high-priced listings, suggesting that availability should be used carefully in ranking logic.

![Availability vs Price](images/availability_vs_price.png)

---

### Review Velocity by Neighborhood

Review velocity provides a lightweight engagement proxy, although it should not be interpreted as confirmed demand or booking volume.

![Reviews per Month by Neighborhood](images/reviews_per_month_by_neighborhood.png)

---

### Feature Relationships for Ranking Design

Correlation analysis helps identify redundant and complementary ranking signals.

![Correlation Heatmap](images/correlation_heatmap.png)

---

## 2. Review Text and Sentiment Analysis

### Sentiment Categories

The sentiment layer is treated as a coarse proxy because public Airbnb reviews are multilingual and the local fallback method is lexicon-based.

![Sentiment Category Distribution](images/sentiment_category_distribution.png)

---

### Positive Review Language

Positive review language highlights general satisfaction terms and practical stay signals such as location, cleanliness, comfort, helpfulness, and quietness.

![Top Positive Review Terms](images/top_positive_review_terms.png)

---

### Sentiment and Ratings

Sentiment and star ratings are related but not interchangeable, which supports using review text as a complementary context signal.

![Sentiment vs Rating](images/sentiment_vs_rating.png)

---

### Neighborhood-Level Sentiment

Average review sentiment varies across neighborhoods, adding contextual information beyond price and structured listing attributes.

![Average Sentiment by Neighborhood](images/average_sentiment_by_neighborhood.png)

---

### Review Depth and Text Coverage

Review length is not treated as a quality score. It helps estimate how much guest language is available for semantic matching and recommendation context.

![Review Length vs Rating](images/review_length_vs_rating.png)

---

## 3. Geospatial Marketplace Analysis

### Listing Density

Airbnb supply is visibly concentrated in central and inner-city areas.

![Listing Density Map](images/listing_density_map.png)

---

### Price by Location

Higher-priced listings appear across multiple areas rather than forming a single isolated cluster.

![Price Map](images/price_map.png)

---

### Neighborhood-Level Recommendation Context

The choropleth maps translate listing-level signals into neighborhood-level context.

![Neighborhood Choropleth Summary](images/neighborhood_choropleth_summary.png)

---

## Interactive Geospatial Output

The notebook also exports an interactive Folium map:

```text
outputs/berlin_listing_density_map.html
```

The interactive map allows exploration of sampled Berlin Airbnb listings by location, price, room type, and neighborhood.

[View the interactive map file](outputs/berlin_listing_density_map.html)

> Note: GitHub does not render interactive HTML maps directly inside README files.  
> To use the map interactively, clone or download the repository and open `outputs/berlin_listing_density_map.html` in a browser.

Preview of the listing density structure:

![Listing Density Map Preview](images/listing_density_map.png)

---

# Recommendation Systems

## Content-Based Recommendation

The content-based recommender identifies similar listings using:

- Listing text
- Amenities
- Price segment
- Room type
- Neighborhood
- Rating
- Availability
- Popularity
- Sentiment
- Location context

This is useful for “similar stays” recommendations when user history is unavailable.

---

## Collaborative Filtering Proxy

Public Airbnb data does not include clicks, bookings, saves, impressions, or searches.  
Because of that, reviewer-listing interactions are used as an implicit behavioral proxy.

This layer is useful for a portfolio prototype, but it should not be interpreted as true user intent.

---

## Hybrid Recommendation System

The hybrid ranker combines:

```text
Final Score =
0.35 Content Similarity
+ 0.25 Collaborative Score
+ 0.15 Rating Score
+ 0.10 Popularity Score
+ 0.10 Sentiment Score
+ 0.05 Value Score
```

The system remains interpretable because each component is explicitly defined and auditable.

---

## Traveler Personas

The same hybrid recommendation framework is adapted to different traveler needs through persona-specific weights.

Personas included:

- **Budget Traveler**
- **Digital Nomad**
- **Luxury Traveler**
- **Nightlife Traveler**
- **Family Traveler**

These personas are not observed customer segments.  
They are scenario-based ranking profiles used to demonstrate how the recommendation logic can adapt to different traveler priorities.

---

## Proxy Evaluation

Because the dataset does not include real booking or click labels, the project uses proxy evaluation metrics:

- Neighborhood diversity
- Price diversity
- Novelty
- Average rating
- Average similarity when applicable

The hybrid approach is evaluated as a ranking behavior prototype, not as a production-validated recommender.

---

## Technical Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- GeoPandas
- Folium
- Scikit-learn
- SciPy
- TF-IDF Vectorization
- Nearest Neighbors
- Truncated SVD
- NLP sentiment fallback methods

---

## How to Run the Project

1. Clone the repository:

```bash
git clone https://github.com/okinileo/berlin-airbnb-hybrid-recommendation-system.git
cd berlin-airbnb-hybrid-recommendation-system
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Download the Berlin dataset from Inside Airbnb.

4. Place the raw files inside a local `DATA/` folder:

```text
DATA/
├── listings.csv.gz
├── calendar.csv.gz
├── reviews.csv.gz
├── listings.csv
├── reviews.csv
├── neighbourhoods.csv
└── neighbourhoods.geojson
```

5. Open and run the notebook:

```text
berlin-airbnb-hybrid-recommendation-system.ipynb
```

---

## Limitations

This project uses public Airbnb data, which means several production-level signals are unavailable.

The dataset does not include:

- Searches
- Clicks
- Saves
- Impressions
- Bookings
- Cancellations
- Conversion rates
- User sessions
- Real traveler profiles

As a result, the collaborative filtering layer and evaluation metrics are based on proxies.  
The system should be interpreted as an explainable recommendation prototype, not as a production-validated recommender.

---

## What This Project Demonstrates

This project demonstrates practical data science skills in:

- Business problem framing
- Marketplace analysis
- Data cleaning and feature engineering
- Geospatial analytics
- NLP review analysis
- Recommendation system design
- Content-based filtering
- Collaborative filtering proxy
- Hybrid ranking
- Persona-based ranking logic
- Proxy evaluation
- Executive-style analytical storytelling
- Clear communication of limitations

---

## Final Takeaway

Airbnb discovery in Berlin benefits from a multi-signal recommendation approach.

Listing similarity is useful, but it is not enough by itself. Stronger recommendation logic comes from combining structured listing attributes, neighborhood context, price and value signals, guest review language, behavioral proxies, and traveler-specific ranking priorities.

Overall, this project shows how a hybrid recommendation framework can make accommodation discovery more personalized, explainable, and product-oriented while remaining honest about the limits of public marketplace data.
