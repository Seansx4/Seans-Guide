---
title: 6 Easy Steps to Scrape a Two-Factor Authentication Website
metaDescription: This blog outlines how to correctly web scrape a dynamic
  website protected with two factor authentication using Python and Selenium.
datum: Jun 23rd 23
blogTitle: 6 Easy Steps to Scrape a 2FA Website
author: Sean Grayson
tags:
  - post
sections:
  - type: Paragraph
    content: Web scraping is a fantastic tool for any programmer to add to their
      skill set. Web scraping allows you to automate data retrieval from
      different websites. For example, you could scrape a list of email
      addresses from one website, the price of products from another, or even
      the Google Search results page. The possibilities are endless, and the
      process is both helpful and fun to set up. While this example will
      undoubtedly be different to your problem, changing some lines of code or
      following the general methodology may help.
  - type: Heading
    content: Why Scrape a Website with 2FA-Factor Authentication?
  - type: Paragraph
    content: The need to scrape a website with two-factor authentication (2FA) is
      quite rare. 2FA provides an extra layer of security to a user account of a
      given platform. When a user attempts to login to the platform a once off
      code is often sent to the email address associated with the account.
      Typically, most data the user can access on their dashboard can be
      exported, or there is usually an API for data retrieval. If neither of
      these options exist however, webscraping can allow you to retrieve the
      data you need, you just have the small task of bypassing 2FA.  When
      attempting to scrape a 2FA protected website myself, I found various
      tutorials outlining how to login to a regular website, but very sparse
      information on how to circumnavigate 2FA. I should make it clear that this
      tutorial is for someone who is the true owner of an account protected with
      2FA and not a guide on how to hack someone else’s account.  I like to keep
      my blogs focused on the topic of discussion, so I won’t be going through
      the technicalities of how to install the required dependencies (Selenium
      and the WebDriver) in this blog, or any basic concepts of Python
      programming. In the future I am going to write some content surrounding
      this but for now I will leave it up to you to do some quick research (I
      recommend YouTube).
  - type: Heading
    content: Static vs Dynamic Websites?
  - type: Paragraph
    content: "There are two possible options to take when trying to overcomethe
      scraping a 2FA website, but which option you chose depends largely on what
      kind of website you are trying to scrape. If you are unsure of what kind
      of website you are scraping, check out A Guide: Static Vs. Dynamic
      Websites  If your site is a static website you can simply use the
      beautifulsoup library in conjunction with a neat little trick to store
      your cookies (I will be writing some content on this soon). This allows
      you to bypass login and TFA and scrape the data as required. If you are
      scraping a dynamic website however, thing can be a little more
      complicated."
  - type: Heading
    content: Scraping a Dynamic Website with Selenium
  - type: Paragraph
    content: Below I have outlined how to use Selenium to scrape the user dashboard
      of a BigCommerce website. Selenium allows for the automation of a
      webdriver, thereby simulating a web browsers interaction with a web page.
      BigCommerce is a popular eCommerce provider. The user dashboard for a
      BigCommerce website is dynamically generated and protected by 2FA at
      login.  For this example, we need to fetch the URL for each of our content
      pages (this was my original problem as BigCommerce does not allow the
      export of your content page URLs). Below is the list of our content pages
      once we have logged into our dashboard.
  - type: Image
    content: https://www.seansguide.com/images/contentPage.png
---
