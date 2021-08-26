Every single day that passes by, terabytes of data are generated on the Internet. To a data enthusiast, it is of utmost importance to have the knowledge of methods that can be used to put that data to use. But even before moving to the analysis, we need to have the data with us, right? That is where web scraping comes to the rescue. Web scraping is one of the many ways that one can utilize to collect data from the Internet. And by the end of this article, you will have the working knowledge of building a basic web scraper.

### What do we intend to do?
We will scrape the website of one of the leading newspapers of India, The Times of India, and get the names of the most trending news topics in the country. For this purpose, we will take the help of the Python library Beautiful Soup. 

### What prerequisite knowledge should have you have?
- Basic knowledge of the Python programming language and data structures
- Basic knowledge of HTML tags

Let’s start building our scrapper!

*Step 1: Import the libraries*

After opening your preferable code editor, the first step is to import the libraries that we will need. For this application, we need two libraries:
1. requests: to send an HTTP request to the website that we want to get the data from.
2. bs4:  The Beautiful Soup library, using which we will collect the exact information that we need from the website. You will have to import the class BeautifulSoup from bs4. 

```
import requests
from bs4 import BeautifulSoup
``` 
*Step 2: Define the URL*

Initialize a variable that stores the website address as a string. Here is the URL of the website: https://timesofindia.indiatimes.com/. 

`url = "https://timesofindia.indiatimes.com/"`

At this point, you can also visit the website and have a look at what we are going to collect from it. After landing on the homepage of the website, if you scroll down a little, you can see a heading “TRENDING NEWS” beside which, subject names have been mentioned. This is what we are going to collect from the website. It has been highlighted (in red) in the image below.

![homepage of the website](https://raw.githubusercontent.com/Anpr1211/Data-files/master/image/website_img_1.png)

*Step 3: Send a request to the website*

Using the `get()` function of the requests library, we will send a request to the website. This will return the underlying code of the website that can be displayed using the content attribute of the variable that you have stored the result in. 

```
page = requests.get(url)                    # sending the request to the website
page.content                                     # displaying the HTML content of the website
```

Your output should be in the format given below. The exact output might differ due to the changes in the website content itself. 

![output of page.content](https://raw.githubusercontent.com/Anpr1211/Data-files/master/image/website_img_2.png)

This output response is in binary format and cannot be worked with directly. Hence, we will have to parse it using BeautifulSoup.

*Step 4: Parsing the website response with BeautifulSoup*

Pass the page content as a parameter to the BeautifulSoup function and specify the parser that you want to use. Here, we have used the `html.parser`. You can use any other parser as well which is suitable to the website that you want to scrape.

`soup = BeautifulSoup(page.content, "html.parser")`

Now we can use the `soup` variable to extract the necessary information from the website.

*Step 5: Finding the appropriate tags on the website*

Before we write any more code, we first need to find out the HTML tags on the website that store the required information. Go to the website, right-click and select “Inspect” from the menu, or you can simultaneously press Ctrl+Shift+I. 
![finding HTML tags](https://raw.githubusercontent.com/Anpr1211/Data-files/master/image/website_img_3.png)

If you are working in Google Chrome, you will be able to see HTML code on the right-hand side, as displayed in the image above. You can inspect the website in any other browser as well. 

![finding the tag of the TRENDING NEWS section](https://raw.githubusercontent.com/Anpr1211/Data-files/master/image/website_img_4.png)

Click on the arrow at the left top corner of the right sidebar, as pointed out in the image above. When you select that, whichever element that you scroll over on the website, the corresponding HTML will be highlighted in the sidebar. 

As displayed in the image, the elements that we need are present in a div tag with a class name *_1gUIC undefined*. 

*Step 6: Getting the data from the `soup` variable*

Now, we need to find all the div tags that have a class name *_1gUIC undefined*, as it stores the required information that we need. We use the find function, specify the tag that we want to look for (“div”), and the class name of the element. 

`trending_news = soup.find('div', class_="_1gUIC undefined")`

We have now got the data from the “TRENDING NEWS” section of the website. We need to move one step further and find out the tags referring to the subject names. 

You can observe that the subject or topic names are present in “a” tags with the class name *keyword*.

![finding the tag of the trending topics](https://raw.githubusercontent.com/Anpr1211/Data-files/master/image/website_image_5.png)

We will find out all the “a” tags with the class *keyword* in the “trending_news” variable using the `find_all()` function. This time, we have used the attrs parameter of the `find_all()` function, which often helps when you need to filter out tags based on multiple conditions. 

`tags = trending_news.find_all('a', attrs={'class':"keyword"})`

The `tags` variable is a list of size 10. This is what the output of the first element of the `tags` list looks like. 

`<a class="keyword" data-ga="TopSection-Actions|Click-TrendingTopics_Coronavirus news_https://timesofindia.indiatimes.com/india/coronavirus-live-updates-august-24/liveblog/85578552.cms/1" frmappuse="1" href="https://timesofindia.indiatimes.com/india/coronavirus-live-updates-august-24/liveblog/85578552.cms" rel="noopener" style="width:100%;display:inline-block" tabindex="-1" target="_blank">Coronavirus news</a>
`

To get the exact text, use the text attribute in the following way.
`tags[0].text`
![output of tags[0].text](https://raw.githubusercontent.com/Anpr1211/Data-files/master/image/website_img_6.png)

Now you can simply iterate over the list elements and get the topic names. 
```for i in range(len(tags)):
    print(tags[i].text)```
    
![all the trending topic names](https://raw.githubusercontent.com/Anpr1211/Data-files/master/image/website_img_7.png)

The entire code has been given below:
```
import requests
from bs4 import BeautifulSoup

url = "https://timesofindia.indiatimes.com/"

page = requests.get(url)                    
page.content                                

soup = BeautifulSoup(page.content, "html.parser")

trending_news = soup.find('div', class_="_1gUIC undefined")

tags = trending_news.find_all('a', attrs={'class':"keyword"})

tags[0].text

for i in range(len(tags)):
    print(tags[i].text)
```

Therefore, we have successfully scraped the website of a newspaper daily and extracted the trending news topics from it. Now, you can choose to do whatever you want with this data, perform a comparative study of the trending news topics in a week or a month, find out how long a certain subject generally stays in the trend, etc. 
