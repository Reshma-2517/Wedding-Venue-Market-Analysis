# Wedding Venue Marketplace Analytics
### Web Scraping, Exploratory Data Analysis (EDA), Statistical Testing, and Business Insights

> **Project focus:** Analyze wedding venue listings in Hyderabad published on WedMeGood to understand how venue pricing varies with venue characteristics, guest capacity, and location—and translate the findings into practical insights for customers, venue owners, and the marketplace.

## Table of Contents
1. [Project Overview](#project-overview)
2. [Business Problem](#business-problem)
3. [Project Objectives](#project-objectives)
4. [Stakeholders and Business Value](#stakeholders-and-business-value)
5. [Research Questions](#research-questions)
6. [Data Collection](#data-collection)
7. [Dataset and Features](#dataset-and-features)
8. [Data Preparation](#data-preparation)
9. [Exploratory Data Analysis](#exploratory-data-analysis)
10. [Hypothesis Testing](#hypothesis-testing)
11. [Key Findings](#key-findings)
12. [Business Recommendations](#business-recommendations)
13. [Tools and Technologies](#tools-and-technologies)
14. [Project Workflow](#project-workflow)
15. [How to Run the Project](#how-to-run-the-project)
16. [Limitations and Considerations](#limitations-and-considerations)
17. [Future Enhancements](#future-enhancements)
18. [Conclusion](#conclusion)
19. [Author](#author)

---

## Project Overview

Choosing a wedding venue is a major planning decision. Customers must balance price, location, guest capacity, venue type, accommodation, and customer feedback. Comparing venues is not always straightforward because listings may present prices in different formats—for example, a fixed rental fee per function, a per-plate price, both, or no public price.

This project uses web scraping and exploratory data analysis to study wedding venue listings for Hyderabad on WedMeGood. The workflow covers collecting listing information, preparing the data for analysis, examining distributions and relationships, performing selected statistical tests, and translating the results into business recommendations.

The analysis is based on the listings collected for this project. It should be interpreted as an analysis of the observed listings, not as a complete census of every wedding venue in Hyderabad or a guarantee of the final price a customer will pay.

## Business Problem

Customers may struggle to identify venues that fit their budget and guest count because venue prices, capacities, locations, and included services differ. A low advertised price may not represent the total event cost, particularly when food is priced separately or when important details are unavailable.

Venue owners also need context for understanding how their listed prices compare with other venues of similar type, capacity, or location. A marketplace such as WedMeGood can use structured listing information to improve search filters, comparison tools, and the completeness of venue profiles.

### Business statement

**Analyze Hyderabad wedding venue listings from WedMeGood to understand how advertised pricing varies with venue type, guest capacity, location, and pricing model, and identify opportunities to improve venue comparison and marketplace listing quality.**

## Project Objectives

- Collect publicly displayed venue-listing information from the target website.
- Convert listing information into a structured dataset suitable for analysis.
- Inspect missing values, inconsistent formats, and differences in how prices are presented.
- Summarize rental and per-plate pricing distributions.
- Explore relationships between rental cost, guest capacity, venue type, and area.
- Use hypothesis testing to assess whether mean rental costs differ across selected groups.
- Translate analytical findings into recommendations for customers, venue owners, and the marketplace.
- Document the limitations of the collected data and identify useful next steps.

## Stakeholders and Business Value

| Stakeholder | Potential value from the analysis |
|---|---|
| Customers and families | Compare venue options using budget, guest count, area, and venue type; understand why advertised prices may not be directly comparable. |
| Venue owners and managers | Benchmark listed prices against relevant venues and identify the importance of communicating capacity, pricing, and amenities clearly. |
| Marketplace team | Identify missing or inconsistent listing details and opportunities to improve filters, price transparency, and venue comparisons. |

## Research Questions

1. What is the distribution of advertised rental costs, and how wide is the range?
2. Which pricing models are most commonly displayed?
3. What is the relationship between guest capacity and rental cost?
4. How does rental pricing vary across venue types?
5. How do observed rental costs vary by area?
6. Does mean rental cost differ across venue types?
7. Does mean rental cost differ across guest-capacity groups?
8. What data-quality and listing-transparency improvements could make venue comparisons easier?

---

## Data Collection

### Source

- **Website:** WedMeGood
- **Target listing category:** Wedding venues in Hyderabad
- **Collection scope documented for this project:** 40 result pages

### Approach

The collection workflow used Python-based web scraping techniques:

- **Requests** to send HTTP requests to listing pages.
- **Browser-like request headers** after an initial request returned HTTP 403.
- **BeautifulSoup** to parse the returned HTML.
- **Regular expressions** where useful for extracting or interpreting text.
- **Pandas** to organize the extracted records into tabular form and save the result as a CSV file.

The collection process extracted available information from venue listing cards and associated listing text. The exact fields available varied by listing, and some fields were missing or represented in different formats.

### Collection considerations

A website may change its HTML structure, request restrictions, pagination, or listing content over time. A scraper that works at one point may require maintenance later. The dataset is therefore a snapshot of the listings captured during collection.

> **Reproducibility note:** The project documentation records Requests and BeautifulSoup as the collection approach and notes that browser-like headers were added after an HTTP 403 response. Do not describe Selenium or browser automation as part of the implemented workflow unless the executed code confirms that it was used.

---

## Dataset and Features

The dataset contains listing-level information and analysis-oriented columns derived during preparation. The main fields include the following.

| Feature | Meaning |
|---|---|
| `venue_name` | Name of the venue listing. |
| `detail_url` | URL associated with the venue's detail page. |
| `page` | Result page from which the listing was collected. |
| `is_paid` | Listing-level indicator created for the project's analysis; interpretation depends on the implemented extraction logic. |
| `rating` | Displayed venue rating, where available. |
| `reviews` | Displayed review count, where available. |
| `area` | Area/location value extracted or standardized for analysis. |
| `venue_type_raw` | Original venue-type text collected from the listing. |
| `venue_group` | Grouped venue category used for analysis. |
| `space_type` | Venue/space classification, where available. |
| `veg_price` | Displayed vegetarian per-plate price, where available. |
| `nonveg_price` | Displayed non-vegetarian per-plate price, where available. |
| `rental_cost` | Rental price per function, where a usable value was available. |
| `rental_source` | Information about the source or interpretation of the rental-price value. |
| `pricing_type` | Classification of the displayed pricing model. |
| `has_rental_cost` | Indicator of whether a rental-cost value was available. |
| `capacity_min` | Lower end of the listed guest-capacity range, where available. |
| `capacity_max` | Upper end of the listed guest-capacity range, where available. |
| `rooms` | Number of rooms advertised, where available. |
| `has_rooms` | Indicator of whether room information was available. |
| `extra_features` | Additional extracted listing details, where available. |
| `rental_tier` | Derived rental-price category. |
| `capacity_group` | Derived guest-capacity category. |
| `rating_group` | Derived rating category. |
| `review_group` | Derived review-count category. |
| `enough_reviews` | Derived indicator used to flag listings meeting a review-count criterion. |
| `cost_per_guest` | Derived metric created from the available rental and capacity data; its interpretation depends on the capacity value selected in the code. |
| `log_rental_cost` | Log-transformed rental-cost feature. |
| `log_capacity` | Log-transformed capacity feature. |

Not every field is suitable for every analysis. Some are raw collection fields, while others are derived variables used for grouping, quality checks, or analysis. The notebook's transformation code should be treated as the authoritative definition of each derived field.

### Important pricing distinction

The listing's advertised price may represent different things:

- **Per-plate pricing:** a food price per guest.
- **Rental per function:** a venue rental fee for an event/function.
- **Both:** both a plate price and a rental fee are displayed.
- **Price on request / unavailable:** a comparable numeric price is not publicly available in the collected listing.

These price types should not be combined as though they represent the same cost. A per-plate price is not directly comparable to a fixed rental fee. Even a rental fee may exclude catering or other event expenses.

---

## Data Preparation

The preparation stage converted extracted listing text into a dataset suitable for analysis.

### Main preparation goals

1. **Standardize fields:** Organize listing information into consistent columns.
2. **Interpret prices:** Separate per-plate prices from rental fees instead of treating all displayed prices as equivalent.
3. **Extract numerical values:** Represent ratings, review counts, prices, capacities, and room counts numerically where possible.
4. **Handle missing information:** Preserve the distinction between an unavailable value and a valid zero. Missing prices should not automatically be interpreted as free venues.
5. **Create analysis groups:** Use fields such as `venue_group` and `capacity_group` to compare categories.
6. **Create derived metrics where appropriate:** Examples include `cost_per_guest` and log-transformed fields. Their formulas and assumptions should be documented in the notebook.
7. **Remove redundant analysis columns when needed:** Keep the final analysis table focused on the variables required for the planned charts and tests.

### Data-quality issues encountered

- Missing prices, locations, or other listing details.
- Different price formats and pricing models.
- Inconsistent venue-type descriptions.
- Different amounts of information displayed across listing cards.
- Potential differences between advertised prices and the final event cost.

### Data-quality principle

A missing value is not automatically zero. For example, a venue without a published rental cost should not be classified as having a ₹0 rental cost. Analysis should use the relevant available values and disclose the resulting sample size.

---

## Exploratory Data Analysis

EDA was used to understand the dataset before drawing conclusions. The analysis includes univariate, bivariate, and multivariate exploration, followed by selected hypothesis tests.

### 1. Univariate analysis: Rental-cost distribution

**Business question:** What is a typical advertised rental cost, and how widely do prices vary?

The analysis summarized the distribution of rental costs using measures such as the median, quartiles, and range.

Reported results from the project:

- Median rental cost: **₹94,000 per function**
- Middle 50% of observed rental costs: **₹45,000–₹2,60,000**
- Observed range: **₹3,500–₹35,00,000**

**Interpretation:** The rental prices in the collected data vary widely. The median provides a useful description of the center without being as strongly affected by extreme values as the mean can be.

**Business relevance:** A single “average venue price” may not adequately represent the variety of available options. Budget filters and price bands can help customers narrow their search.

These figures describe the rental-cost values included in that analysis, not every listing or every actual event cost.

### 2. Pricing-model analysis

**Business question:** How do venues present their prices?

Reported distribution of listing pricing models:

- Per-plate pricing: **55%**
- Fixed rental per function: **18%**
- Both types displayed: **10%**
- No public price shown: **17%**

Among venues with available per-plate prices, the middle 50% were reported to be approximately **₹360–₹800 per plate**.

**Interpretation:** Listings use different pricing structures, which makes direct comparison difficult. A customer needs to consider guest count, catering, and what is included before comparing the total cost of two venues.

**Business relevance:** A marketplace could improve transparency by clearly labeling the pricing model and offering an estimated total-cost calculator based on guest count and the selected services.

### 3. Bivariate analysis: Capacity versus rental cost

**Business question:** Do higher-capacity venues tend to have higher rental costs?

The project reported a correlation of **0.64 across 118 venues** between venue capacity and rental cost.

**Interpretation:** This indicates a moderately strong positive linear association in the analyzed data: venues with greater capacity tended to have higher rental costs. Correlation does not prove that capacity alone causes a higher price.

The analysis also reported median rental cost per guest values of:

- Banquet halls: **₹90**
- Farmhouses: **₹294**

These comparisons depend on how capacity and rental cost were defined in the notebook. A derived cost-per-guest measure is an indicative comparison, not necessarily the actual amount each attendee would cost, especially when catering and other services are excluded.

**Business relevance:** Customers can use guest capacity and indicative cost metrics as initial screening criteria. Venue owners can use comparable listings to inform their pricing review.

### 4. Area-level pricing comparison

**Business question:** How do observed rental costs vary by area?

Reported median rental costs:

| Area | Median rental cost |
|---|---:|
| Gandipet | ₹9,25,000 |
| Kompally | ₹5,00,000 |
| Shamshabad | ₹3,50,000 |
| Secunderabad | ₹50,000 |
| Moinabad | ₹55,000 |

**Interpretation:** The observed medians vary substantially across these areas. However, the project noted that the number of priced venues in each area was small—approximately **3–12 venues** for the comparisons shown.

**Caution:** These values should be treated as indicative observations, not as definitive rankings of area-wide market prices. Venue type, capacity, service inclusions, and the small group sizes may explain some of the differences.

**Business relevance:** Customers may benefit from comparing nearby areas rather than limiting their search to one locality. Venue owners can consider comparable listings in their area while accounting for differences in venue characteristics.

### 5. Multivariate analysis: Venue type and capacity group

**Business question:** How does observed rental pricing vary when venue type and guest-capacity category are considered together?

Reported examples:

- Convention halls in the **1,001–3,000 guest** capacity group had the highest reported median rental cost: **₹4,90,000**.
- Hotels in the **up-to-500 guest** capacity group had the lowest reported median rental cost: **₹25,000**.

**Interpretation:** Pricing patterns may differ across combinations of venue type and capacity. However, small group sizes can make medians unstable and limit generalization.

**Business relevance:** Comparing venues with similar capacity and type is likely to be more informative than comparing all venues as one group.

---

## Hypothesis Testing

The project used One-Way ANOVA for two questions about rental costs.

### What One-Way ANOVA tests

One-Way ANOVA tests whether the population means of a numerical outcome are equal across three or more independent groups defined by one categorical factor.

The null hypothesis is:

**H₀:** All group population means are equal.

The alternative hypothesis is:

**H₁:** At least one group population mean differs.

The significance threshold used in the project interpretation was **α = 0.05**. When the p-value is below this threshold, the null hypothesis is rejected. A small p-value is evidence against the equal-means null hypothesis; it is not the probability that the null hypothesis is true.

### Test 1: Rental cost by venue type

- **Independent/grouping variable:** Venue type (`venue_group`)
- **Dependent variable:** Rental cost (`rental_cost`)
- **Test:** One-Way ANOVA
- **Reported F-statistic:** 9.53
- **Reported p-value:** 0.00000137

**Decision:** Since the reported p-value is below 0.05, reject H₀.

**Conclusion:** There is statistical evidence that mean rental cost is not the same across all venue-type groups included in the test.

**What this test does not establish:** It does not identify exactly which venue types differ from one another. A suitable post-hoc procedure, such as Tukey's HSD when its assumptions are appropriate, would be needed to examine pairwise differences.

### Test 2: Rental cost by capacity group

- **Independent/grouping variable:** Guest-capacity category (`capacity_group`)
- **Dependent variable:** Rental cost (`rental_cost`)
- **Test:** One-Way ANOVA
- **Reported F-statistic:** 25.26
- **Reported p-value:** 1.32 × 10⁻¹²

**Decision:** Since the reported p-value is below 0.05, reject H₀.

**Conclusion:** There is statistical evidence that mean rental cost differs across at least some of the capacity groups included in the test.

Again, this overall result does not identify which particular groups differ. Pairwise follow-up testing would be needed.

### Why One-Way ANOVA was selected

Each test examined one categorical factor at a time:

1. Venue type → rental cost.
2. Capacity group → rental cost.

A two-sample t-test is designed for comparing two means, while a chi-square test is used for associations between categorical variables. Correlation measures the relationship between numerical variables and does not answer the same multi-group mean-comparison question.

A Two-Way ANOVA could be considered if the goal were to model venue type and capacity group simultaneously and examine their effects and potential interaction. It was not the question answered by the two separate One-Way ANOVA tests.

### Assumptions and interpretation cautions

Standard One-Way ANOVA generally assumes:

- Independent observations.
- Approximately normal residuals within groups.
- Reasonably similar group variances.

Rental costs can be strongly skewed and may contain extreme values. The project results should therefore be interpreted alongside diagnostic checks. If variance assumptions are substantially violated, Welch's ANOVA may be a useful alternative; if the research question concerns distributions or ranks rather than means, a Kruskal–Wallis test may be considered. The correct alternative depends on the data and analytical objective.

ANOVA tests **means**, whereas several EDA charts in this project describe **medians**. Those are different summaries and should not be treated as interchangeable.

---

## Key Findings

1. **Rental prices vary widely.** The reported median was ₹94,000 per function, while the observed range extended from ₹3,500 to ₹35 lakh.
2. **Pricing is not presented consistently.** Listings may show per-plate pricing, fixed rental, both, or no public price.
3. **Capacity and rental cost were positively associated.** The reported correlation was 0.64 across 118 analyzed venues, though this does not establish causation.
4. **Observed prices varied across areas.** Small numbers of priced venues in some areas limit how confidently these differences can be generalized.
5. **Pricing patterns varied across venue-type and capacity groups.** The reported ANOVA results provided evidence against equal mean rental costs across the tested groups, subject to the test assumptions.
6. **Listing completeness matters.** Missing prices and inconsistent price formats make comparisons and customer decisions more difficult.

---

## Business Recommendations

### For customers

- Filter venues by guest capacity, area, venue type, and budget.
- Compare like with like: fixed rental fees should not be directly compared with per-plate prices.
- Confirm what the advertised price includes, including catering and other services.
- Treat area-level medians and cost-per-guest measures as initial indicators, not guaranteed final costs.

### For venue owners

- Display pricing models clearly and explain what is included.
- Keep venue capacity, room availability, location, and amenities up to date.
- Benchmark against venues with similar capacity and type rather than relying only on broad area averages.
- Use customer feedback and listing completeness as inputs for improving the venue's marketplace presentation.

### For the marketplace

- Standardize pricing labels and distinguish “price on request” from missing or invalid data.
- Introduce a total-cost estimate that can account for guest count, per-plate charges, rental fees, and selected services.
- Add data-quality checks for missing, inconsistent, or implausible values.
- Improve filters for capacity, area, venue type, rooms, and pricing model.
- Track changes in advertised prices over time, where permitted, to provide more current comparisons.

---

## Tools and Technologies

| Tool / technology | Role in the project |
|---|---|
| Python | General-purpose language used to implement the workflow. |
| Requests | Retrieve HTML responses from listing pages. |
| BeautifulSoup | Parse HTML and extract relevant listing information. |
| Regular expressions (`re`) | Help interpret or extract patterns from text. |
| Pandas | Store, clean, transform, group, and analyze tabular data; export CSV files. |
| NumPy | Support numerical calculations where used in the notebook. |
| Matplotlib | Create data visualizations. |
| Seaborn | Create statistical visualizations, where used. |
| Statistical testing | One-Way ANOVA to compare mean rental costs across selected groups. |

Only list a library as an implemented dependency if it is present in the executed notebook or script.

---

## Project Workflow

```text
Define business problem
        ↓
Identify WedMeGood Hyderabad venue listings
        ↓
Retrieve listing pages
        ↓
Parse listing information from HTML
        ↓
Build structured dataset and save CSV
        ↓
Clean fields and interpret pricing formats
        ↓
Inspect missing values and data quality
        ↓
Perform univariate and bivariate EDA
        ↓
Compare pricing by area, venue type, and capacity
        ↓
Run selected One-Way ANOVA tests
        ↓
Interpret findings with limitations
        ↓
Develop business recommendations
```

---

## How to Run the Project

The exact filenames and notebook entry point depend on how the repository is organized. The following is a suggested workflow for a local setup.

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd <your-repository-folder>
```

Replace the placeholders with the actual repository URL and folder name.

### 2. Create and activate a virtual environment (recommended)

On Windows:

```bash
python -m venv .venv
.venv\Scripts\activate
```

### 3. Install the dependencies

If a `requirements.txt` file is provided:

```bash
pip install -r requirements.txt
```

If no dependency file exists, install only the packages imported by the project notebook/script. A possible starting set, subject to the actual code, is:

```bash
pip install requests beautifulsoup4 pandas numpy matplotlib seaborn scipy jupyter
```

### 4. Run the notebook

```bash
jupyter notebook
```

Open the project's notebook and execute the cells in order. If a prepared CSV is included, use it to reproduce the analysis without repeating web requests.

### 5. Respect website rules

Before rerunning the scraper, review the website's applicable terms, access restrictions, and robots guidance. Use reasonable request rates, avoid bypassing access controls, and do not collect personal or sensitive information. A website's structure or permissions may change after the original collection.

> This README does not assume the exact notebook filename, CSV filename, or repository layout. Update those details after arranging the project files in the repository.

---

This is a suggested structure, not a claim about the files currently in the project. Use filenames that match the actual artifacts, and avoid committing large or sensitive data unnecessarily. If redistribution of scraped data is not permitted, publish the code and methodology instead of the raw records.

---

## Limitations and Considerations

- **Coverage:** The analysis represents listings captured from the documented 40 result pages, not necessarily all venues in Hyderabad.
- **Time sensitivity:** Prices, ratings, review counts, and availability can change after collection.
- **Missing information:** Some listings did not provide all pricing or venue details.
- **Price comparability:** Per-plate prices, fixed rental fees, and bundled offerings measure different things.
- **Small groups:** Some area and venue-category comparisons included only a small number of priced venues.
- **Selection bias:** Listed venues and venues with available prices may not represent the entire local market.
- **Association versus causation:** A correlation between capacity and rental cost does not prove that capacity alone determines price.
- **Statistical assumptions:** ANOVA conclusions depend on assumptions such as independence and appropriate residual/variance behavior.
- **Reported results:** Numerical findings in this README reflect the values documented for the project; they should be reproduced from the final dataset and notebook before being treated as independently verified.
- **Scraping reliability:** HTML structure, access rules, and page content may change, requiring the collection code to be reviewed.

---

## Future Enhancements

1. **Improve data validation:** Add checks for impossible prices, malformed capacity ranges, duplicate listings, and inconsistent units.
2. **Improve pricing comparability:** Distinguish rental fees, vegetarian/non-vegetarian plate prices, and bundled offerings throughout the analysis.
3. **Add robust statistical diagnostics:** Inspect distributions, outliers, residuals, and group variances; consider Welch's ANOVA or non-parametric methods where appropriate.
4. **Run post-hoc tests:** If ANOVA is significant, identify which groups differ while accounting for multiple comparisons.
5. **Expand data coverage responsibly:** Collect additional pages or locations only when permitted and document the collection date and scope.
6. **Track prices over time:** Build a repeatable, policy-compliant process to compare listing changes across collection dates.
7. **Create an interactive Power BI dashboard:** Allow users to filter by area, venue type, capacity, pricing model, and available amenities.
8. **Develop better customer comparisons:** Estimate an event budget using guest count, rental cost, food prices, and explicitly stated service inclusions.
9. **Add uncertainty to comparisons:** Show group sample sizes and, where appropriate, confidence intervals so users can distinguish robust patterns from comparisons based on very few listings.

---

## Conclusion

This project demonstrates an end-to-end analytics workflow using publicly displayed wedding venue listings: collecting web data, structuring and cleaning the information, exploring pricing patterns, comparing groups, applying hypothesis testing, and converting findings into business recommendations.

The main value is not simply identifying the most expensive or least expensive venue. It is understanding why venue comparisons are difficult, which attributes appear associated with price, where the data is incomplete, and how clearer pricing and better marketplace tools could support more informed decisions.

The findings should be interpreted within the scope and limitations of the collected listings. Additional data, diagnostic checks, and follow-up tests would strengthen the conclusions and support more reliable comparisons.

---

## Author

**Reshma Sappa**  
**Project:** Wedding Venue Marketplace Analytics — Web Scraping, EDA & Business Insights  
**Focus areas:** Python-based web scraping, data cleaning, exploratory data analysis, statistical hypothesis testing, and business recommendations.

