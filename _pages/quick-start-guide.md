---
layout: archive
title: "Not-so-quick-start Guide"
permalink: /quick-start-guide/
author_profile: false
---
&nbsp;
# Project by Categories
==========================

## Scraping

- Focuses on the technique to obtain or "scrape" data that are located anywhere on the world wide web
- The data can be both "structured" or "unstructured"
- Strutured data are data that are stored in the form of traditional row-column database
- Unstructured data on the other hand are basically everything else than the traditional row-column database. This can include - but not   limited to - presentation slides, movie ratings, a picture on a website, or a [report of a pandemic on WHO website](https://www.who.int/emergencies/diseases/novel-coronavirus-2019/events-as-they-happen)
- Compared to structured data, unstructured are a bit more complicated, either to scrape them, or to clean them

[Scraping]({{ "/scraping/" | relative_url }}){: .btn .btn--success .btn--large}

## Wrangling

- Focuses on the technique of transforming of messy or un-tidy data into "tidy" data, which is a term revolutionized by [Hadley Wickham](http://hadley.nz) in his popular [journal article](https://vita.had.co.nz/papers/tidy-data.pdf)
- Messy data can be both structured and unstructured data
- Tidy data are a _lot_ more easier to analysis, as it has the formal structure where;
1. Each variable forms a column
2. Each observation forms a row
3. Each type of observational unit forms a table

[Wrangling]({{ "/wrangling/" | relative_url }}){: .btn .btn--success .btn--large}

## Exploratory Data Analysis (EDA)

- Focuses on the technique of exploring the data to answer questions and visualizing the results
- EDA allows us to validate data and unravel the relationships between variables
- It is common to use summary statistics and regression models in EDA

[Exploratory Data Analysis]({{ "/eda/" | relative_url }}){: .btn .btn--success .btn--large}

## Machine Learning

- Focuses on the technique of "supervised" and "unsupervised" learning
- Supervised learning is done when we expect an output from a particular dataset, say predicting how many people will buy a particular service or product
- Unsupervised learning on the other hand focuses on the _natural structure_ of the dataset, say given a large dataset of people, can we categorize them into such and such clusters depending on their natural characteristics?

[Machine Learning]({{ "/machine_learning/" | relative_url }}){: .btn .btn--success .btn--large}

## Visualizations

- Focuses on the technique of visualizing the findings from the data in order to communicate the findings clearly and efficiently
- The audience can be both from those who are well-versed in the field of data science and those who are not familiar with the field
- Beautiful are not the _main_ goal in visualizations; it is comprehensibility and accuracy. Beautiful is a plus

## Express Projects

- A category for projects that are too simple to be broken down into the above categories
- Each project are completed in a single post

[Express Projects]({{ "/express_projects/" | relative_url }}){: .btn .btn--success .btn--large}
&nbsp;
# Project Naming Convention
=================================

- Each project will have a name that comprises of three(3) parts
- First part consist of the actual name of the dataset that are utilized
- Second part consist of the name of the project's category. It is separated from the first part with "-"
- Third part consist of the name of the programming language that are utilized. It is enclosed in brackets
- For example, consider a project titled "Parkinson's Disease - Machine Learning (RStudio)". "Parkinson's Disease" is the first part, "Machine Learning" is the second part, and "(RStudio)" is the name of the programming language
- Keep the naming convention in mind as you browse the website. It makes navigating the projects a _lot_ easier
