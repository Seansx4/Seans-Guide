---
title: Web Scraping Google Search Results
metaDescription: "In this article I focus on web scraping the Google Search
  Results Page for a search term of interest. This simple approach utilises
  Python and Beautiful Soup and may be used for tracking the position of a page
  or domain in Google for a given search term. "
datum: Jun 26th 22
blogTitle: Web Scraping Google Search Results
description: "In this article I focus on web scraping the Google Search Results
  Page for a search term of interest. This simple approach utilises Python and
  Beautiful Soup and may be used for tracking the position of a page or domain
  in Google for a given search term. "
author: Sean Grayson
tags:
  - post
  - coding
JSON: >-
  

  \

  { "@context" : "http://schema.org", "@type" : "Article", "name" : "Web Scraping Google Search Results", "author" : { "@type" : "Person", "name" : "Sean Grayson" }, "datePublished" : "2022-06-26", "image" : "", "url" : "" }
paragraph1: >-
  <h2>Step 1: Search Query, Location and Number of Results</h2>




  <p>The first step to web scraping the Google Search Results Page is to ensure we use the correct URL structure. The URL structure of Google Search looks like the following: “https://www.google.com/search?q=”plus our search term, with spaces replaced by “+”. So, for the search of “garden hose” the URL would look like “https://www.google.com/search?q=garden+hose”. </p>


  <p>Next, we must determine the location we want to see the results for. This is achieved by adding a "location" parameter to our URL after our search query. You can see in the screenshot below I have added “&gl=us” to the end of the URL after I search for the term “garden hose” to set the results to that of Google US.</p>
image1: /images/gardenhose.png
imageAlt1: Google Search Results Page
subheading2: Google Search Results for "garden hose". Note the location is set
  to USA by using the parameter "&gl=us".
paragraph2: <p>Next, we must decide how many results we want to scrape. Once
  again, this can be set by adding a parameter to the end of the URL. In the
  image below I have added the parameter “&num=50” to ensure that the top 50
  results are returned. </p>
image2: /images/num50.png
imageAlt2: Google Search Results
caption2: Google Search Results Page with 50 results instead of the default 10.
  This is achieved by adding the parameter "&num=50".
paragraph3: >-
  <p>Now that we have the desired output for our search term it’s time to work
  with cookies. </p>




  <h2>Step 2: Handling Cookies</h2>


  <p>When we run a Google Search query through Python our program will run into an issue with cookies. To bypass this issue, we give our Python program cookies so that Google thinks it is a real user. To find the cookies we need to use we do a quick manual Google Search. On the Google Results Page we can then go Inspect --> Application --> Cookies. Click the cookies for Google and copy the cookie labelled “CONSENT”. This is the cookie we will use in our Python program as a parameter at the time of our request. </p>
image3: /images/cookies.png
imageAlt3: Devloper Tools on the Google Results
caption3: Using Developer Tools we can retrieve the cookies we require from the
  Application tab.
paragraph4: >-
  <p>In the code below we pass our search URL and our Consent cookie to requests
  and use BeautifulSoup to parse the HTML of the response. </p>


  <pre class="code_terminal">


  <code>

  from bs4 import BeautifulSoup

  import requests


  urls = ["https://www.google.com/search?q=garden+hose&gl=us&num=50"


  cookies = {
      "CONSENT": "YES+cb.20220301-11-p0.en+FX+104"
  }


  def scrape(url):
      # r = requests.get(url, headers=headers, cookies=cookies)
      r = requests.get(url, cookies=cookies)
      webpage = BeautifulSoup(r.text, "html.parser")

  print(webpage)


  if name ==  "main":
      for url in urls:
          scrape(url)

  </code>


  </pre>




  <h2>Step 3: Inspecting Returned Page, Not our Browser</h2>


  <p>This next step is where a lot of people run into problems scraping Google. Often the page you inspect after conducting a Google Search is not the same HTML which is returned by requests. This means that when you try to target a class in the returned HTML with beautiful soup, it may not exist, and so the program does not return any results. To get around this problem we simply print the HTML response. </p>
image4: /images/printing-response.png
imageAlt4: Screenshot of code terminal
caption4: Printing out the HTML response so we can determine the classes we need
  to target in our scraping program.
paragraph5: <p>We can then copy this HTML from the terminal and paste it into an
  editor where we can inspect the code for the attributes we are hoping to
  target. I like to use a live editor such as <a
  href="https://htmledit.squarefree.com/" target="_blank">HTML Square Free</a>
  for this task which renders a live view of the HTML code. We can then open
  developer tools and look for the classes we need. In the below example we can
  see that each Google result is enclosed in <div class="egMi0 kCrYT">. </p>
image5: /images/inspecting-response.png
imageAlt5: Inspecting the HTML
caption5: "Pasting the HTML response into a live server allows us to inspect the
  results page for the classes we need to target.  "
paragraph6: >-
  <p>This means we can target this <div> element to retrieve the URL and Title
  of the result. The below code does this:</p>\

  \

  <pre class="code_terminal">


  <code>

  from bs4 import BeautifulSoup

  import requests


  urls = ["https://www.google.com/search?q=garden+hose&gl=us&num=50"


  cookies = {
      "CONSENT": "YES+cb.20220301-11-p0.en+FX+104"
  }


  def scrape(url):
      # r = requests.get(url, headers=headers, cookies=cookies)
      r = requests.get(url, cookies=cookies)
      webpage = BeautifulSoup(r.text, "html.parser")

  if name ==  "main":
      for url in urls:
          scrape(url)

  </code>


  </pre>




  <p>We can now copy and paste this code to Excel to tidy up.</p>


  <h2>Step 4: Using Excel to Highlight Domains of Interest</h2>\

  <p>After pasting the data into Excel we can now highlight the column and do a simple find and replace to remove the unnecessary “/url?q=” at the start of each URL.</p>
image6: /images/find-and-replace.png
imageAlt6: Excel screenshot
caption6: In Excel we can use "Find and Replace" to clean up our results.
paragraph7: <p>In the Data tab we can now select Text to Columns --> delimited
  --> other and use the pipe symbol as a delimiter. Press finish and this should
  separate out our URLs and Titles in separate columns for analysis.</p>
image7: /images/text-to-columns.png
imageAlt7: Text to columns to clean data
caption7: Using "Text to Columns" we can split our data so that we have both our
  URLs and Titles in individual columns.
paragraph8: <p>Using conditional formating we can now highlight any domains of
  interest. To do this on the Home tab click Condtional Formating --> Highlight
  Cell Rules --> Text that contains and enter the domain you are interested in
  along with a colour.</p>
image8: /images/conditional-formatting.png
imageAlt8: Conditional Formatting in Excel
caption8: Using Conditional Formatting in Excel we can highlight domains of
  interest so it is easier to track results.
paragraph9: >-
  <p>Lets say we own the domain “yourbestdigs.com” and want to track the ranking
  of a landing page, plus the ranking of a competitor domain
  “gardenerspath.com”where they rank. We could colour our domain green and our
  competitor domain red.</p>\

  \

  <p>Next we can add some headers and positions to our data. First highlight column A1  right click  insert column. Now select cell B2, navigate to the View tab and select freeze panes. This will freeze the top row and the first column so as we fill up our Excel file the data will be easy to visualise as we scroll. </p>
image9: /images/freezing-panes.png
imageAlt9: Freezing panes
caption9: We can use Excel to "Freeze Panes". This means that the top row and
  first column will be locked in position when we scroll, we makes it easier to
  visualise data as we fill up the Excel file.
paragraph10: <p>We can now fil the numbers down the first column and the date
  across the top row.</p>
image10: /images/formatted.png
imageAlt10: Excel file correctly formatted
caption10: Adding numbers in the first column denotes the position of the result
  in Google. We can then track daily changes by adding date to the first row.
paragraph11: <p>Now if we were to rerun the program over a few days we may
  visualise the position jumps of our domains of interest. </p>
image11: /images/changes.png
imageAlt11: Daily changes
caption11: If we rerun the program over a time period we cant rack the changes
  in Google Results. In this example we can see our domain has climbed 3
  positions while our competitor has dropped 1.
paragraph12: >-
  <h2>Full Code</h2>


  <pre class="code_terminal">


  <code>

  from bs4 import BeautifulSoup

  import requests


  urls = ["https://www.google.com/search?q=search+term +here&gl=location here&num=number of results here"


  cookies = {
      "CONSENT": "Your cookie here"
  }


  def scrape(url):
      # r = requests.get(url, headers=headers, cookies=cookies)
      r = requests.get(url, cookies=cookies)
      webpage = BeautifulSoup(r.text, "html.parser")

  if name ==  "main":
      for url in urls:
          scrape(url)

  </code>


  </pre>
---
