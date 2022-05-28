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
JSON: "\r{\r

  \  \"@context\" : \"http://schema.org\",\r

  \  \"@type\" : \"Article\",\r

  \  \"name\" : \"Web Scraping Content from Two-Factor Authentication
  Website\",\r

  \  \"author\" : {\r

  \    \"@type\" : \"Person\",\r

  \    \"name\" : \"Sean Grayson\"\r

  \  },\r

  \  \"datePublished\" : \"2022-05-24\",\r

  \  \"image\" : \"https://www.seansguide.com/images/code.jpg\",\r

  \  \"url\" :
  \"https://www.seansguide.com/blogs/tech/coding/web-scraping-a-two-factor-auth\
  entication-website/\"\r

  }\n"
paragraph1: " <p>Web scraping is a fantastic tool for any programmer to add to
  their skill\r

  \  set. Web scraping allows you to automate data retrieval from different
  websites. For example, you could scrape a list of email addresses from one
  website, the price\r

  \  of products from another, or even the Google Search results page. The
  possibilities are endless, and the process is both helpful\r

  \  and fun to set up. While this example will undoubtedly be different to
  your problem, changing some lines of code or following the general methodology
  may help.</p>\r

  \r

  \r

  \  <h3>Jump to:<h3>\r

  \r

  \  <ul class=\"list--jump\">\r

  \r

  \  <li><a class=\"jump\" href=\"#2FA\">Why Scrape a Website with 2FA-Factor
  Authentication?</a></li>\r

  \r

  \  <li><a class=\"jump\" href=\"#StaticVsDynamic\">Static Vs. Dynamic
  Websites</a></li>

  \  <li><a class=\"jump\" href=\"#ScrapingDynamicSelenium\r\">Scraping a
  Dynamic Website with Selenium</a></li>

  \r

  \  <li><a class=\"jump\" href=\"#step1\">Step One: Simulating the
  Process</a></li>\r

  \r

  \  <li><a class=\"jump\" href=\"#step2\">Step Two: Creating the
  Scraper</a></li>

  \  <li><a class=\"jump\" href=\"#step3\">Step Three: Bypassing Two-Factor
  Authentication</a></li>

  \  <li><a class=\"jump\" href=\"#step4\">Step Four: Using Excel to Tidy
  Data</a></li>

  \  <li><a class=\"jump\" href=\"#step5\">Step Five: Scraping Content Page
  URLs</a></li>

  \r

  \r

  \  <li><a class=\"jump\" href=\"#step6\">Creating a Headless
  Browser</a></li>

  \  <li><a class=\"jump\" href=\"#fullcode\">Full Code</a></li>\r

  \r

  \  </ul>"
image1: /images/code.jpg
imageAlt1: Python code for web scraping
subheading2: ""
paragraph2: "<h2 id=\"2FA\">Why Scrape a Website with 2FA-Factor
  Authentication?</h2>\r

  \r

  \  <p>The need to scrape a website with two-factor authentication (2FA) is
  quite rare. 2FA provides an extra layer of security to a user account of a
  given platform. When a user attempts to login to the platform a once off code
  is often sent to the email address associated with the account. Typically,
  most data the user can access on their dashboard can be exported, or there is
  usually an API for data retrieval. If neither of these options exist however,
  webscraping can allow you to retrieve the data you need, you just have the
  small task of bypassing 2FA.</p>


  <p>When attempting to scrape a 2FA protected website myself, I found various
  tutorials outlining how to login to a regular website, but very sparse
  information on how to circumnavigate 2FA. I should make it clear that this
  tutorial is for someone who is the true owner of an account protected with 2FA
  and not a guide on how to hack someone else’s account. </p>


  <p>I like to keep my blogs focused on the topic of discussion, so I won’t be
  going through the technicalities of how to install the required dependencies
  (Selenium and the WebDriver) in this blog, or any basic concepts of Python
  programming. In the future I am going to write some content surrounding this
  but for now I will leave it up to you to do some quick research (I recommend
  YouTube). </p>


  <h2 id=\"StaticVsDynamic\">Static vs Dynamic Websites?</h2>\r

  \r

  \  <p>There are two possible options to take when trying to overcomethe
  scraping a 2FA website, but which option you chose depends largely on what
  kind of website you are trying to scrape. If you are unsure of what kind of
  website you are scraping, check out my guide Static vs Dynamic Sites: What’s
  the Difference? </p>\r

  \r

  \r

  \  <p>If your site is a static website you can simply use the beautifulsoup
  library in conjunction with a neat little trick to store your cookies (I will
  be writing some content on this soon). This allows you to bypass login and TFA
  and scrape the data as required. If you are scraping a dynamic website
  however, thing can be a little more complicated. </p>\r

  \r

  \r

  <h2 id=\"ScrapingDynamicSelenium\">Scraping a Dynamic Website with
  Selenium</h2>\r

  \  <p>Below I have outlined how to use Selenium to scrape the user dashboard
  of a BigCommerce website. Selenium allows for the automation of a webdriver,
  thereby simulating a web browsers interaction with a web page. BigCommerce is
  a popular eCommerce provider. The user dashboard for a BigCommerce website is
  dynamically generated and protected by 2FA at login.</p>\r

  \r

  \r

  \  <p>For this example, we need to fetch the URL for each of our content
  pages (this was my original problem as BigCommerce does not allow the export
  of your content page URLs). Below is the list of our content pages once we
  have logged into our dashboard.</p>"
image2: /images/contentPage.png
imageAlt2: Content page with URLs
caption2: BigCommerce page with the list of URLs
subheading3: ""
paragraph3: "<p>While we can see what looks like links to our content pages, if
  we right click\r

  \  and inspect on any given link we can see that it is actually a link to
  another\r

  \  page. </p>\n"
image3: /images/inspectingContentpage.png
imageAlt3: Inspecting URLs
caption3: Inspecting the URLs in developer tools
subheading4: ""
paragraph4: "<p>This next page acts as an editorial page for the content page,
  and it\r

  \  is here that the content web page URL is stored.</p>"
image4: /images/editorialPage.png
imageAlt4: Editorial page
caption4: Editorial page, note you can see the true page URL
subheading5: ""
paragraph5: "  <p>This means that our task is split into two stages. First, we
  must scrape\r

  \  the list of editorial URLs on the dashboard, before visiting each URL\r

  \  individually and scraping the required content web page URL. </p>




  \  <h2 id=\"step1\">Step One: Simulating the Process</h2>\r

  \r

  \  <p>The first step of our web scraping journey is to attempt to access our
  BigCommerce dashboard. To do this we must login and pass our one-time 2FA code
  for login. To simulate the process, we are going to login to the dashboard
  from an incognito browser. This ensures we follow the same route our program
  will, and that we are not automatically logged in.</p>"
image5: /images/login.png
imageAlt5: Login
caption5: BigCommerce login page
subheading6: ""
paragraph6: <p>By inspecting the email field, we can see the code for the input.
  In this example we can see that the input field has name = “user[email]” and
  id = “user_email”. We can target either of these within our webscraper but in
  this case I am going to use the name attribute for the sake of consistency, as
  the login button has a name but not an ID. </p>
image6: /images/inspectingemailinput.png
imageAlt6: Inspecting email input
caption6: Using developer tools we inspect the email input field. We then repeat
  this for the password field and the login button to find their names
subheading7: ""
paragraph7: " <p>We then repeat this process for the password. And again for the
  login\r

  \  button. </p>


  <p>From this we can see that the password has a name=“user[password]” and
  the login button has a name=“commit”.</p>

  <p>After logging in manually we are redirected towards a new page requesting
  our 2FA code. On this page we carry out the same inspection process to find
  the name of the code field and the verify button.</p>"
image7: /images/inspectingverify.png
imageAlt7: Inspecting the verify option
caption7: Inspecting 2FA code input field
subheading8: ""
paragraph8: "<p>In this instance they have the attributes
  name=“verification[opt_code]” and name=“commit”.</p>


  <h2 id=\"step2\">Step Two: Creating the Scraper</h2>

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


  \ <p>Next we initialise our chrome driver for the actual scraping. This is\r

  \  achieved by storing the browser driver file path in a variable and then
  using that\r

  \  variable to instantiate a driver object. The chrome service is to avoid\r

  \  depreciation in future Selenium releases.</p>


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

  <p>Next we create some variables to store our user email, password,
  dashboard URL and the URL for the page containing all of our content pages, in
  this case I have named it content.</p>


  <pre class=\"code_terminal\">\r

  \          <code>

  username = \"useremail@mail.com\"\r

  password = \"userpassword1\"\r

  \r

  dashboard = \"https://store.mybigcommerce.com/manage/dashboard\"\r

  content = \"https://store.mybigcommerce.com/manage/content/pages\"

  \ </code>\r

  \        </pre>


  <p>We then pass the dashboard URL to the driver. When the driver attempts to
  access the dashboard page it will automatically be redirected to the login
  page instead. This is where our variables come into play.</p>

  <p>We use the “.find_element” function to find the user input fields which
  we identified earlier. We then use the “.send_keys” method and pass our
  matching variables, so username is passed into the email input and password is
  passed into the password input. We then locate the login button by its name of
  “commit”, and use .click() to continue.</p>


  <pre class=\"code_terminal\">\r

  \          <code>

  driver.get(dashboard)\r

  \r

  driver.find_element(By.NAME, \"user[email]\").send_keys(username)\r

  driver.find_element(By.NAME, \"user[password]\").send_keys(password)\r

  driver.find_element(By.NAME, \"commit\").click()\r

  \ </code>\r

  \        </pre>


  <h2 id=\"step3\">Step Three: Bypassing Two-Factor Authentication</h2>

  <p>Next we create a variable called “verification” which stores a run time
  user input. This step allows the program to pause as it waits for our input
  and is essentially the step that allows us to bypass 2FA. During this pause we
  check our emails for the verification code, paste it into the terminal and hit
  enter. Be sure not to include any spaces after the code or else the program
  will crash. Once we hit enter our program continues from where it left off. At
  this point it finds the input field for the verification code. Using
  “.send_keys” we send the new “verification” variable to the input field before
  using “.click()” to verify the code.</p>



  <pre class=\"code_terminal\">\r

  \          <code>

  verification = input(\"Enter verification code: \")\r

  \r

  driver.find_element(By.NAME,device_verification[otp_code]\").send_keys(veri\
  fication)\r

  \r

  driver.find_element(By.NAME, \"commit\").click()\r

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
  This “content” page contains all of the links to our editorial pages, which in
  turn store our desired URLs. In this example I had to switch iframes to the
  iframe containing the content we require. To find the correct iframe simply
  use developer tools to inspect the content you are attempting to scrape, use
  CTRL + F to search for the keyword \"iframe\" and take the name of the iframe
  that your content is contained within. This step is not always required. </p>


  <pre class=\"code_terminal\">\r

  \          <code>

  driver.get(content)\r

  driver.switch_to.frame(\"content-iframe\")\r

  \r

  \r

  \ </code>\r

  \        </pre>


  <p>Inspecting each page link we can see that it is contained within an <a>
  tag that has a title of “Edit this page”, and so it is this that we will
  target in our scraping bot. </p>


  \r\n"
image8: /images/inspectcontentpage.jpg
imageAlt8: Inspecting URL
caption8: Inspection of content page URLs shows that they all have a title
  attribute of “Edit this page”
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
caption9: List of URLs printed on our terminal
paragraph10: >-
  <h2 id="step4">Using Excel to Tidy Our Data:</h2>

  <p>Once we have a list of editorial page URLs we can move onto stage two of the process. First, we simply copy and paste the terminal code into Excel to tidy up the data. We can remove any duplicates, filter for URLs not containing “pageId=” and delete them, and prepare the new structure for storing this list of URLs in a Python list. This stage could be handled as part of the Python program itself, but sometimes I find Excel easier to visualize the data. </p>
image10: /images/excelurls.png
imageAlt10: list of URLs in Excel
caption10: Pasting data into Excel for clean up
paragraph11: >-
  <p>We can prepare the URL structure by using the concatenate formula. The
  correct URL structure is as follows:</p>

  <p>“URL“, &rarr; which looks like &rarr; “example.com/example-page”,</p>

  <p>To do this we add a “ “, to the cells on either side of our URLs. We then use the concatenate formula to join them to our URLs. Note the dollar signs in the cells on either side of the URL. This is to lock these cells in place meaning, the only cell to change as we go down the sheet is the URL. This is achieved by pressing F4.</p>
image11: /images/concatenate.png
imageAlt11: Using the concatenate formula in Excel
caption11: Note the dollar signs to lock cell E1 and G1 so that they are applied
  to each cell (or URL) as we drag down.
paragraph12: <p>By dragging down we can now apply the correct structure to all
  of our URLs.</p>
image12: /images/correcturls.png
imageAlt12: Correct URL structure in Excel
caption12: Correct structure applied to all URLs
paragraph13: "\r

  <p>We can now take the list of URLs and add them to a list in our Python
  program.</p>


  <pre class=\"code_terminal\">\r

  \          <code>

  urls =
  [\"https://store.mybigcommerce.com/admin/index.php?ToDo=editPage&pageId=1519\
  \",\r

  \"https://store.mybigcommerce.com/admin/index.php?ToDo=editPage&pageId=12542\
  \",\r

  \"https://store.mybigcommerce.com/admin/index.php?ToDo=editPage&pageId=1538]\
  \r

  \r

  \r

  \r

  \r

  \ </code>\r

  \        </pre>

  <h2 id=\"step5\">Scraping Content Page URLs:</h2>

  <p>Once we have this list of URLs which contains the actual data we require,
  we can use a for loop to cycle through them. Applying a wait of 3 seconds to
  the browser helps to ensure the page has finished loading and we are not
  missing any elements. </p>


  <p>Next we can then create our desired output URL by creating a string in
  which we concatenate our domain name to the individual page URL. We locate the
  individual page URL by using its ID after inspecting.</p>"
image13: /images/customurl.png
imageAlt13: Retrieving the custom URL for the webpage
caption13: Inspecting the URL field so we can target its ID in our scraper
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


  <h2 id=\"step6\">Creating a Headless Browser:</h2>


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

  \ </pre>"
paragraph15: "<h2 id=\"fullcode\">Full Code</h2>

  <h3>Stage One:</h3>

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

  \ </pre>


  <h3>Stage Two:</h3>


  <pre class=\"code_terminal\">\r

  \         <code>\r

  \r

  \r

  \r

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

  \r

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

  \ </code>\r

  \ </pre>"
---
