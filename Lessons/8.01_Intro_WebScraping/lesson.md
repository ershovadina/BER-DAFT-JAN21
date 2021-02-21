# Lesson 8.3

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is  to introduce the business case study of the segmentation of a company by their musical taste. Then we will introduce the main concepts and tools around web scraping.

---

### Learning Objectives: 
After this lesson, students will be able to: 

- Understand clearly the objective of their project in the context of a business strategy.
- Understand the basics of HTML (structure, tags, classes...)
- Locate data in the HTML code of a website using Chrome Inspect.
- First download from a basic webpage using BeautifulSoup.
--- 

---

### Teacher material: 
To prepare and give this lesson, you can use: 

- Slides to introduce the project: https://docs.google.com/presentation/d/1JypC2O7Ek9gXSd5W7HhdxXUqqywzTPMet8uDcrwCCUU/edit?usp=sharing
  **Note to instructor:** Start by showing only the first slide (the super simple version of the project). Show the next slides during the week as the project gets developed.
- Slides for Web Scraping: https://docs.google.com/presentation/d/1AUl84jjdDJMSrEQJMTFr8Sik-SzpRKYZB5dmkdQz_1U/edit?usp=sharing
- This tutorial on HTML: https://www.w3schools.com/html/
- Beautiful Soup Documentation: https://www.crummy.com/software/BeautifulSoup/bs4/doc/
--- 


### Lesson 1 key concepts
> :clock10: 20 min

- Introduce the project
- Understand the need for webscraping with use cases, its pros & cons and the tools used
- Understand the basic structure of HTML
- Learn about HTML tags & attributes (classes and id's)

<details>
<summary> Web Scraping: Why... and how? </summary>

Web scraping is the process of collecting and parsing raw data from the internet ("parsing" here means to convert the data into something that's readable and useful). Why would you do that?

- **No API:** When websites have built a structured tool for you to do request data from them, it's always better to use it - this allow the site's owners to manage the traffic and the data they want to share, and usually provides us with cleaner data. Sometimes, this tool (the API) simply doesn't exist. If you still want to extract information from a website... you are left with web scraping.

- **Automation needed:** Of course, you can simply copy paste manually data from websites. Web scraping consists in building a script that automates this task, and is especially usefull when you need to extract big amounts of data, or you need to collect data periodically from a site.

- **Advantages:** Using APIs, sometimes you need to create an account, get authenticated, read docummentation, follow certain rules... And you usually have a limit (quota) of requests you can make. Web scraping is more free-style: you don't have these limitations.

- **Disadvantages:** You depend on the structure of the website you're trying to scrape. If the structure of the site is really messy, it might be a nightmare to create a script that collects the desired data. The structure of the website can change overnight and your code will simply break. Some websites are protected against web scraping, and the data they show on the screen is very difficult to access (for example, they can block you if you send too many requests, they can simply show data as images).

- **Use cases:**

Ask students what use cases can they imagine for web scraping. Complete their ideas with the following list:
    * Market & product research (scrape ecommerce sites and get the brands, products, prices, reviews… of your competitors!)
    * Lead generation (scrape forums and find contanct info of people that might be interested in your product... bit edgy but effective!)
    * Keyword research (what is people mentioning when they talk about something in the internet? Those keywords might be really useful in SEO/Growth-hacking)
    * “People Analytics” (the field of HR/Talent research is really benefiting from data science stuff. And they might need data like job descriptions, skills, offers… only available through web scraping)
    * Science (if you want to have an original paper that stands out, you better study stuff that’s not present in official records or traditional datasets: subletting apartments, drug use, people’s language & beheavior...)
    * Stock Analysis (using data in the web to forecast... frequently used together with sentiment analysis with varying success)
    * Media Gathering & Analysis (how do different news outlets talk about the candidates of an important election? Web scrape them!)
    * Lots of cool unique personal projects: what websites do you like? Add a project to your portfolio with their data and you'll make yourself different from other job candidates using kaggle data!

- **Tools:**

We will use python's request library again, and other python tools like regex and pandas to clean and structure the information we gather. But there are some modules that are built specifically for web scraping:

* BeautifulSoup: User friendly & Popular, albeit slow and with many dependencies

* Scrapy: fast and self-contained, but not really user friendly

* Selenium: a tool that was designed for website testing, difficult to learn and a bit slow, although very powerful because it allows for scraping JavaScript content.

We will use beautifulsoup because our main goal here is to get things done by Friday :)



</details>

<details>
  <summary>Click for Description: HTML basics</summary>

Basic websites consist of a document structured in a language called HTML, that contains the main content and some of its format, and another document written in CSS that contains the style of each element in the HTML document. When web scraping we're interested in the HTML part, since we want the data of the websites and we don't care that much about the styles.

The basic structure of an HTML Document:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Page Title</title>
    </head>
    <body>
        <h1>My First Heading</h1>
        <p>My first paragraph.</p>
        <p>My second paragraph has a <b>bold<b> word!</p>
    </body>
</html>

```

Show the script above to students and explain the concept of 'elements' and 'tags':

- An **HTML element** is defined by a start tag, some content and an end tag. When webscraping we will mostly be interested in the content, but the tag will be crucial in locating the content.

- **Tags** are just keywords that encapsulate some content. They tell the web browser how to display the content. Some examples of common tags are:

    - `<!DOCTYPE>` and <html> define the document type.
    - <head>, <title> and <body> define the main parts of the document.
    - <h1> to <h6> define headings.
    - <p> defines a paragraph.
    - <b> will make its contents bold.

You can find an exhaustive list here: https://www.w3schools.com/TAGS/default.ASP
    
It's important to remind that, even though these tags give style to websites, currently most of the style will come from CSS or Javascript.

- As you can see above, **tags are not unique**: there are multiple `<p>` tags. That makes getting data a bit tricky if you only want one of the elements with a certain tag.

- An HTML document follows a **tree structure**: elements can have other elements "inside" (meaning, between their opening and closing tag). In the example above, the elements `<h1>My First Heading</h1>`, `<p>My first paragraph.</p>` and `<p>My second paragraph has a <b>bold<b> word!</p>` are "inside" the `<body>` element - we will say that they're children of that element.

- Tags can have **attributes**. Attributes just add additional information or define the behaviour of the elements they belong to. One of the most common attributes is the `href` one, which defines a link. Attributes come in key-value pairs:

```html
<a href="https://www.ironhack.com/">Best bootcamps ever</a>

```
The HTML element above has the tag `a`, which contains the attribute `href`. This attribute has a value, which is the link to the ironhack website.

There are two attributes that deserve special attention to the web-scraper:

- The **id** attribute: unless the creator of the site has broken basic conventions, id's are unique. That makes them the best attributes for locating data in a site. If you discover that the piece of information you're trying to collect is an element that has an id, your job will be SO EASY. Bad news though: that doesn't happen often.

- The **class** attribute: it's often used to give style to multiple elements. For example, go to https://xkcd.com/. Notice how there are elements like "boxes" or "buttons" that are styled similarly. Instead of defining the style for each one of these elements, the style for all the "boxes" might be defined in a different script (a CSS document), and it just points to all elements with class = "box". This is often a useful way to locate content inside of an HTML script.

Web scraping sometimes feels like searching for a book in a library: information is well structured and tagged. Others, it feels like searching for gold nuggets in a dumpster. Assessing whether is possible or not to access the data and being able to estimate the difficulty of the task ahead of time are two crucial skills when webscraping.

Understanding the HTML tree structure is key to webscraping, but that doesn't mean we should know what each tag or attribute is supposed to do. It might help to get the logic underlying that site, but ultimately we don't care much if the data we're looking for is under a <div> or a <li> tag, we just have to detect it and write the code that grabs it.

</details>


:coffee: __BREAK__

---

#### :pencil2: Check for Understanding - Class activity/quick quiz
> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>
  
Each student must find one website on the internet that would be cool to scrape, either for a personal project or with a business use case. They should share this site on slack. Go through the sites they suggested, ask them what would they do with the data and debate if it would be feasable, legal and ethical to do so.

</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

Not an actual solution, but you can suggest these sites:

- **Wikipedia:** Almost infinite content, really accessible and most of the time, well structured. For example, each President of the USA has a table with the same items: time in office, personal details... it's reasonable to assume we can easily extract infromation about all the presidents with an automated script.

- **Amazon:** It's easy to come up with scenarios in which having information contained in Amazon is useful. Tracking prices and reviews to products there can be a huge advantage for a company selling similar products there or elsewhere.

</details>

---

:coffee: __BREAK__

---


### Lesson 2 key concepts
> :clock10: 20 min

- Sme basic beautifulsoup functions to extract info from a simple html script:
    - Creating the soup (parsing the html)
    - Navigating the html tree
    - find_all() and select()

<details>
  <summary> Basic beautifulsoup functions </summary>

The first step when web scraping is to download the html code from the website you want to collect data from.

However, real html code from websites is usually long and complex. We will skip this step for now, and start directly using beautifulsoup with a very simple HTML script, just to get used to an HTML document and get a sense of what it is to collect information from it.

If you want, you can open the document "DormouseStory.html" on the web browser and show them how would this "website" look like.

This lesson is a code-along. It's quite simple, so it's encouraged students really code themselves. Share with them the following code containing a simple HTML script that will be stored in a variable:

```python

html_doc = """
<!DOCTYPE html>
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
</html>
"""

```

Go through the code below with your students and comment on the following syntax of beautifulsoup:

- The first step to web-scraping is "parsing". That makes the html code readable in for us and the python interpreter. Doing that is sometimes called "creating the soup", since the convention is to store the parsed html code in a variable called `soup`.

- You can navigate down the "html tree of tags" using `parent.children`.

- The most popular method in beautifulsoup is `find_all()`. Simply gets all elements belonging to a tag.

- If you want to get the value of an attribute, you can use `element.get(attributeName)`. It's specially useful to get links, as they're always the values of the attribute `href`. You can also use `element[attributeName]` to achieve the same results.

- `get_text()` extracts the content of a tag.

```python

html_doc

# parse the element
soup = BeautifulSoup(html_doc, 'html.parser')

# html well indented. not always works great...
print(soup.prettify())

# basic tree navigation
soup.title

soup.title.name

soup.title.string

soup.title.parent.name

soup.p

soup.p["class"]


# get all elements of a tag
soup.find_all("p")


soup.find_all("a")[0].get("href")

# get links
for link in soup.find_all('a'):
    print(link.get('href'))

# exactly the same results as before:
for link in soup.find_all('a'):
    print(link['href'])

# extract content
print(soup.get_text())

for a in soup.find_all('a'):
    print(a.get_text())


```

- **CSS Selectors**: besides beautifulsoup own functions, namely the find_all() method, the library offers an alternative method, select(), to locate information in the html tree using the simple synthax of CSS Selectors.

Go to https://flukeout.github.io/ and play with the students through the first few levels of the game so that they understand how CSS selectors work (up to level 6 should be enough, encourage them to get to level 32 on their free time).

Go back to your notebook and show them how you can use select() instead of find_all()

```python

soup.select("a")

for a in soup.select('a'):
    print(a.get_text())

soup.select(".title")

soup.select(".sister")

soup.select("p.story")[1].get_text()


```

</details>


#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 20 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

Send your students the html script below and this question:

Write code to print the following contents (not including the html tags, only human-readable text):
1. All the "fun facts".
2. The names of all the places.
3. The content (name and fact) of all the cities (only cities, not countries!)
4. The names (not facts!) of all the cities (not countries!)

Students will have to search for themselves how to get elements by class. They can use either find_all() or select(). Chances are they will stumble upon this post on Stackoverflow: https://stackoverflow.com/questions/5041008/how-to-find-elements-by-class

```html
<!DOCTYPE html>
<html>
<head> Geography</head>
<body>

<div class="city">
  <h2>London</h2>
  <p>London is the most popular tourist destination in the world.</p>
</div>

<div class="city">
  <h2>Paris</h2>
  <p>Paris was originally a Roman City called Lutetia.</p>
</div>

<div class="country">
  <h2>Spain</h2>
  <p>Spain produces 43,8% of all the world's Olive Oil.</p>
</div>

</body>
</html>
```

</details>

<details>
  <summary> Activity 2 solutions</summary>

```python
# Creating the soup
html_doc2 = """<!DOCTYPE html>
<html>
<head> Geography</head>
<body>

<div class="city">
  <h2>London</h2>
  <p>London is the most popular tourist destination in the world.</p>
</div>

<div class="city">
  <h2>Paris</h2>
  <p>Paris was originally a Roman City called Lutetia.</p>
</div>

<div class="country">
  <h2>Spain</h2>
  <p>Spain produces 43,8% of all the world's Olive Oil.</p>
</div>

</body>
</html>"""

soup = BeautifulSoup(html_doc2, 'html.parser')

print(soup.prettify())

# 1. All the "fun facts"
for i in soup.find_all("p"):
    print(i.get_text())

# 2. The names of all the places.
for i in soup.find_all("h2"):
    print(i.get_text())

# 3. All the content (name and fact) of all the cities (only cities, not countries!)
for i in soup.find_all("div", {"class":"city"}):
    print(i.get_text())

# 4. The names (not facts!) of all the cities (not countries!)
for i in soup.find_all("div", {"class":"city"}):
    print(i.h2.get_text())

```
---


:coffee: __BREAK__

---

### Lesson 3 key concepts
> :clock10: 20 min

- Inspecting the code of a site using Chrome's Inspector
- Downloading the code of a site using the requests library
- Collecting data from a real website

<details>
<summary> Chrome inspector </summary>

**Use case:**
Let's go to https://www.imdb.com/chart/top, where we'll see the top 250 movies according to IMDb ratings.

Notice how each movie has the following elements:

- Title
- Release Year
- IMDb rating
- Director & main stars (they appear when you hover over the title)

Our objective in the following 2 lessons is going to be to scrape this information and store it in a pandas dataframe.


**Chrome Dev Tools:**

- Right click on the browser and go to "inspect" (or use Cmd+Shift+C) to open the Dev Tools.

- Make sure you're on the "Elements" panel.

- Show to students how they can hover the mouse over each line of the HTML code and see how the corresponding elements in the site light up.

- On the top left corner of the inspector, click on the icon that has a mouse pointer on top of a square (or click Command+Shift+C): now, you can do the opposite as before: hover the mouse over the elements on the website and see how the corresponding code lights up. If the code is hidden in the tree, you can click on the element in the site and the tree will unfold.

- When you click on an element, look at the bar at the botom of the HTML inspector (ignor the Styling pane). You'll find the path of tags, id's and classes.
    For example, if you click on the title "Top Rated Movies" you'll see how the path includes the following tags, id's and classes:

    `html #styleguide-v2 #wrapper #root #pagecontent div #content-2-wide #main div span div.seen-collection div.article h1.header`

    This path shows all the branches of the html tree leading to this particular element.

    Fortunately, there's an easier way than following this tree. You can also right click on the html element (in the script) and go to Copy > Copy Selector. This will give you a simplified path of CSS Selectors you can use to select this element:

    `#main > div > span > div > div > h1`

- Go to the title of the movie ranked #1 and we copy the selector, we should get:

    `#main > div > span > div > div > div.lister > table > tbody > tr:nth-child(1) > td.titleColumn > a`

    Paste this somewhere, as we'll need it later when we're searching the html from python.

- Look below that element on the html script and find the rest of the data for that movie. You can copy now the selector paths or do it later.

Inspecting the elements you want to scrape is a common first step before actually trying to get them from python. Now, we'll switch to a Jupyter Notebook and get started with scraping. Code along with your students:

```python
# 1. import libraries
from bs4 import BeautifulSoup
import requests
import pandas as pd

# 2. find url and store it in a variable
url = "https://www.billboard.com/charts/hot-100"

# 3. download html with a get request
response = requests.get(url)
response.status_code # 200 status code means OK!

# 4.1. parse html (create the 'soup')
soup = BeautifulSoup(response.content, "html.parser")
# 4.2. check that the html code looks like it should
soup

# 5. retrieve/extract the desired info (here, you'll paste the "Selector" you copied before to get the element that belongs to the top movie)

soup.select("#main > div > span > div > div > div.lister > table > tbody > tr:nth-child(1) > td.titleColumn")

```
Output:
```python
[<td class="titleColumn">
       1.
       <a href="/title/tt0111161/" title="Frank Darabont (dir.), Tim Robbins, Morgan Freeman">Cadena perpetua</a>
 <span class="secondaryInfo">(1994)</span>
 </td>]
```

This long selector we copied is kind of long and ugly, isn't it? And it only selects one single movie, while we will want to collect data from all of them. Going from that particular selector to one that's more "general" and "elegant" is the actual work the web scraper needs to do.

In this case, we can play around a bit with different tags and classes, until we notice that all the information about the movies is under the tag <td class="titleColumn">. We're lucky that under this tag there's not much "trash", just the info we need.

```python
soup.select("td.titleColumn") # all the info about all the movies

soup.select("td.titleColumn a") # all elements containing movie titles

# we can use .get_text() to extract the content of the tags we selected
# we'll need to do it to each tag with a for loop: here we do it to the first one
soup.select("td.titleColumn a")[0]
soup.select("td.titleColumn a")[0].get_text()

# the director and main stars are in the same tag, but as a value of the attribute "title"
# we can access attributes as key-value pairs of dictionaries: using ["key"] to get the value:
soup.select("td.titleColumn a")[0]["title"]
# instead of ["title"] we could use .get("title"): choose whatever you prefer

# the years are inside a 'span' tag with the 'secondaryInfo' class
# we also specify the parent tag and its class, which is the same we used before
# the years are inside parentheses, but we'll take care of that later
soup.select("td.titleColumn span.secondaryInfo")[0].get_text()

```

</details>


---

#### :pencil2: Check for Understanding - Class activity/quick quiz
> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

xxxxx

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

```python

```

</details>

---

:coffee: __BREAK__

---

### Lesson 4 key concepts
> :clock10: 20 min

- Getting the scraped info into a dataframe
- Data cleaning & wrangling
- Introducing the use case for our music project.
- Getting started with scraping more complex websites.


<details>
  <summary> Scraped info into a dataframe </summary>

We already know how to extract all the pieces of info for each movie individually.  Now, we will:

- Build a for loop to extract them all and store them in lists.
- Create a dataframe where each one of the lists you created is a column

The expected output we aim for is a pandas dataframe with 3 columns, called "title", "dir_stars" and "year", each one containing all 200 elements for each movie.

Code along with your students:

```python
#initialize empty lists
title = []
dir_stars = []
year = []

# define the number of iterations of our for loop 
# by checking how many elements are in the retrieved result set
# (this is equivalent but more robust than just explicitly defining 250 iterations)
num_iter = len(soup.select("td.titleColumn a"))

# iterate through the result set and retrive all the data
for i in range(num_iter):
    title.append(soup.select("td.titleColumn a")[i].get_text())
    dir_stars.append(soup.select("td.titleColumn a")[i]["title"])
    year.append(soup.select("td.titleColumn span.secondaryInfo")[i].get_text())
    
print(title)
print(dir_stars)
print(year)

# each list becomes a column
movies = pd.DataFrame({"title":title,
                       "dir_stars":dir_stars,
                       "year":year
                      })

movies.head()

```

An inherent part of web scraping is data cleaning. We managed to get the information we needed, but for it to be useful, we still need some extra steps:

- Take the year out of the parentheses: we know we can totally do that with regex, but string methods such as str.replace() might be simpler to use.

- Change the data type of the year column to integer.

- Split dir_stars into 3 columns, one for each person: "director", "star_1", "star_2". This could have been done by filtering when extracting the data from the html document, but it looks easier afterwards: 
    - The "(dir.)" pattern can be totally removed
    - We can split the string at each comma

This is simpler to do on lists than it is on dataframes, so we'll go back to the lists we have and then we'll assemble again the dataframe.

If your students are capable of doing this on their own, give them some time to try it on their own before the code along (like an activity).

In the code along, you might want to go step by step building the loop, but here's the completed script:

```python

director = []
star_1 = []
star_2 = []

for movie in dir_stars:
    crew = movie.split(",")
    director.append(crew[0].replace(" (dir.)", ""))
    star_1.append(crew[1])
    star_2.append(crew[2])

# each list becomes a column
movies = pd.DataFrame({"title":title,
                       "director":director,
                       "star_1":star_1,
                       "star_2":star_2
                      })

movies.head()

```
</details>


### :pencil2: Practice on key concepts - Lab
> :clock10: 30 min 

<details>
  <summary> Click for Instructions: Lab </summary>

```python
TBD
```

</details>

<details>
  <summary>Click for Solution: Lab solutions</summary>

```python
TBD
```

</details>

---

:sandwich: __LUNCH BREAK__

---