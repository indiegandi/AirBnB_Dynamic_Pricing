# Airbnb Dynamic Pricing Analysis 🏠📈

## Overview
This project explores a practical approach to **dynamic pricing** for short-term rentals listed on Airbnb. The goal is to estimate competitive pricing for a specific listing by analyzing comparable properties and overall market demand.

The analysis focuses on listings in the same market segment and uses scraped search results to approximate both competitor prices and short-term demand conditions. Based on this information, a simple pricing algorithm generates a suggested nightly rate.

The project was developed as an applied data analysis exercise and demonstrates how publicly visible marketplace data can be used to inform pricing decisions.

---

## 🛠 Data Collection Challenges
Collecting listing data from Airbnb presents several technical constraints. Airbnb actively restricts automated scraping and frequently modifies the structure of its web pages.

**The main challenges encountered:**
* **Dynamic Selectors:** Airbnb regularly changes the internal keys that store pricing information.
* **Lazy Loading:** Search results use lazy loading, meaning not all listings are immediately available in the page source.
* **Anti-Bot Mechanisms:** Automated navigation between pages is often blocked.

> **Note:** Because of these limitations, the project focuses on collecting a **filtered subset** of listings that represent relevant competitors rather than attempting to capture the entire market.

---

## 🎯 Competitor Selection
Instead of scraping every listing in the region, the analysis filters for properties comparable to the target:
* **Capacity:** Up to 4 guests.
* **Amenities:** Must have a separate kitchen.
* **Characteristics:** Similar property types.
* **Price Range:** Weekend prices between **5,000 and 12,000 Turkish Lira**.

---

## 📊 Estimating Market Occupancy
Since Airbnb does not provide direct occupancy data, demand is approximated using **listing availability**.

### Method:
1. Run a search for the **entire month** to determine total market capacity.
2. Run a search for **specific dates** of interest.
3. Compare the results to estimate bookings.

### Formula:
$$\text{Occupancy Rate} = \frac{\text{Total Listings} - \text{Available Listings}}{\text{Total Listings}}$$

**Example:**
* Full-month search (April): **220 listings**
* Search for April 10–12: **108 available listings**
* Estimated occupancy: $(220 - 108) / 220 = 50.9\%$

---

## 💰 Pricing Data Adjustment
Airbnb displays "Gross Prices" (including cleaning fees and ~15% service fees). To determine the host's actual revenue, the algorithm converts these into **Net Values**:
* **Platform commissions are removed** to find the effective revenue.
* **Cleaning fees are included** in the adjusted nightly pricing to ensure fair comparison across different fee structures.

---

## 🤖 Dynamic Pricing Algorithm
The recommendation is calculated in four steps:

1.  **Market Benchmark:** Uses the **median** net price of comparable listings (more robust than the average).
2.  **Occupancy-Based Adjustment:** A dynamic multiplier is applied:
    * `1.25` if occupancy > 80%
    * `1.10` if occupancy > 60%
    * `1.00` otherwise
3.  **Target Net Price:** `target_net = median_net * multiplier`
4.  **Convert to Suggested Listing Price:** * `suggested_gross = (target_net / 2) / 0.85`

---

## ✨ Strategic Adjustments
The algorithm provides a baseline. Hosts can manually increase the multiplier if the listing offers:
* Superior location or interior design.
* Higher quality amenities.
* Stronger guest reviews.

---

## 🔧 Tools & Libraries
* **Python**
* **Pandas** & **NumPy** (Data Manipulation)
* **Requests** (Scraping utilities)



---

## ⚠️ Disclaimer
This project relies on publicly visible search results and approximate demand indicators. The calculated occupancy rates and price recommendations should be interpreted as **analytical estimates** rather than precise market measurements.
