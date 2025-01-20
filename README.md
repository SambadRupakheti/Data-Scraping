# Data-Scraping

# PubMed Scraper

This script scrapes article details such as titles, authors, and journals from PubMed and saves them into a CSV file. Used for educational purpose only

---

# Code Explanation

# Import necessary libraries
# `requests` is used to send HTTP requests to fetch the webpage.
# `BeautifulSoup` helps parse and extract specific data from the HTML.
# `csv` is used to save the extracted data into a file.
import requests
from bs4 import BeautifulSoup
import csv

# Define the PubMed URL with a specific search query (e.g., "machine learning").
# You can change the `term` parameter in the URL to search for different topics.
url = "https://pubmed.ncbi.nlm.nih.gov/?term=machine+learning"

# Send a GET request to the PubMed website to retrieve the page content.
response = requests.get(url)

# Parse the HTML content of the page using BeautifulSoup.
# This converts the raw HTML into a format that allows us to search for specific elements.
soup = BeautifulSoup(response.content, "html.parser")

# Find all the articles on the page.
# Each article is represented by an <article> tag with the class "full-docsum".
articles = soup.find_all("article", class_="full-docsum")

# Open a CSV file to save the scraped data.
# We use "w" mode for writing, `newline=""` to avoid extra lines, and `encoding="utf-8"` for proper character handling.
file = open("pubmed_scraped_articles.csv", "w", newline="", encoding="utf-8")
writer = csv.writer(file)

# Write the headers (column names) to the CSV file.
writer.writerow(["Title", "Authors", "Journal"])

# Loop through each article container to extract the required details.
for paper in articles:
    # Extract the title from the <a> tag with the class "docsum-title".
    # If the tag is not found, assign "N/A" to handle missing data.
    title_tag = paper.find("a", class_="docsum-title")
    title = title_tag.text.strip() if title_tag else "N/A"

    # Extract the authors from the <span> tag with the class "docsum-authors".
    # Again, use "N/A" for missing data.
    authors_tag = paper.find("span", class_="docsum-authors")
    authors = authors_tag.text.strip() if authors_tag else "N/A"

    # Extract the journal details from the <span> tag with the class "docsum-journal-citation".
    # Handle missing data by assigning "N/A".
    journal_tag = paper.find("span", class_="docsum-journal-citation")
    journal = journal_tag.text.strip() if journal_tag else "N/A"

    # Print the extracted details to the console for debugging.
    print("Title:", title)
    print("Authors:", authors)
    print("Journal:", journal)
    print("-" * 50)

    # Write the extracted details as a row in the CSV file.
    writer.writerow([title, authors, journal])

# Close the CSV file after writing all the data.
file.close()

# Print a message to indicate the script has completed and the data is saved.
print("Data saved to pubmed_scraped_articles.csv")
