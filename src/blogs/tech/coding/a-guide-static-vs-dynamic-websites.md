---
title: "A Guide: Static Vs. Dynamic Websites"
metaDescription: In this article I outline the main differences between a
  statically generated website and a dynamically generated website. The main
  advantages and disadvantages of each are also outlined, along with some
  examples.
datum: Jun 13th 22
blogTitle: "A Guide: Static Vs. Dynamic Websites"
description: In this article the differences between static and dynamic websites
  are outlined, along with their advantages and disadvantages and some common
  examples.
author: Sean
tags:
  - post
  - coding
  - featured
JSON: "\r{\r

  \  \"@context\" : \"http://schema.org\",\r

  \  \"@type\" : \"Article\",\r

  \  \"name\" : \"A Guide: Static Vs. Dynamic Websites\",\r

  \  \"author\" : {\r

  \    \"@type\" : \"Person\",\r

  \    \"name\" : \"Sean Grayson\"\r

  \  },\r

  \  \"datePublished\" : \"2022-06-13\",\r

  \  \"image\" : \"https://www.seansguide.com/images/code.jpg\",\r

  \  \"url\" :
  \"https://www.seansguide.com/blogs/tech/coding/web-scraping-a-two-factor-auth\
  entication-website/\"\r

  }\n"
paragraph1: <p>An important concept to understand when learning about the web is
  the difference between static and dynamic websites. Static websites are
  created using HTML, CSS and Javascript. These client-side languages ensure
  every user is served the same version of a website. Conversely, dynamic
  websites utilise server-side languages to deliver alternative versions of a
  website to different users. Let’s look into what this really means and explore
  the advantages and disadvantages of both types of websites.</p>
image1: /images/websitebuilder.png
imageAlt1: Animation of a website builder
subheading2: ""
paragraph2: >+
  <h2>What is a Static Website?</h2>

  <p>As mentioned, static websites are built using the client-side languages of HTML, CSS and JavaScript. But what does this actually mean? Well, a client is anyone who visits a website. As you read this article, you are in fact a client of this website. A client-side language is a coding language that is processed on the client’s side. This “processing” is taken care of by the client’s web browser, for example Google Chrome, Microsoft Edge, Safari, or whatever browser the client is using when they visit a website.</p>


  <p>The three client-side languages of HTML, CSS and JavaScript form the building blocks of today’s web. HTML files provide the structure of a website while CSS provides the styling. JavaScript contributes by providing functionality to different elements on a website, for example a button that displays more information when clicked. </p>


  <p>When a client visits a website a series of interactions occur between the client’s device and the server which stores the website. The client’s browser requests the files required to display the website on the client’s device. In the case of a static website, the server simply sends the required HTML, CSS and JavaScript files to the client’s browser. During this exchange between the client and the server, none of the files required for the website are altered. This means that every client who requests the website from the server is delivered the exact same content, meaning the website is the same for everyone who visits. </p>


  <p>Using JavaScript and CSS, a static website can be made interactive, but the term “static website” arises from the fact that the content is essentially the same. A static website does not depend on a database design whereby different information is pulled from the database and displayed depending on the client. </p>


  <h2>Examples of Static Websites</h2>

  <p>Static websites are often used for small informational sites, often known as brochure websites. </p>

image2: /images/brochurewebsite.png
imageAlt2: Brochure website
caption2: Brochure websites act as small informational sites
subheading3: ""
paragraph3: >-
  <p>These brochure websites are ideal for providing an individual or a small
  business with a web presence and a platform to showcase some key
  information.</p><p>Brochure sites are popular for small local businesses,
  personal websites, CV websites and portfolio websites.</p>

  <p>While static websites may not be as common as they once were, they are still widely used across the web. In fact, static sites are even making a comeback in recent years due to some of their inherent advantages.</p>
subheading4: ""
paragraph4: >+
  <h2>Advantages of Static Websites</h2>

  <p>Believe it or not, there are many advantages to static websites. These advantages include:</p>


  <ul>

  <li>Static websites can be easier and less time consuming to develop and deploy</li>

  <li>Due to their architecture they are more secure. Static websites contain all the content they are going to display to a client in pre-generated files. This means they do not communicate with any databases, which is often a point of attack for hackers. </li>

  <li>The biggest advantage of static websites is their speed, which is a reason for their recent surge in popularity.</li>

  </ul>

  <h3>Static Website Speed</h3>

  <p>Due to their simplicity, static sites are typically much faster than clunky dynamic websites. In 2020 Google announced the roll out of <a href="https://backlinko.com/hub/seo/core-web-vitals" target="_blank">Web Core Vitals</a>. One of these core vitals is site speed. Google favors fast websites, and so it is no surprise that developers are returning to static sites as an attempt to climb the ever-competitive Google Rankings. </p>

image4: /images/pagespeed.jpg
imageAlt4: Pagespeed Insight Results
caption4: Page Speed Insight results for my website. The fast loading speed is
  owed to the fact that this is a static website
subheading5: ""
paragraph5: >-
  <h2>Disadvantages of Static Websites</h2>

  <p>The biggest drawback to a static website is the lack of scalability. As a static website is comprised of several HTML pages, any global website change must be made to each individual page. For example, imagine an owner of a static website wished to change a link in the footer section of their website. The footer area is found across all web pages and so the website owener would have to open up each HTML file to make the change. Thus, making updates to a static website can be time consuming, and is not practical for a large website. </p>


  <p>Another drawback to static websites is the fact that content can’t be customised for users. As every client receives the same web page, it is not possible to create unique experiences. For example, we can’t display alternate content to users based on their location or their previous visits to the website. </p>



  <h3>Static Site Generators</h3>

  <p>The disadvantage of static site scalability is combated by static site generators (SSG). These SSG have lead to a surge in popularity of static sites. SSG are tools which allows you to generate static websites using templates and data known as front matter. Using these templates, editing and compiling entire static sites is made much easier. In the case of the example above, if a website owener were to use a SSG, they would only have to change the code in thier footer template once, and it would then be inherited to each page. SSG are very handy tools, in fact I have built this very website using one! 
subheading6: ""
paragraph6: >-
  <h2>Dynamic Websites</h2>

  <p>While dynamic websites still use the client-side languages of HTML, CSS and JavaScript, they also make use of server-side languages. Server-side languages are programming languages that run on the server, performing specific tasks on a web page before it is sent to the clients browser. Examples of server-side languages include PHP, Python, Ruby and C#.</p>

  <p>The use of server-side languages allows dynamic websites to interact with databases and supply different users with alternative information. This architecture means that when a user requests a page, the server languages will pull the required information from various databases before constructing the HTML page and sending it to the client. A simple example of this is Facebook, whereby the content on the Facebook website is different depending on the user that is logged in. </p>


  <p>It is not just for different private accounts that dynamic websites are used for though. Content may be generated depending on where a client is requesting to access a website from or what the time is.</p>


  <p>The rise in architecture complexity of a dynamic website means that page loading times tend to be increased, especially if a user is requesting a lot of information and multiple databases must be queried. A client does not visualise this extended process however, and instead only experiences a slow load time.</p>


  <h2>Examples of Dynamic Websites</h2>

  <p>Nowadays most websites on the internet use some dynamic architecture. Common examples include:</p>


  <ul>

  <li>Social networking websites, which display custom content to a user once they are logged in</li>

  <li>Ecommerce sites, where a user may see suggested products based on their previous browsing activities</li>

  <li>Website displaying local currency based on location of a client</li>

  </ul>


  <h2>Advantages of Dynamic Websites</h2>

  <p>The main advantage of a dynamic website lies in its ability to generate custom experiences for visiting clients. Providing personalised content based on various factors means dynamic websites are more likely to display content that a user is interested in and create a better user experience. Good user experience creates good sentiment and increases the likelihood of a returning visitor. </p>

  <p>The next primary advantage is the fact that global site wide changes can be made easily, without the need to edit multiple HTML code files. This is vitally important for online businesses as they adapt to changing markets and expectations. </p>

  <p>Finally, dynamic websites are quite scalable. With a traditional static website each page is constructed individually and stored on the server. With a dynamic site the information is stored in a database and retrieved to build web pages automatically. </p>


  <h2>Disadvantages of Dynamic Websites</h2>

  <p>Dynamic websites tend to be more resource intensive, as their architecture inherently requires more organisation. Complications can arise in setting up various databases and ensuring all the different parts communicate with one another efficiently. Most dynamic websites are built using website builders, also known as content management systems (CMS) to handle these increased technicalities. Common examples include Wordpress and Wix. Due to their increase in behind the scenes logic, dynamic websites have increased processing in comparison to a static website. This increased processing can take time and may impact the performance of a site in terms of site speed. While modern dynamic sites are highly optimised, site speed will always be affected due to increased complexities. </p>
subheading7: ""
paragraph7: >-
  <h2>Web scraping Implications </h2>

  <p>The method you take to scrape a website depends largely on what kind of website you are attempting to scrape, or the specific content you are attempting to scrape. Using Python, Beautiful Soup and the Requests library may be used for static content or websites. For more complex content that is loaded dynamically or dynamic websites, web drivers and automation tools such as Selenium may be incorporated. I will be providing a guide shortly on how to decide what libraries to use when scraping a website!</p>
subheading8: ""
paragraph8: ""
subheading9: ""
paragraph9: ""
subheading10: ""
paragraph10: ""
---
