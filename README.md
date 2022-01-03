# PelicanJoe.com

*Travis CI Build Status:*  
[![Build Status](https://travis-ci.org/joetechem/https-pelicanjoe.svg?branch=master)](https://travis-ci.org/joetechem/https-pelicanjoe)  

## A Simple Static Site  

Along with a few other *components*. This repository holds content for a static site built with Hugo. The site itself is hosted on Amazon S3.  


### Components: High-Level Overview  

**Hugo** + **GitHub** + **Travis CI** + **AWS** *(S3, CloudFront, IAM, ACM)*  

## Simply Fast - IO  

"The world's fastest framework for building websites" - https://gohugo.io/  

When comparing the order of operations (or flow) of a static site in general to; let's say a CMS (Content Management System), the amount of steps taken when a user visits the site in a browser is considerably less. This makes for faster output to the end user. For example, when a user hits a domain which directs their traffic to a site built with some Content Management Framework, the CMS is triggered. The CMS pulls the information it needs from its accompanying database. Then from the server, to the database, to the CMS, and finally the browser, where the end user is patiently waiting to see the output of the site's content. In retrospect to just several years ago, this process happens fairly quickly. Now compare this process to a static site. The only player after a user visits the static site is the server, where all the content is ready to be delivered. It only makes sense; with fewer steps, the quicker it is to get to your destination (or output). Here's another way to look at the comparison:

**CMS Built Site:** Browser --> CMS --> Database --> Server | Server --> Database --> CMS --> Browser  

**Static Site:** Browser --> Server | Server --> Browser  

Not only are the amount of players and steps involved smaller; moreover, the project contained (abstracted) in this repository is a static site built with Hugo. Hugo is **Go**-based.
