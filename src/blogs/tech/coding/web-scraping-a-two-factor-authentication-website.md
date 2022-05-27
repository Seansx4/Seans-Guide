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

  <a href="#step1">Step 1</a>




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
paragraph9: "<p>To scrape all of these editorial page links we create a for
  loop, instructing the driver to find all <a> elements with the title of “Edit
  this page”. The for loop then cycles through all of these <a> tags and prints
  their hrefs, which is their editorial links. The code is below.</p>


  <pre class=\"code_terminal\">\r

  \          <code>

  for result in driver.find_elements(By.CSS_SELECTOR, 'a[title=\"Edit this
  page\"]'):\r

  \    print(result.get_attribute(\"href\"))\r

  \r

  \r

  \r


  \ </code>\r

  \        </pre>

  Once we run this program we get a number of URLs printed on our terminal. "
image9: /images/codeterminal.png
imageAlt9: code terminal of URLs
paragraph10: <p>At this stage, you can simply copy and paste the terminal code
  into Excel to tidy up the data. I remove duplicates, filter for any URLs not
  containing “pageId=” and delete them, and prepare the new structure for
  storing this list of URLs in a Python list. This stage could be handled as
  part of the Python program itself, but sometimes I find Excel easier as I like
  to visualize the data. </p>
image10: /images/excelurls.png
imageAlt10: list of URLs in Excel
paragraph11: >-
  <p>I prepare the URL structure by using the concatenate formula. The correct
  URL structure is as follows:</p>

  <p>“URL“, &rarr; which looks like &rarr; “example.com/example-page”,</p>

  <p>To do this we add a “ “, to the cells on either side of our URLs. We then use the concatenate formula to join them to our URLs. You can see the dollar signs in the cells on either side of the URL. This is to lock these cells in place meaning, the only cell to change as we go down the sheet is the URL. This is achieved by pressing F4.</p>
image11: /images/concatenate.png
imageAlt11: Using the concatenate formula in Excel
image12: /images/correcturls.png
imageAlt12: Correct URL structure in Excel
paragraph13: "<p>We can now take the list of URLs and add them to a list in our
  Python program. </p>


  <pre class=\"code_terminal\">\r

  \          <code>

  urls =
  [\"https://store-h68l9z2lnx.mybigcommerce.com/admin/index.php?ToDo=editPage&p\
  ageId=1519\",\r

  \"https://store-h68l9z2lnx.mybigcommerce.com/admin/index.php?ToDo=editPage&\
  pageId=12542\",\r

  \"https://store-h68l9z2lnx.mybigcommerce.com/admin/index.php?ToDo=editPage&\
  pageId=1538\",\r

  \"https://store-h68l9z2lnx.mybigcommerce.com/admin/index.php?ToDo=editPage&\
  pageId=1540\",\r

  \"https://store-h68l9z2lnx.mybigcommerce.com/admin/index.php?ToDo=editPage&\
  pageId=1786\",\r

  ]\r

  \r

  \r

  \r

  \r


  \ </code>\r

  \        </pre>


  <p>Once we have this list of URLs which contains the actual data we require,
  we can use a for loop to cycle through them. In this case I apply a wait of 3
  second to the browser, just to make sure the webpage has finished loading and
  we are not missing any elements. </p>


  <p>The next line of code is to switch to the iframe containing the content
  we require, but this step may not be required in your example. </p>


  <p>We can then create our desired output URL by creating a string in which
  we concatenate our domain name to the individual page URL. We locate the
  individual page URL by using its ID after inspecting.</p>"
image13: /images/customurl.png
imageAlt13: Retrieving the custom URL for the webpage
paragraph14: "<p>We then print the full URL for the page before moving on to the
  next URL in the list. </p>


  <pre class=\"code_terminal\">\r

  \          <code>

  for url in urls:\r

  \    driver.get(url)\r

  \    time.sleep(3)\r

  \    driver.switch_to.frame(\"content-iframe\")\r

  \    url = \"https://www.example.com\" + driver.find_element(By.ID,
  \"page_custom_url\").get_attribute(\"value\")\r

  \    print(url)\r

  \r

  \r

  \r

  \r

  \r


  \ </code>\r

  \        </pre>


  <p>The final piece to the puzzle is to turn this session into a headless
  session, meaning selenium will run in the background for us and we do not have
  to watch the browser. To do this we simply create a chrome options object, in
  which we set the browser option to headless. We then pass this options object
  as an argument to our driver.</p>




  <pre class=\"code_terminal\">\r

  \          <code>

  chrome_options = Options()\r

  chrome_options.headless = True\r

  driver = webdriver.Chrome(service=path, options=chrome_options)\r

  \r

  \r

  \r

  \r

  \r

  \r


  \ </code>\r

  \        </pre>"
subheading15: Full Code
paragraph15: "<h3 id="step1">Step 1</h3>

  <pre class=\"code_terminal\">\r

  \          <code>

  from selenium import webdriver\r

  import time\r

  from selenium.webdriver.common.by import By\r

  \r

  from selenium.webdriver.chrome.options import Options\r

  from selenium.webdriver.chrome.service import Service\r

  \r

  chrome_options = Options()\r

  chrome_options.headless = True\r

  \r

  username = \"username@mail.com\"\r

  password = \"password\"\r

  \r

  \r

  path = Service(\"Filepath of your webdriver\")\r

  driver = webdriver.Chrome(service=path, options=chrome_options)\r

  dashboard = \"Dashboard URL\"\r

  content = \"Content URL\"\r

  driver.get(dashboard)\r

  \r

  \r

  driver.find_element(By.NAME, \"user[email]\").send_keys(username)\r

  driver.find_element(By.NAME, \"user[password]\").send_keys(password)\r

  driver.find_element(By.NAME, \"commit\").click()\r

  \r

  \r

  verification = input(\"Enter verification code: \")\r

  driver.find_element(By.NAME,
  \"device_verification[otp_code]\").send_keys(verification)\r

  driver.find_element(By.NAME, \"commit\").click()\r

  \r

  driver.get(content)\r

  driver.switch_to.frame(\"content-iframe\")\r

  \r

  for result in driver.find_elements(By.CSS_SELECTOR, 'a[title=\"Edit this
  page\"]'):\r

  \    print(result.get_attribute(\"href\"))\r

  \r

  \r

  \r

  \r

  \r

  \r

  \r


  \ </code>\r

  \        </pre>




  <h3>Step 2</h3>

  <pre class=\"code_terminal\">\r

  \          <code>

  from selenium import webdriver\r

  import time\r

  from selenium.webdriver.common.by import By\r

  \r

  from selenium.webdriver.chrome.options import Options\r

  from selenium.webdriver.chrome.service import Service\r

  \r

  chrome_options = Options()\r

  chrome_options.headless = True\r

  \r

  username = \"username@mail.com\"\r

  password = \"password\"\r

  \r

  \r

  path = Service(\"Filepath of your webdriver\")\r

  driver = webdriver.Chrome(service=path, options=chrome_options)\r

  dashboard = \"Dashboard URL\"\r

  content = \"Content URL\"\r

  driver.get(dashboard)\r

  \r

  \r

  driver.find_element(By.NAME, \"user[email]\").send_keys(username)\r

  driver.find_element(By.NAME, \"user[password]\").send_keys(password)\r

  driver.find_element(By.NAME, \"commit\").click()\r

  \r

  \r

  verification = input(\"Enter verification code: \")\r

  driver.find_element(By.NAME,
  \"device_verification[otp_code]\").send_keys(verification)\r

  driver.find_element(By.NAME, \"commit\").click()\r

  \r

  \r

  urls = [\"URL 1\",\r

  \"URL 2\",\r

  \"URL 3\",\r

  \"Etc etc..\"\r

  ]\r

  \r

  \r

  for url in urls:\r

  \r

  \    driver.get(url)\r

  \    time.sleep(3)\r

  \    driver.switch_to.frame(\"content-iframe\")\r

  \    url = \"yourdomain.com\" + driver.find_element(By.ID,
  \"page_custom_url\").get_attribute(\"value\")\r

  \    print(url)\r

  \r

  \r

  \r

  \r

  \r

  \r

  \r

  \r


  \ </code>\r

  \        </pre>"
---
