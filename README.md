# Amazon Sales Analysis

R analysis of Amazon product listings that asks how customer ratings vary by category and whether ratings relate to sales volume.

The report lives in [`proj2.Rmd`](proj2.Rmd). Rating count is used as a proxy for sales because the source data does not include units sold.

## Research question

How do customer ratings differ among Amazon product categories, and is there a correlation between those ratings and sales volume?

The working hypothesis is that satisfaction is shaped by more than one factor (including price and discounting) and that those effects differ by category.

## Dataset

[`amazon.csv`](amazon.csv) is a public Amazon sales extract from Kaggle ([Karkavelraja J., 2023](https://www.kaggle.com/datasets/karkavelrajaj/amazon-sales-dataset/data)). It has **1,465** product rows and these fields:

| Column | Description |
| --- | --- |
| `product_id` | Unique product identifier |
| `product_name` | Product title |
| `category` | Amazon category path (`Department\|...\|Leaf`) |
| `discounted_price` | Listed sale price |
| `actual_price` | Original price |
| `discount_percentage` | Discount from the original price |
| `rating` | Average customer rating |
| `rating_count` | Number of ratings (used as a sales proxy) |
| `about_product` | Product description |
| `review_title`, `review_content` | Sample review text |
| `img_link`, `product_link` | Product image and listing URLs |

Top-level categories in this file:

- Electronics (526)
- Computers & Accessories (453)
- Home & Kitchen (448)
- Office Products (31)
- Smaller groups: Musical Instruments, Home Improvement, Toys & Games, Car & Motorbike, Health & Personal Care

## Analysis

The notebook:

1. Collapses the nested `category` path to the top-level department.
2. Sums `rating_count` by category as a stand-in for sales volume.
3. Compares mean ratings and rating distributions across categories.
4. Plots rating vs. rating count (linear and log-scaled) with a fitted trend.
5. Tests Pearson correlation between average rating and rating count.
6. Summarizes the overall rating distribution.

### Main findings

- **Computers & Accessories**, **Electronics**, and **Home & Kitchen** account for most of the rating volume.
- Mean ratings are fairly stable across categories. High review volume does not imply lower average quality.
- Categories with more ratings also have wider rating spreads.
- Pearson correlation between rating and rating count is about **0.10** (95% CI roughly 0.05–0.15, p < 0.05): a statistically significant but weak positive association.
- Ratings alone are a weak predictor of sales. Visibility, pricing, and marketing likely matter more.

See the Discussion section in `proj2.Rmd` for limitations (review bias, Amazon-only sample, no true sales counts) and follow-up ideas.

## How to run

You need R and the packages used in the notebook:

```r
install.packages(c(
  "tidyverse",
  "lubridate",
  "readr",
  "kableExtra",
  "broman",
  "knitr",
  "rmarkdown"
))
```

From the repository root:

```bash
Rscript -e "rmarkdown::render('proj2.Rmd')"
```

That writes `proj2.html`. Keep `amazon.csv` in the same directory as the R Markdown file.

## Repository layout

```
.
├── README.md      # this file
├── amazon.csv     # product listings and reviews
└── proj2.Rmd      # analysis report
```

## Citation

Karkavelraja J. (2023). *Amazon Sales Dataset*. Kaggle. https://www.kaggle.com/datasets/karkavelrajaj/amazon-sales-dataset/data
