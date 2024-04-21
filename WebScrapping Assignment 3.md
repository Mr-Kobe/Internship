```python
import requests
from bs4 import BeautifulSoup

def search_amazon(product):
    # Define the URL based on user input
    url = f"https://www.amazon.in/s?k={product}"

    # Send a GET request to the URL
    response = requests.get(url)

    # Parse the HTML content
    soup = BeautifulSoup(response.content, 'html.parser')

    # Find all the product containers
    product_containers = soup.find_all('div', {'data-component-type': 's-search-result'})

    # Loop through each product container and extract relevant information
    for product in product_containers:
        # Extract product title
        title = product.find('span', {'class': 'a-text-normal'}).text.strip()

        # Extract product price
        price_tag = product.find('span', {'class': 'a-price'})
        price = price_tag.find('span', {'class': 'a-offscreen'}).text.strip() if price_tag else '-'

        # Extract product rating
        rating_tag = product.find('span', {'class': 'a-icon-alt'})
        rating = rating_tag.text.strip() if rating_tag else '-'

        # Print the extracted information
        print("Title:", title)
        print("Price:", price)
        print("Rating:", rating)
        print("\n")

# Take user input for the product to search
search_query = input("Enter the product you want to search for on Amazon.in: ")

# Call the function to search Amazon for the specified product
search_amazon(search_query)

```

    Enter the product you want to search for on Amazon.in: guitars
    


```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

def scrape_product_details(product):
    # Create an empty DataFrame to store product details
    product_df = pd.DataFrame(columns=["Brand Name", "Name of the Product", "Price", "Return/Exchange", "Expected Delivery", "Availability", "Product URL"])

    # Iterate over the first 3 pages of search results
    for page_number in range(1, 4):
        # Define the URL for the search results page
        url = f"https://www.amazon.in/s?k={product}&page={page_number}"

        # Send a GET request to the URL
        response = requests.get(url)

        # Parse the HTML content
        soup = BeautifulSoup(response.content, 'html.parser')

        # Find all the product containers
        product_containers = soup.find_all('div', {'data-component-type': 's-search-result'})

        # Loop through each product container and extract details
        for product in product_containers:
            # Extract product details
            brand_name = product.find('span', {'class': 'a-size-base-plus a-color-base'}).text.strip() if product.find('span', {'class': 'a-size-base-plus a-color-base'}) else '-'
            product_name = product.find('span', {'class': 'a-text-normal'}).text.strip() if product.find('span', {'class': 'a-text-normal'}) else '-'
            price = product.find('span', {'class': 'a-offscreen'}).text.strip() if product.find('span', {'class': 'a-offscreen'}) else '-'
            return_exchange = product.find('div', {'class': 'a-row a-size-base a-color-secondary'}).text.strip() if product.find('div', {'class': 'a-row a-size-base a-color-secondary'}) else '-'
            expected_delivery = product.find('span', {'class': 'a-text-bold'}).text.strip() if product.find('span', {'class': 'a-text-bold'}) else '-'
            availability = product.find('span', {'class': 'a-size-base'}).text.strip() if product.find('span', {'class': 'a-size-base'}) else '-'
            product_url = 'https://www.amazon.in' + product.find('a', {'class': 'a-link-normal'})['href'] if product.find('a', {'class': 'a-link-normal'}) else '-'

            # Append the details to the DataFrame
            product_df = product_df.append({"Brand Name": brand_name,
                                            "Name of the Product": product_name,
                                            "Price": price,
                                            "Return/Exchange": return_exchange,
                                            "Expected Delivery": expected_delivery,
                                            "Availability": availability,
                                            "Product URL": product_url}, 
                                            ignore_index=True)

    return product_df

# Take user input for the product to search
search_query = input("Enter the product you want to search for on Amazon.in: ")

# Scrape product details
product_data = scrape_product_details(search_query)

# Save the data to a CSV file
product_data.to_csv("amazon_product_details.csv", index=False)

print("Product details scraped and saved successfully!")

```

    Enter the product you want to search for on Amazon.in: guitar
    Product details scraped and saved successfully!
    


```python
#Using Selenium to automate the Chrome web browser
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time
import os
import requests
from bs4 import BeautifulSoup

def scrape_images(keyword, num_images):
    # Create a folder to save the images
    folder_path = f"{keyword}_images"
    os.makedirs(folder_path, exist_ok=True)

    # Initialize Chrome web browser
    driver = webdriver.Chrome()

    # Open images.google.com
    driver.get("https://images.google.com/")
    time.sleep(2)

    # Find the search bar
    search_bar = driver.find_element_by_name("q")
    
    # Input the keyword
    search_bar.send_keys(keyword)

    # Hit Enter to perform the search
    search_bar.send_keys(Keys.ENTER)
    time.sleep(2)

    # Find and click on the "Images" tab
    images_tab = driver.find_element_by_link_text("Images")
    images_tab.click()
    time.sleep(2)

    # Scroll down to load more images
    last_height = driver.execute_script("return document.body.scrollHeight")
    while len(driver.find_elements_by_xpath("//img")) < num_images:
        driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
        time.sleep(2)
        new_height = driver.execute_script("return document.body.scrollHeight")
        if new_height == last_height:
            break
        last_height = new_height

    # Get image URLs
    image_urls = []
    soup = BeautifulSoup(driver.page_source, "html.parser")
    for img in soup.find_all("img", class_="rg_i"):
        if len(image_urls) == num_images:
            break
        img_url = img.get("src")
        if img_url:
            if img_url.startswith("data:image"):
                img_url = img.get("data-src")
            image_urls.append(img_url)

    # Download images
    for i, url in enumerate(image_urls):
        response = requests.get(url)
        with open(f"{folder_path}/{keyword}_{i+1}.jpg", "wb") as f:
            f.write(response.content)

    # Close the browser
    driver.quit()

    print(f"{num_images} images for '{keyword}' scraped and saved successfully!")

# Keywords and number of images to scrape
keywords = ["fruits", "cars", "Machine Learning", "Guitar", "Cakes"]
num_images_per_keyword = 10

# Scrape images for each keyword
for keyword in keywords:
    scrape_images(keyword, num_images_per_keyword)

```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Cell In[5], line 75
         73 # Scrape images for each keyword
         74 for keyword in keywords:
    ---> 75     scrape_images(keyword, num_images_per_keyword)
    

    Cell In[5], line 22, in scrape_images(keyword, num_images)
         19 time.sleep(2)
         21 # Find the search bar
    ---> 22 search_bar = driver.find_element_by_name("q")
         24 # Input the keyword
         25 search_bar.send_keys(keyword)
    

    AttributeError: 'WebDriver' object has no attribute 'find_element_by_name'



```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time

def get_coordinates(city):
    # Initialize Chrome web browser
    driver = webdriver.Chrome()

    # Open Google Maps
    driver.get("https://www.google.com/maps")
    time.sleep(2)

    # Find the search bar using XPath
    search_bar = driver.find_element_by_xpath("//input[@id='searchboxinput']")
    
    # Input the city
    search_bar.send_keys(city)

    # Hit Enter to search
    search_bar.send_keys(Keys.ENTER)
    time.sleep(2)

    # Find the coordinates in the URL
    current_url = driver.current_url
    coordinates_start_index = current_url.index("@") + 1
    coordinates_end_index = current_url.index(",", coordinates_start_index)
    latitude_longitude = current_url[coordinates_start_index:coordinates_end_index].split(",")
    latitude = latitude_longitude[0]
    longitude = latitude_longitude[1]

    # Close the browser
    driver.quit()

    return latitude, longitude

# Input the city name
city_name = input("Enter the name of the city: ")

# Get coordinates
latitude, longitude = get_coordinates(city_name)

print(f"Coordinates of {city_name}: Latitude: {latitude}, Longitude: {longitude}")

```

    Enter the name of the city: Aberdeen
    


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    Cell In[7], line 40
         37 city_name = input("Enter the name of the city: ")
         39 # Get coordinates
    ---> 40 latitude, longitude = get_coordinates(city_name)
         42 print(f"Coordinates of {city_name}: Latitude: {latitude}, Longitude: {longitude}")
    

    Cell In[7], line 14, in get_coordinates(city)
         11 time.sleep(2)
         13 # Find the search bar using XPath
    ---> 14 search_bar = driver.find_element_by_xpath("//input[@id='searchboxinput']")
         16 # Input the city
         17 search_bar.send_keys(city)
    

    AttributeError: 'WebDriver' object has no attribute 'find_element_by_xpath'



```python
import requests
from bs4 import BeautifulSoup

def scrape_gaming_laptops():
    # URL of the page with the list of best gaming laptops
    url = "https://www.digit.in/top-products/best-gaming-laptops-40.html"

    # Send a GET request to the URL
    response = requests.get(url)

    # Parse the HTML content
    soup = BeautifulSoup(response.content, 'html.parser')

    # Find all the containers with details of gaming laptops
    laptop_containers = soup.find_all('div', {'class': 'TopNumbeHeading active sticky-footer'})

    # Loop through each laptop container and extract details
    for laptop in laptop_containers:
        # Extract laptop details
        name = laptop.find('div', {'class': 'TopNumbeHeading sticky-footer'}).text.strip()
        specs = laptop.find('div', {'class': 'Spcs-details'}).text.strip()

        # Print the extracted details
        print("Name:", name)
        print("Specifications:", specs)
        print("\n")

# Scrape details of gaming laptops
scrape_gaming_laptops()

```


```python
import requests
from bs4 import BeautifulSoup

def scrape_billionaires():
    # URL of the page with the list of billionaires
    url = "https://www.forbes.com/billionaires/"

    # Send a GET request to the URL
    response = requests.get(url)

    # Parse the HTML content
    soup = BeautifulSoup(response.content, 'html.parser')

    # Find all the containers with details of billionaires
    billionaire_containers = soup.find_all('div', {'class': 'personInfo'})

    # Loop through each billionaire container and extract details
    for billionaire in billionaire_containers:
        # Extract billionaire details
        rank = billionaire.find('div', {'class': 'rank'}).text.strip()
        name = billionaire.find('div', {'class': 'personName'}).text.strip()
        net_worth = billionaire.find('div', {'class': 'netWorth'}).text.strip()
        age = billionaire.find('div', {'class': 'age'}).text.strip()
        citizenship = billionaire.find('div', {'class': 'countryOfCitizenship'}).text.strip()
        source = billionaire.find('div', {'class': 'source'}).text.strip()
        industry = billionaire.find('div', {'class': 'category'}).text.strip()

        # Print the extracted details
        print("Rank:", rank)
        print("Name:", name)
        print("Net Worth:", net_worth)
        print("Age:", age)
        print("Citizenship:", citizenship)
        print("Source:", source)
        print("Industry:", industry)
        print("\n")

# Scrape details for all billionaires
scrape_billionaires()

```


```python
# Parsing the HTML content using BeautifulSoup.
import requests
from bs4 import BeautifulSoup

def scrape_hostels(location):
    url = f"https://www.hostelworld.com/findabed.php/ChosenCity.{location}/ChosenCountry.England"

    # Send a GET request to the URL
    response = requests.get(url)

    # Parse the HTML content
    soup = BeautifulSoup(response.content, 'html.parser')

    # Find all the containers with details of hostels
    hostel_containers = soup.find_all('div', {'class': 'property-card'})

    # Loop through each hostel container and extract details
    for hostel in hostel_containers:
        # Extract hostel details
        name = hostel.find('h2', {'class': 'title title-6'}).text.strip()
        distance = hostel.find('span', {'class': 'description'}).text.strip()
        ratings = hostel.find('div', {'class': 'score orange'}).text.strip()
        total_reviews = hostel.find('div', {'class': 'reviews'}).text.strip()
        overall_reviews = hostel.find('div', {'class': 'keyword'}).text.strip()
        privates_price = hostel.find('div', {'class': 'price-col'}).text.strip()
        dorms_price = hostel.find('div', {'class': 'price-col'}).find_next_sibling('div').text.strip()
        facilities = [facility.text.strip() for facility in hostel.find_all('i', {'class': 'property-icon'})]
        description = hostel.find('div', {'class': 'rating-factors'}).text.strip()

        # Print the extracted details
        print("Name:", name)
        print("Distance from city centre:", distance)
        print("Ratings:", ratings)
        print("Total reviews:", total_reviews)
        print("Overall reviews:", overall_reviews)
        print("Privates from price:", privates_price)
        print("Dorms from price:", dorms_price)
        print("Facilities:", facilities)
        print("Description:", description)
        print("\n")

# Scrape data for hostels in London
scrape_hostels("London")

```


```python

```
