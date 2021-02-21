
# Lesson 8.1

### Lesson Duration: 3 hours

> Purpose: The purpose of this lesson is  to introduce the business case study of the music segmentation of a company. In this lesson we will try to understand what an API is and how can we connect to some of them. We will also take a look at the main API we will be working on, the Spotify API.

---

### Learning Objectives: 
After this lesson, students will be able to: 

- Understand what an API is
- HTTP Protocol and requests library
- Access to APIs with python
- JSON format ovreview
- API Wrappers
- First connection to the SpotiPy API library
--- 

### Lesson 1 key concepts
> :clock10: 20 min

- Understand what an API is
- RESTful APIs

<details>
<summary> Click for Description: Introduction to Case study </summary>

- Our company *<company_name>* is interested in grouping clients by their musical interests. To do that we will need to collect data from musical sites so we can understand better this topic. We have heard that the spotify **API** is pretty good for this kind of data collection so we will take a look at it. If it's note enough or we need more information, we will try to use other data collection methods in later lessons (**web sacraping**)

</details>

<details>
  <summary>Click for Description: What is an API?</summary>

- An **API** (*Application Programming Interface*) is a type of intermediary software that allows two aplications to talk to each other and communicate between them. Everytime you use Youtube or Wikipedia you're also using an API.

- When you use an application it connects to the Internet and sends the required information to a server. The server then retrieves that data, interprets it, performs the necessary actions and calculations so it can send the desired information / action to your phone. The application then interprets the data and presents it to you in a readable way. All of this happens via APIs. 

- You can imagine the API as a waiter in a restaurant. The guest (application) was some food (data) and the chef (server) can cook for him (give data to the application). But instead of letting the person ask directly what he wants to the chef (what would make him mental, having all the clients talking to him at the same time) there is a procedure, a helper, in the restaurant there is a waiter that helps with the commands and carry the food from one side to another (app -server). This is exactly what an API is.

</details>
<summary> Click for Description: Why do we use APIs? </summary>

- Sometimes it is better to use a simple CSV file instead, but APIs are useful in the following cases:

    -  **The data changes quickly**. Stock price data or betting houses are a perfect example for it. It doesn’t really make sense to regenerate a dataset and download it every second. It is incredibly expensive and wouldn't be efficient nor effective at all, as it would be not just expensive but also really slow.
    
    -    **You want just a piece of all your data**. Imagine you want to download your facebook pictures. Without an API you would need to download the entire Facebook dataset, and that doesn't really make a lot of sense since we have APIs to filter that information. 

    -    **Repeated computation involved**. The Spotify API that can tell you the genre of a piece of music. You could theoretically create your own classifier, and use it to compute music categories, but you’ll never have as much data as Spotify does, saving a lot of space.

Overall we can say that the main advantages of using APIS are **Automation**, **Efficiency** and **Adaptation**.

There are many ways in which web services can be organized but the most popular way is to make a REST (**RE**presentational **S**tate **T**ransfer) API.

REST defines 6 architectural constraints which make any web service a RESTful API. You can find them [here](https://www.geeksforgeeks.org/rest-api-architectural-constraints/#:~:text=The%20only%20optional%20constraint%20of,API%20and%20Non-REST%20API.&text=For%20example%3A%20API%2Fusers.). (These concepts can be explained to more advanced classes, added as **optional** content)


This is important because it generalizes the use of **HTTP**, making **requests** to specific **URLs**. It is the tool we will be using when interacting with an API.

Like a python object, HTTP  has different methods, including the most important ones **GET** and **POST**. We will take a look at it in the next lesson.


---

:coffee: __BREAK__

---

#### :pencil2: Check for Understanding - Class activity/quick quiz
> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 1 </summary>
  

[Kahoot](kahoot.com) - Sli.do activity:

- 1º What does the acronym API stand for?
-- *Application Programming Interface*
-- *Application Procedure Interface*
-- *Authentication Procedure Interface*
-- *Authentication Programming Interpreter*

- 2º When won't we have an API for an specific software?
-- *The software hasn't been updated since 2015*
-- *When it is not specifically developed*
-- *The software is banned from the US*
-- *HTTP does not give its permission*

- 3º What are the main benefits of using an API?
--  *Automation, Efficiency and Adaptation*
--  *Automation, Effectiveness and Adaptation*
-- *Automation, Effectiveness and Adeption*
-- *Accesibility, Effectiveness and Adaptation*

- 4º What is the most popular way of organizing an API?
-- JSON
-- HTTP
-- HTTPS
-- REST

- 5º What protocol do we use when interacting with a RESTful API?
-- JSON
-- HTTP
-- HTTPS
-- REST


</details>

<details>
  <summary>Click for Solution: Activity 1 solutions</summary>

- 1º Application Programming Interface

- 2º Just when it's not developed, there is no other constraint

- 3º  Automation, Efficiency and Adaptation

- 4º REST

- 5º HTTP

</details>

---

:coffee: __BREAK__

---


### Lesson 2 key concepts
> :clock10: 20 min

- We will start working with APIs in the next lesson but first we need to get in touch with the tools that will make the connection possible as well as an overview of the **REST**ful way. 

- The first tool we will take a look at will be the **requests** library, that's meant to cope with **HTTP** and watch out the status code.

- We will also see some files in **JSON** format to understand its structure and be able to use them

-- Now let's start by installing the request library and importing it as well doing a request get with the following websites: [Google](https://developers.google.com), [NBA]() and [Rotten Tomatoes](). Let's check their `status_code` by using the requests method **GET**, which simply indicates that you're trying to retrieve information from the data source:

```python
#Install requests library with the command promt

# Import requests library 
import requests

google = requests.get("https://developers.google.com")
print("Google:", google.status_code)

NBA = response = requests.get("https://api.sportsdata.io/api/nba/fantasy/json/CurrentSeason")
print("NBA:", NBA.status_code)

rotten_tomato = requests.get("http://api.rottentomatoes.com/api/public/v1.0/lists/movies/box_office.json")
print("Rotten Tomatoes:", rotten_tomato.status_code)
```

Output:
```python
Google: 200
NBA: 401
Rotten Tomatoes: 403
```

So we can see we receive some **status codes**, the most frequent are these:

-   `200`: Everything went okay and the result has been returned (if any).
-   `301`: The server is redirecting you to a different endpoint. This can happen when a company switches domain names, or an endpoint name is changed.
-   `400`: The server thinks you made a bad request. This happens when you don’t send along the right data, among other things.
-   `401`: You are not properly authenticated. 
-   `403`: The resource you’re trying to access is forbidden: you don’t have the right permissions to get it.
-   `404`: The resource you tried to access doesn't exist.
-   `503`: The server can't handle the request.
</details>

<details>
  <summary> After connecting </summary>

- As we are connected now to the google developer endpoint let's try to get some data from it


```python
print(google.text)
```
This is not the proper format for us right now so let's try to tranform it to the one we need, the **JSON** (JavaScript Object Notation) format, which is the language of RESTful APIs. It is the way to encode data to make data easily readable by machines.
```python
google.json()
```
Output
```python
JSONDecodeError: Expecting value: line 1 column 1 (char 0)
```
The problem here is that we are not connected to a proper API that has a json as the endpoint, so we will need to get another valid URL. Remember that the code `200`means that the connection has been established, but not that it is the connection you need. Let's try with the [skyscrapper]() API .

We will connect to this API through the web [RapidAPI](https://rapidapi.com/skyscanner/api/skyscanner-flight-search) so you will need to create an account to get a personal **Key** (user ID) to have access to it, just sign up and get your Key.
The "problem" with this API is that we will need to pass some **parameters**, including the **headers** and the **city** following this code:
```python
url = "https://skyscanner-skyscanner-flight-search-v1.p.rapidapi.com/apiservices/autosuggest/v1.0/UK/GBP/en-GB/"

params = {"query":"Tokyo"}

headers = {'x-rapidapi-host': "skyscanner-skyscanner-flight-search-v1.p.rapidapi.com",
                      'x-rapidapi-key': "<introduce your RapidAPI key here>"}

response = requests.get(url, headers = headers, params = params)
response.json()
```
Output:
```python
{"Places":[{"PlaceId":"TYOA-sky","PlaceName":"Tokyo","CountryId":"JP-sky","RegionId":"","CityId":"TYOA-sky","CountryName":"Japan"},{"PlaceId":"NRT-sky","PlaceName":"Tokyo Narita","CountryId":"JP-sky","RegionId":"","CityId":"TYOA-sky","CountryName":"Japan"},{"PlaceId":"HND-sky","PlaceName":"Tokyo Haneda","CountryId":"JP-sky","RegionId":"","CityId":"TYOA-sky","CountryName":"Japan"},{"PlaceId":"TJH-sky","PlaceName":"Toyooka","CountryId":"JP-sky","RegionId":"","CityId":"JTJH-sky","CountryName":"Japan"},{"PlaceId":"OOK-sky","PlaceName":"Toksook Bay","CountryId":"US-sky","RegionId":"AK","CityId":"OOKA-sky","CountryName":"United States"},{"PlaceId":"TKZ-sky","PlaceName":"Tokoroa","CountryId":"NZ-sky","RegionId":"","CityId":"TKZN-sky","CountryName":"New Zealand"}]}
```
To know the parameters we need to be able to connect properly to an API we should look first at its documentation. For the next query we will look at the prices for the flights from San Francisco to New York City in 2020/12/12

```python
url = "https://skyscanner-skyscanner-flight-search-v1.p.rapidapi.com/apiservices/browsequotes/v1.0/US/USD/en-US/SFO-sky/NYCA-sky/2020-12-12"

params = {"inboundpartialdate":"2020-12-12"}

headers = {
    'x-rapidapi-host': "skyscanner-skyscanner-flight-search-v1.p.rapidapi.com",
    'x-rapidapi-key': "<introduce your RapidAPI key here>"}

response = requests.get(url, headers=headers, params=params)

response.json()
```
Output:
```python
{'Quotes': [{'QuoteId': 1,
   'MinPrice': 92.0,
   'Direct': False,
   'OutboundLeg': {'CarrierIds': [1065],
    'OriginId': 81727,
    'DestinationId': 50290,
    'DepartureDate': '2020-12-12T00:00:00'},
   'QuoteDateTime': '2020-09-21T10:49:00'},
  {'QuoteId': 2,
   'MinPrice': 133.0,
   'Direct': True,
   'OutboundLeg': {'CarrierIds': [851],
    'OriginId': 81727,
    'DestinationId': 50290,
    'DepartureDate': '2020-12-12T00:00:00'},
   'QuoteDateTime': '2020-09-21T10:49:00'}],
 'Places': [{'PlaceId': 50290,
   'IataCode': 'EWR',
   'Name': 'New York Newark',
   'Type': 'Station',
   'SkyscannerCode': 'EWR',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 60987,
   'IataCode': 'JFK',
   'Name': 'New York John F. Kennedy',
   'Type': 'Station',
   'SkyscannerCode': 'JFK',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 65633,
   'IataCode': 'LGA',
   'Name': 'New York LaGuardia',
   'Type': 'Station',
   'SkyscannerCode': 'LGA',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 81727,
   'IataCode': 'SFO',
   'Name': 'San Francisco International',
   'Type': 'Station',
   'SkyscannerCode': 'SFO',
   'CityName': 'San Francisco',
   'CityId': 'SFOA',
   'CountryName': 'United States'}],
 'Carriers': [{'CarrierId': 819, 'Name': 'Aegean Airlines'},
  {'CarrierId': 851, 'Name': 'Alaska Airlines'},
  {'CarrierId': 870, 'Name': 'jetBlue'},
  {'CarrierId': 1065, 'Name': 'Frontier Airlines'},
  {'CarrierId': 1721, 'Name': 'Sun Country Airlines'},
  {'CarrierId': 1793, 'Name': 'United'},
  {'CarrierId': 1902, 'Name': 'Southwest Airlines'}],
 'Currencies': [{'Code': 'USD',
   'Symbol': '$',
   'ThousandsSeparator': ',',
   'DecimalSeparator': '.',
   'SymbolOnLeft': True,
   'SpaceBetweenAmountAndSymbol': False,
   'RoundingCoefficient': 0,
   'DecimalDigits': 2}]}
```

We will learn what the JSON format really is in the next lesson, for now just try to go with the activity

</details>
#### :pencil2: Check for Understanding - Class activity/quick quiz

> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 2 </summary>

Connect yourself to the **SkyScanner** API and extract information from all the flights flying from Munich to Berlin. To do that you will need to get their city ID which you can find in the first query we used in the lesson. In this activity you will just need to create a function called `city_code` that takes a `city_name`(*string-like*, *"Mumbai"*) as a parameter and returns the city flight code.

</details>

<details>
  <summary>Click for Solution: Activity 2 solutions</summary>

```python
def city_code(city_name):
    url = "https://skyscanner-skyscanner-flight-search-v1.p.rapidapi.com/apiservices/autosuggest/v1.0/US/USD/en-US/"

    params = {"query": city_name}

    headers = {'x-rapidapi-host': "skyscanner-skyscanner-flight-search-v1.p.rapidapi.com",
                      'x-rapidapi-key': "<introduce your RapidAPI key here>"}

    response = requests.get(url, headers = headers, params = params)

    return response.json()["Places"][0]["PlaceId"]
```

</details>

---


:coffee: __BREAK__

---

### Lesson 3 key concepts
> :clock10: 20 min

- What is JSON?
- Understanding JSON structure 
- Applying it to the skyscanner API 


JSON (*JavaScript Object Notation*) is a standard **text-based** format for representing **structured data** (such as dictionaries) based on JavaScript object syntax. JSON exists as a **string**, something very useful when you want to transmit data across a network.

<details>
<summary> What's the JSON structure? </summary>

- To understand how JSON structure works we will look at the previous output.

```python
{'Quotes': [{'QuoteId': 1,
   'MinPrice': 92.0,
   'Direct': False,
   'OutboundLeg': {'CarrierIds': [1065],
    'OriginId': 81727,
    'DestinationId': 50290,
    'DepartureDate': '2020-12-12T00:00:00'},
   'QuoteDateTime': '2020-09-21T10:49:00'},
  {'QuoteId': 2,
   'MinPrice': 133.0,
   'Direct': True,
   'OutboundLeg': {'CarrierIds': [851],
    'OriginId': 81727,
    'DestinationId': 50290,
    'DepartureDate': '2020-12-12T00:00:00'},
   'QuoteDateTime': '2020-09-21T10:49:00'}],
 'Places': [{'PlaceId': 50290,
   'IataCode': 'EWR',
   'Name': 'New York Newark',
   'Type': 'Station',
   'SkyscannerCode': 'EWR',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 60987,
   'IataCode': 'JFK',
   'Name': 'New York John F. Kennedy',
   'Type': 'Station',
   'SkyscannerCode': 'JFK',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 65633,
   'IataCode': 'LGA',
   'Name': 'New York LaGuardia',
   'Type': 'Station',
   'SkyscannerCode': 'LGA',
   'CityName': 'New York',
   'CityId': 'NYCA',
   'CountryName': 'United States'},
  {'PlaceId': 81727,
   'IataCode': 'SFO',
   'Name': 'San Francisco International',
   'Type': 'Station',
   'SkyscannerCode': 'SFO',
   'CityName': 'San Francisco',
   'CityId': 'SFOA',
   'CountryName': 'United States'}],
 'Carriers': [{'CarrierId': 819, 'Name': 'Aegean Airlines'},
  {'CarrierId': 851, 'Name': 'Alaska Airlines'},
  {'CarrierId': 870, 'Name': 'jetBlue'},
  {'CarrierId': 1065, 'Name': 'Frontier Airlines'},
  {'CarrierId': 1721, 'Name': 'Sun Country Airlines'},
  {'CarrierId': 1793, 'Name': 'United'},
  {'CarrierId': 1902, 'Name': 'Southwest Airlines'}],
 'Currencies': [{'Code': 'USD',
   'Symbol': '$',
   'ThousandsSeparator': ',',
   'DecimalSeparator': '.',
   'SymbolOnLeft': True,
   'SpaceBetweenAmountAndSymbol': False,
   'RoundingCoefficient': 0,
   'DecimalDigits': 2}]}
```

As we mentioned before, the structure shares some similarities with a python dictionary, that's due to how the data is structured overall, and at the same time we can think of them about dataframes. For any **key** (or column) of the json / dictionary, there will be one or more **values** (or rows). Let's transform the previous output into a pandas dataframe, for that we will need to import **pandas** and use the method `json_normalize` to automatically transform the file into a DataFrame:
</details>
<details>
  <summary> Click for Code Sample </summary>

```python
# Import libraries
import pandas as pd
import json

# Normalize the response

pd.json_normalize(response.json())
# If this doesn't work, try to import json_normalize directly from pandas.io.json
```
As an output we will have a DataFrame with 4 columns, **Quotes, Places, Carriers** and **Currencies**, but the values will also be in JSON format. From this we can infere that we are not working with a dataset, but a relational database, and the columns are the datasets we were looking for. Let's transform them into `pd.DataFrames`:
```python
quotes = pd.DataFrame(json_normalize(response.json())["Quotes"][0])
carriers  pd.DataFrame(json_normalize(response.json())["Carriers"][0])
places = pd.DataFrame(json_normalize(response.json())["Places"][0])
currencies = pd.DataFrame(json_normalize(response.json())["Currencies"][0])
```
After reviewing and printing our data we can conclude that quotes are the flights we have for our selected origin and destination (just one in this case), places are the airports we could travel from and to, carriers are the companies usually have the kind of trip we have chosen, and currency is self-explanatory.

However we still have another json in one of the datasets, in **Quotes**, variable `["OutboundLeg"]`, that's due to the same principle we have been working for during this lesson, relational databases. Let's unfold that variable:
```python
flights = pd.DataFrame(pd.DataFrame(json_normalize(response.json())["Quotes"][0])["OutboundLeg"][0])
```
</details>

#### :pencil2: Check for Understanding - Class activity/quick quiz
> :clock10: 10 min (+ 10 min Review)

<details>
  <summary> Click for Instructions: Activity 3 </summary>

Create a function called `flight_prices`that takes `departure` (as the origin city name, "Mumbai"), `arrival`(as the destination city name, "Istanbul") and `date`. It should return an output similar to  the one we had for San Francisco - New York. You may want to check if it works with these param values:

* Departure: "Berlin"
* Arrival: "Vienna"
* Date: "2020-10-03" (The format will help but remember that it has to be an upcoming date)

</details>

<details>
  <summary>Click for Solution: Activity 3 solutions</summary>

```python
def flight_prices(departure, arrival, date):
    
    departure_code = city_code(departure)
    arrival_code = city_code(arrival)
    url = f"https://skyscanner-skyscanner-flight-search-v1.p.rapidapi.com/apiservices/browsequotes/v1.0/US/USD/en-US/{departure_code}/{arrival_code}/{date}"

    params = {"inboundpartialdate":{date}}

    headers = {
    'x-rapidapi-host': "skyscanner-skyscanner-flight-search-v1.p.rapidapi.com",
    'x-rapidapi-key': "<introduce your RapidAPI key here>"}

    response = requests.get(url, headers=headers, params=params)

    return response.json()
```

</details>

---

:coffee: __BREAK__

---

### Lesson 4 key concepts
> :clock10: 20 min
- Spotify API
---


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