---
title: Web Scraping a Two-Factor Authentication Website
metaDescription: This blog outlines how to correctly web scrape a dynamic
  website protected with 2FA using Python and Selenium.
datum: May 24th 22
blogTitle: Web Scraping Content from Two-Factor Authentication Website
description: This blog outlines how to correctly web scrape a dynamic website
  protected with 2FA using Python and Selenium.
author: Sean
tags:
  - post
  - coding
paragraph1: >-
  <p>Web scraping is a fantastic tool for any programmer to add to their
  arsenal. Web scraping allows you to automate data retrieval from webpages. For
  example, you could scrape a list of email addresses from a website, the price
  of products from another, or even the Google results page for specific search
  queries. The possibilities are endless, and process is both helpful and fun to
  set up. Win Win.  </p>




  <p>I learned to web scrape from hours of watching YouTube videos, reading blogs and tinkering with different lines of code, so I know first-hand that it is easy to be overwhelmed when you first look into web scraping. The first obstacle you will encounter is the numerous considerations to take into account. These range from which programming language to use, what kind of website you are scraping, to what libraries are available. I personally use Python for my web scraping antics and as such is what I outline in the demo below.</p>
image1: /images/code.jpg
imageAlt1: Python code for web scraping
subheading2: Overview
paragraph2: >-
  <p>In this blog I am going to outline how to scrape a website with two-factor
  authentication. Two-factor authentication (2FA) provides extra security to a
  user account. When a user attempts to login to a platform protected with 2FA,
  a once off passcode is often sent to the email address associated with the
  account. </p>


  <p>When attempting to scrape a 2FA protected website myself, I found various tutorials outlining how to login to a regular website, but very sparse information on how to circumnavigate 2FA. I should make it clear that this tutorial is for someone who is the true owner of an account protected with 2FA and not a guide on how to hack someone else’s account. </p>


  <p>I like to keep my blogs focused on the topic of discussion, so I won’t be going through the technicalities of how to install the required dependencies (Selenium and the WebDriver) in this blog, or any basic concepts of Python programming. In the future I am going to write some content surrounding this but for now I will leave it up to you to do some quick research (I recommend YouTube). </p>




  <p>There are two possible options to take when trying to overcome this problem, but which option you chose depends largely on what kind of website you are trying to scrape. If you are unsure of what kind of website you are scraping, check out my guide Static vs Dynamic Sites: What’s the Difference? </p>


  <p>If your site is a static website you can simply use the beautifulsoup library in conjunction with a neat little trick to store your cookies (link). This allows you to bypass login and TFA and scrape the data as required. If you are scraping a dynamic website however, thing can be a little more complicated. </p>




  <p>Below I have outlined how to use Selenium to scrape the user dashboard of a BigCommerce website. BigCommerce is a popular ecommerce provider. The user dashboard for a BigCommerce website is dynamically generated and protected by 2FA at login. While this example may be different to the problem you are trying to solve, the solution should still function. </p>


  <p>For this example, we need to fetch the URL for each of our web pages (this was my original problem as BigCommerce does not allow the export of your webpage URLs). Below is the list of our webpages once we have logged into our dashboard.</p>
image2: /images/content-page.png
imageAlt2: Content page with URLs
caption2: BigCommerce page with the list of URLs
---
