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
subheading2: ""
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
image2: /images/contentPage.png
imageAlt2: Content page with URLs
caption2: BigCommerce page with the list of URLs
subheading3: ""
paragraph3: >
  <p>While we can see what looks like links to our webpages, if we right click
  and press inspect on any given link we can see that it is actually a link to
  another page. </p>
image3: /images/inspectingContentpage.png
imageAlt3: Inspecting URLs
caption3: Inspecting URLss
subheading4: ""
paragraph4: <p>This next page acts as an editorial page for the webpage, and it
  is here that the webpage URL is stored.</p>
image4: /images/editorialPage.png
imageAlt4: Editorial page
caption4: Editorial page, note you can see the correct web page URL.
subheading5: ""
paragraph5: >-
  <p>This means our task is split into two stages. First, we must scrape the
  list of editorial URLs on the dashboard, before visiting each URL individually
  and scraping the required webpage URL. </p>




  <p>The first step of our web scraping journey is to attempt to access our BigCommerce dashboard. To do this we must login and pass our one-time 2FA code for login. To simulate the process, we are going to login to the dashboard from an incognito browser. This ensures we follow the same route our program will, and that we are not automatically logged in.</p>
image5: /images/login.png
imageAlt5: Login
subheading6: ""
paragraph6: <p>By inspecting the email field, we can see the code for the input.
  In this example we can see that the input field has name = “user[email]” and
  id = “user_email”. We can use either of these for our function but in this
  case I am going to use the name attribute for the sake of consistency, as the
  login button has a name but not an ID. </p>
image6: /images/inspectingemailinput.png
imageAlt6: Inspecting email input
caption6: Using developer tools we inspect the email input field. We then repeat
  this for the password field and the login button to find their names.
subheading7: ""
paragraph7: >-
  <p>We then repeat this process for the password.And again for the login
  button. </p>


  <p>From this we can see that the password has a name=“user[password]” and the login button has a name=“commit”.</p>

  <p>After logging in manually you will be redirected towards a new page for the 2FA. On this page carry out the same inspection process to find the name of the code field and the verify button.</p>
image7: /images/inspectingverify.png
imageAlt7: Inspecting the verify option
subheading8: ""
paragraph8: "<p>In this instance they are name=“verification[opt_code]” and
  name=“commit”.</p>

  <p>With this data we can now begin to write our python program. Below are
  the lines of code for the dependencies you will need:</p>


  <pre class=\"code_terminal\">\r

  \          <code>\r

  from selenium import webdriver\r

  from selenium.webdriver.common.by import By\r

  from selenium.webdriver.chrome.options import Options\r

  from selenium.webdriver.chrome.service import Service\r

  import time\r

  \r

  \          </code>\r

  \        </pre>


  <p>Next we initialise our chrome driver for the actual scraping. This is
  done by storing the browser driver file path in a variable and then using that
  variable to instantiate a driver object. The chrome service is to avoid
  depreciation in future releases. If you don’t understand just trust me and
  follow the guide.</p>


  <pre class=\"code_terminal\">\r

  \          <code>

  chrome_options = Options()\r

  \r

  path = Service(\"C:\\Program Files (x86)\\chromedriver.exe\")\r

  \r

  driver = webdriver.Chrome(service=path)

  \ </code>\r

  \        </pre>


  \r

  <p>The next step is to create some variables to store our user email,
  password for login, dashboard URL and the URL for the page containing our
  links, in this instance I have named it content.</p>


  <pre class=\"code_terminal\">\r

  \          <code>

  username = \"useremail@mail.com\"\r

  password = \"userpassword1\"\r

  \r

  dashboard =
  \"https://store-h68l9z2lnx.mybigcommerce.com/manage/dashboard\"\r

  content =
  \"https://store-h68l9z2lnx.mybigcommerce.com/manage/content/pages\"

  \ </code>\r

  \        </pre>


  <p>We then pass the dashboard URL to the driver. When the driver attempts to
  access the dashboard page however, it will automatically be redirected to the
  login page instead. This is where our variables come into play.</p>

  <p>We use the “.find_element” function to find the user input fields which
  we earlier identified. We then use the “.send_keys” method and pass our
  matching constants, so username into the email input and password into the
  password input. At this stage, our driver finds the email, input and enters
  our username, which we have previously defined as our email address. We then
  locate the login button by its name of “commit”, and use .click() to continue.
  The next part is where the cunning of the program comes to play.</p>


  <pre class=\"code_terminal\">\r

  \          <code>

  driver.get(dashboard)\r

  \r

  driver.find_element(By.NAME, \"user[email]\").send_keys(username)\r

  driver.find_element(By.NAME, \"user[password]\").send_keys(password)\r

  driver.find_element(By.NAME, \"commit\").click()\r


  \ </code>\r

  \        </pre>


  <p>We create a variable called “verification” which stores a run time user
  input. This allows our program to pause. During this pause we check our emails
  for the verification code, paste it into the terminal and hit enter. Be sure
  not to include any spaces after the code or else the program will crash. Once
  we hit enter our program continues from where it left off. At this point it
  finds the input field for the verification code. Using “.send_keys” we send
  the new “verification” variable to the input field before using “.click()” to
  verify the code.</p>



  <pre class=\"code_terminal\">\r

  \          <code>

  verification = input(\"Enter verification code: \")\r

  \r

  driver.find_element(By.NAME,device_verification[otp_code]\").send_keys(veri\
  fication)\r

  \r

  driver.find_element(By.NAME, \"commit\").click()\r

  \r


  \ </code>\r

  \        </pre>


  <p>Just like that we are into the BigCommerce dashboard past the 2FA! At
  this stage we can navigate directly to the URL stored in our content variable.
  This “content” page contains the links to our webpages editorial pages, which
  in turn store our desired URLs. In this example I had to switch iframes to the
  iframe containing the content we require, but this step is not usually
  required. </p>


  <pre class=\"code_terminal\">\r

  \          <code>

  driver.get(content)\r

  driver.switch_to.frame(\"content-iframe\")\r

  \r

  \r


  \ </code>\r

  \        </pre>


  <p>Inspecting each page link we can see that it is contained within an <a>
  tag that has a title of “Edit this page”, and so it is this that we target in
  our scraping bot. </p>


  \r\n"
image8: /images/contentPage.png
imageAlt8: Inspecting URL
---
