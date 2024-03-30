```python
# First step is to install necessary tools and libraries
# requests library is used to fetch the HTML content of the website
# Beaufifulsoup4 is then used to parse the HTML and extract information
#  pandas library is used to create a DataFrame from the extracted data.

import requests
from bs4 import BeautifulSoup
import pandas as pd

# URL of the IMDb page website
url = "https://www.imdb.com/list/ls056092300/"

# Send a GET request to the URL
response = requests.get(url)

# Parse the HTML content
soup = BeautifulSoup(response.content, 'html.parser')

# Find all the movie items
movie_items = soup.find_all('div', class_='lister-item-content')

# Lists to store data
names = []
ratings = []
years = []

# Extract data for each movie
for item in movie_items:
    name = item.find('a').text.strip()  # Movie name
    rating = item.find('span', class_='ipl-rating-star__rating').text.strip()  # Movie rating
    year = item.find('span', class_='lister-item-year').text.strip()  # Movie release year
    
    # Append data to lists
    names.append(name)
    ratings.append(rating)
    years.append(year)

# Create DataFrame
data = {
    'Name': names,
    'Rating': ratings,
    'Year': years
}
df = pd.DataFrame(data)

# Display DataFrame
print(df)




```

                                     Name Rating    Year
    0                     Ship of Theseus      8  (2012)
    1                              Iruvar    8.4  (1997)
    2                     Kaagaz Ke Phool    7.8  (1959)
    3   Lagaan: Once Upon a Time in India    8.1  (2001)
    4                     Pather Panchali    8.2  (1955)
    ..                                ...    ...     ...
    95                   The World of Apu    8.4  (1959)
    96                        Kanchivaram    8.2  (2008)
    97                    Monsoon Wedding    7.3  (2001)
    98                              Black    8.1  (2005)
    99                            Deewaar      8  (1975)
    
    [100 rows x 3 columns]
    


```python
# use the requests library to fetch the website content
# use BeautifulSoup to parse it

import requests
from bs4 import BeautifulSoup

def scrape_house_details(locality):
    url = f"https://www.nobroker.in/property/sale/bangalore/{locality.lower()}"
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    
# Create empty list

    houses = []

    house_cards = soup.find_all('div', class_='card')
    for card in house_cards:
        house = {}
        title = card.find('h2', class_='heading-6 font-semi-bold nb__1AShY').text.strip()
        location = card.find('div', class_='nb__2CMjv').text.strip()
        area = card.find('div', class_='nb__3oNyC').text.strip()
        emi = card.find('div', class_='font-semi-bold heading-6').text.strip()
        price = card.find('div', class_='font-semi-bold heading-6').next_sibling.text.strip()
        house['Title'] = title
        house['Location'] = location
        house['Area'] = area
        house['EMI'] = emi
        house['Price'] = price
        houses.append(house)

    return houses

# Enter the localities
localities = ['Indira-Nagar', 'Jayanagar', 'Rajaji-Nagar']

for locality in localities:
    print(f"Houses in {locality}:")
    print()
    houses = scrape_house_details(locality)
    for house in houses:
        print("Title:", house['Title'])
        print("Location:", house['Location'])
        print("Area:", house['Area'])
        print("EMI:", house['EMI'])
        print("Price:", house['Price'])
        print()



    

```

    Houses in Indira-Nagar:
    
    Houses in Jayanagar:
    
    Houses in Rajaji-Nagar:
    
    


```python
# use the requests library to fetch the website content
# use BeautifulSoup to parse it

import requests
from bs4 import BeautifulSoup

def scrape_product_details():
    url = "https://www.bewakoof.com/bestseller?sort=popular"
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')

    products = []

    # Find all product items
    product_items = soup.find_all('div', class_='productCardItem')

    # Limit to first 10 products
    for item in product_items[:10]:
        product = {}

        # Extract product name
        product_name = item.find('div', class_='productCardDetail').find('div', class_='name').text.strip()
        product['Name'] = product_name

        # Extract product price
        product_price = item.find('div', class_='productCardDetail').find('span', class_='price').text.strip()
        product['Price'] = product_price

        # Extract product image URL
        image_url = item.find('div', class_='productCardImgWrapper').find('img')['src']
        product['Image URL'] = image_url

        products.append(product)

    return products

# To Scrape and print the first 10 product details
products = scrape_product_details()
for index, product in enumerate(products, start=1):
    print(f"Product {index}:")
    print("Name:", product['Name'])
    print("Price:", product['Price'])
    print("Image URL:", product['Image URL'])
    print()

```


```python
import requests
from bs4 import BeautifulSoup

def scrape_cnbc_news():
    url = "https://www.cnbc.com/world/?region=world"
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')

    news_articles = []

    # Find all news articles
    article_items = soup.find_all('div', class_='Card-text')

    for item in article_items:
        news_article = {}

        # Extract heading
        heading = item.find('h3', class_='Card-title').text.strip()
        news_article['Heading'] = heading

        # Extract date
        date = item.find('time')['datetime']
        news_article['Date'] = date

        # Extract news link
        link = item.find('a')['href']
        news_article['News Link'] = link

        news_articles.append(news_article)

    return news_articles

#  To scrape and print the headings, dates, and news links
articles = scrape_cnbc_news()
for index, article in enumerate(articles, start=1):
    print(f"Article {index}:")
    print("Heading:", article['Heading'])
    print("Date:", article['Date'])
    print("News Link:", article['News Link'])
    print()

```


```python
import requests
from bs4 import BeautifulSoup

url = 'https://www.keaipublishing.com/en/journals/artificial-intelligence-in-agriculture/most-downloaded-articles/'

# Sending a GET request to the URL
response = requests.get(url)

# Parsing the HTML content
soup = BeautifulSoup(response.text, 'html.parser')

# Finding all the articles
articles = soup.find_all('div', class_='article')

# Extracting information for each article
for article in articles:
    # Extracting paper title
    title = article.find('h3', class_='title').text.strip()
    
    # Extracting date
    date = article.find('span', class_='date').text.strip()
    
    # Extracting author
    author = article.find('span', class_='authors').text.strip()
    
    # Printing information
    print("Title:", title)
    print("Date:", date)
    print("Author:", author)
    print()

```


```python
import requests
from bs4 import BeautifulSoup
from googleapiclient.discovery import build

# Function to extract details from each post
def scrape_patreon_posts():
    url = "https://www.patreon.com/coreyms"
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')

    posts = []

    # Find all post items
    post_items = soup.find_all('div', class_='post')

    for item in post_items:
        post_details = {}

        # Extract heading
        heading = item.find('h2', class_='post__title').text.strip()
        post_details['Heading'] = heading

        # Extract date
        date = item.find('time', class_='post__date').text.strip()
        post_details['Date'] = date

        # Extract content
        content = item.find('div', class_='post__excerpt').text.strip()
        post_details['Content'] = content

        # Extract likes for video if present
        youtube_link = item.find('iframe', class_='embed-thumbnail__iframe')
        if youtube_link:
            youtube_url = youtube_link['src']
            likes = get_likes_from_youtube(youtube_url)
            post_details['Likes'] = likes
        else:
            post_details['Likes'] = 'N/A'

        posts.append(post_details)

    return posts

# Function to get likes for a video from YouTube using YouTube Data API
def get_likes_from_youtube(youtube_url):
    # Extract video ID from the YouTube URL
    video_id = youtube_url.split('/')[-1].split('?')[0]

    # YouTube Data API Key (replace 'YOUR_API_KEY' with your actual API key)
    api_key = 'YOUR_API_KEY'

    # Initialize YouTube Data API client
    youtube = build('youtube', 'v3', developerKey=api_key)

    # Call the videos.list method to retrieve video details
    request = youtube.videos().list(
        part='statistics',
        id=video_id
    )
    
    # Execute the request
    response = request.execute()

    # Extract likes count from the response
    likes = response['items'][0]['statistics']['likeCount'] if 'likeCount' in response['items'][0]['statistics'] else 'N/A'

    return likes

# Call the function to scrape and display post details
posts = scrape_patreon_posts()
for post in posts:
    print("Heading:", post['Heading'])
    print("Date:", post['Date'])
    print("Content:", post['Content'])
    print("Likes:", post['Likes'])
    print()

```


```python
pip install requests beautifulsoup4 google-api-python-client

```

    Requirement already satisfied: requests in c:\users\admin\anaconda3\lib\site-packages (2.31.0)
    Requirement already satisfied: beautifulsoup4 in c:\users\admin\anaconda3\lib\site-packages (4.12.2)
    Collecting google-api-python-client
      Obtaining dependency information for google-api-python-client from https://files.pythonhosted.org/packages/83/b7/7db8b769f67826c6c9919b1e9fc128fa6dd9be1c41aa221dfd7fc7a65176/google_api_python_client-2.124.0-py2.py3-none-any.whl.metadata
      Downloading google_api_python_client-2.124.0-py2.py3-none-any.whl.metadata (6.6 kB)
    Requirement already satisfied: charset-normalizer<4,>=2 in c:\users\admin\anaconda3\lib\site-packages (from requests) (2.0.4)
    Requirement already satisfied: idna<4,>=2.5 in c:\users\admin\anaconda3\lib\site-packages (from requests) (3.4)
    Requirement already satisfied: urllib3<3,>=1.21.1 in c:\users\admin\anaconda3\lib\site-packages (from requests) (1.26.16)
    Requirement already satisfied: certifi>=2017.4.17 in c:\users\admin\anaconda3\lib\site-packages (from requests) (2023.7.22)
    Requirement already satisfied: soupsieve>1.2 in c:\users\admin\anaconda3\lib\site-packages (from beautifulsoup4) (2.4)
    Collecting httplib2<1.dev0,>=0.19.0 (from google-api-python-client)
      Obtaining dependency information for httplib2<1.dev0,>=0.19.0 from https://files.pythonhosted.org/packages/a8/6c/d2fbdaaa5959339d53ba38e94c123e4e84b8fbc4b84beb0e70d7c1608486/httplib2-0.22.0-py3-none-any.whl.metadata
      Downloading httplib2-0.22.0-py3-none-any.whl.metadata (2.6 kB)
    Collecting google-auth!=2.24.0,!=2.25.0,<3.0.0.dev0,>=1.32.0 (from google-api-python-client)
      Obtaining dependency information for google-auth!=2.24.0,!=2.25.0,<3.0.0.dev0,>=1.32.0 from https://files.pythonhosted.org/packages/9e/8d/ddbcf81ec751d8ee5fd18ac11ff38a0e110f39dfbf105e6d9db69d556dd0/google_auth-2.29.0-py2.py3-none-any.whl.metadata
      Downloading google_auth-2.29.0-py2.py3-none-any.whl.metadata (4.7 kB)
    Collecting google-auth-httplib2<1.0.0,>=0.2.0 (from google-api-python-client)
      Obtaining dependency information for google-auth-httplib2<1.0.0,>=0.2.0 from https://files.pythonhosted.org/packages/be/8a/fe34d2f3f9470a27b01c9e76226965863f153d5fbe276f83608562e49c04/google_auth_httplib2-0.2.0-py2.py3-none-any.whl.metadata
      Downloading google_auth_httplib2-0.2.0-py2.py3-none-any.whl.metadata (2.2 kB)
    Collecting google-api-core!=2.0.*,!=2.1.*,!=2.2.*,!=2.3.0,<3.0.0.dev0,>=1.31.5 (from google-api-python-client)
      Obtaining dependency information for google-api-core!=2.0.*,!=2.1.*,!=2.2.*,!=2.3.0,<3.0.0.dev0,>=1.31.5 from https://files.pythonhosted.org/packages/86/75/59a3ad90d9b4ff5b3e0537611dbe885aeb96124521c9d35aa079f1e0f2c9/google_api_core-2.18.0-py3-none-any.whl.metadata
      Downloading google_api_core-2.18.0-py3-none-any.whl.metadata (2.7 kB)
    Collecting uritemplate<5,>=3.0.1 (from google-api-python-client)
      Obtaining dependency information for uritemplate<5,>=3.0.1 from https://files.pythonhosted.org/packages/81/c0/7461b49cd25aeece13766f02ee576d1db528f1c37ce69aee300e075b485b/uritemplate-4.1.1-py2.py3-none-any.whl.metadata
      Downloading uritemplate-4.1.1-py2.py3-none-any.whl.metadata (2.9 kB)
    Collecting googleapis-common-protos<2.0.dev0,>=1.56.2 (from google-api-core!=2.0.*,!=2.1.*,!=2.2.*,!=2.3.0,<3.0.0.dev0,>=1.31.5->google-api-python-client)
      Obtaining dependency information for googleapis-common-protos<2.0.dev0,>=1.56.2 from https://files.pythonhosted.org/packages/dc/a6/12a0c976140511d8bc8a16ad15793b2aef29ac927baa0786ccb7ddbb6e1c/googleapis_common_protos-1.63.0-py2.py3-none-any.whl.metadata
      Downloading googleapis_common_protos-1.63.0-py2.py3-none-any.whl.metadata (1.5 kB)
    Collecting protobuf!=3.20.0,!=3.20.1,!=4.21.0,!=4.21.1,!=4.21.2,!=4.21.3,!=4.21.4,!=4.21.5,<5.0.0.dev0,>=3.19.5 (from google-api-core!=2.0.*,!=2.1.*,!=2.2.*,!=2.3.0,<3.0.0.dev0,>=1.31.5->google-api-python-client)
      Obtaining dependency information for protobuf!=3.20.0,!=3.20.1,!=4.21.0,!=4.21.1,!=4.21.2,!=4.21.3,!=4.21.4,!=4.21.5,<5.0.0.dev0,>=3.19.5 from https://files.pythonhosted.org/packages/ad/6e/1bed3b7c904cc178cb8ee8dbaf72934964452b3de95b7a63412591edb93c/protobuf-4.25.3-cp310-abi3-win_amd64.whl.metadata
      Downloading protobuf-4.25.3-cp310-abi3-win_amd64.whl.metadata (541 bytes)
    Collecting proto-plus<2.0.0dev,>=1.22.3 (from google-api-core!=2.0.*,!=2.1.*,!=2.2.*,!=2.3.0,<3.0.0.dev0,>=1.31.5->google-api-python-client)
      Obtaining dependency information for proto-plus<2.0.0dev,>=1.22.3 from https://files.pythonhosted.org/packages/ad/41/7361075f3a31dcd05a6a38cfd807a6eecbfb6dbfe420d922cd400fc03ac1/proto_plus-1.23.0-py3-none-any.whl.metadata
      Downloading proto_plus-1.23.0-py3-none-any.whl.metadata (2.2 kB)
    Collecting cachetools<6.0,>=2.0.0 (from google-auth!=2.24.0,!=2.25.0,<3.0.0.dev0,>=1.32.0->google-api-python-client)
      Obtaining dependency information for cachetools<6.0,>=2.0.0 from https://files.pythonhosted.org/packages/fb/2b/a64c2d25a37aeb921fddb929111413049fc5f8b9a4c1aefaffaafe768d54/cachetools-5.3.3-py3-none-any.whl.metadata
      Downloading cachetools-5.3.3-py3-none-any.whl.metadata (5.3 kB)
    Requirement already satisfied: pyasn1-modules>=0.2.1 in c:\users\admin\anaconda3\lib\site-packages (from google-auth!=2.24.0,!=2.25.0,<3.0.0.dev0,>=1.32.0->google-api-python-client) (0.2.8)
    Collecting rsa<5,>=3.1.4 (from google-auth!=2.24.0,!=2.25.0,<3.0.0.dev0,>=1.32.0->google-api-python-client)
      Obtaining dependency information for rsa<5,>=3.1.4 from https://files.pythonhosted.org/packages/49/97/fa78e3d2f65c02c8e1268b9aba606569fe97f6c8f7c2d74394553347c145/rsa-4.9-py3-none-any.whl.metadata
      Downloading rsa-4.9-py3-none-any.whl.metadata (4.2 kB)
    Requirement already satisfied: pyparsing!=3.0.0,!=3.0.1,!=3.0.2,!=3.0.3,<4,>=2.4.2 in c:\users\admin\anaconda3\lib\site-packages (from httplib2<1.dev0,>=0.19.0->google-api-python-client) (3.0.9)
    Requirement already satisfied: pyasn1<0.5.0,>=0.4.6 in c:\users\admin\anaconda3\lib\site-packages (from pyasn1-modules>=0.2.1->google-auth!=2.24.0,!=2.25.0,<3.0.0.dev0,>=1.32.0->google-api-python-client) (0.4.8)
    Downloading google_api_python_client-2.124.0-py2.py3-none-any.whl (12.4 MB)
       ---------------------------------------- 0.0/12.4 MB ? eta -:--:--
        --------------------------------------- 0.2/12.4 MB 4.4 MB/s eta 0:00:03
       - -------------------------------------- 0.5/12.4 MB 5.9 MB/s eta 0:00:03
       -- ------------------------------------- 0.7/12.4 MB 5.7 MB/s eta 0:00:03
       --- ------------------------------------ 1.1/12.4 MB 5.1 MB/s eta 0:00:03
       ---- ----------------------------------- 1.3/12.4 MB 5.0 MB/s eta 0:00:03
       ----- ---------------------------------- 1.6/12.4 MB 5.2 MB/s eta 0:00:03
       ------ --------------------------------- 1.9/12.4 MB 5.2 MB/s eta 0:00:03
       ------- -------------------------------- 2.2/12.4 MB 5.0 MB/s eta 0:00:03
       -------- ------------------------------- 2.5/12.4 MB 5.0 MB/s eta 0:00:02
       --------- ------------------------------ 2.8/12.4 MB 5.0 MB/s eta 0:00:02
       --------- ------------------------------ 3.1/12.4 MB 5.1 MB/s eta 0:00:02
       ----------- ---------------------------- 3.4/12.4 MB 5.0 MB/s eta 0:00:02
       ----------- ---------------------------- 3.7/12.4 MB 5.0 MB/s eta 0:00:02
       ------------ --------------------------- 4.0/12.4 MB 5.0 MB/s eta 0:00:02
       ------------- -------------------------- 4.3/12.4 MB 5.1 MB/s eta 0:00:02
       -------------- ------------------------- 4.6/12.4 MB 5.1 MB/s eta 0:00:02
       --------------- ------------------------ 4.9/12.4 MB 5.1 MB/s eta 0:00:02
       ---------------- ----------------------- 5.3/12.4 MB 5.0 MB/s eta 0:00:02
       ----------------- ---------------------- 5.5/12.4 MB 5.0 MB/s eta 0:00:02
       ------------------ --------------------- 5.7/12.4 MB 5.1 MB/s eta 0:00:02
       ------------------- -------------------- 5.9/12.4 MB 5.1 MB/s eta 0:00:02
       ------------------- -------------------- 6.2/12.4 MB 5.0 MB/s eta 0:00:02
       -------------------- ------------------- 6.4/12.4 MB 5.1 MB/s eta 0:00:02
       --------------------- ------------------ 6.6/12.4 MB 5.1 MB/s eta 0:00:02
       --------------------- ------------------ 6.8/12.4 MB 5.0 MB/s eta 0:00:02
       ---------------------- ----------------- 7.0/12.4 MB 5.1 MB/s eta 0:00:02
       ----------------------- ---------------- 7.2/12.4 MB 5.0 MB/s eta 0:00:02
       ------------------------ --------------- 7.5/12.4 MB 5.0 MB/s eta 0:00:01
       ------------------------- -------------- 7.8/12.4 MB 5.1 MB/s eta 0:00:01
       -------------------------- ------------- 8.1/12.4 MB 5.1 MB/s eta 0:00:01
       -------------------------- ------------- 8.4/12.4 MB 5.0 MB/s eta 0:00:01
       --------------------------- ------------ 8.7/12.4 MB 5.0 MB/s eta 0:00:01
       ---------------------------- ----------- 8.9/12.4 MB 5.1 MB/s eta 0:00:01
       ----------------------------- ---------- 9.2/12.4 MB 5.1 MB/s eta 0:00:01
       ------------------------------ --------- 9.5/12.4 MB 5.0 MB/s eta 0:00:01
       ------------------------------- -------- 9.9/12.4 MB 5.0 MB/s eta 0:00:01
       -------------------------------- ------- 10.1/12.4 MB 5.0 MB/s eta 0:00:01
       --------------------------------- ------ 10.4/12.4 MB 5.0 MB/s eta 0:00:01
       ---------------------------------- ----- 10.7/12.4 MB 5.0 MB/s eta 0:00:01
       ----------------------------------- ---- 11.0/12.4 MB 5.0 MB/s eta 0:00:01
       ------------------------------------ --- 11.3/12.4 MB 5.0 MB/s eta 0:00:01
       ------------------------------------- -- 11.5/12.4 MB 5.1 MB/s eta 0:00:01
       -------------------------------------- - 11.9/12.4 MB 5.0 MB/s eta 0:00:01
       ---------------------------------------  12.2/12.4 MB 5.0 MB/s eta 0:00:01
       ---------------------------------------  12.4/12.4 MB 5.1 MB/s eta 0:00:01
       ---------------------------------------  12.4/12.4 MB 5.0 MB/s eta 0:00:01
       ---------------------------------------  12.4/12.4 MB 5.0 MB/s eta 0:00:01
       ---------------------------------------  12.4/12.4 MB 5.0 MB/s eta 0:00:01
       ---------------------------------------  12.4/12.4 MB 5.0 MB/s eta 0:00:01
       ---------------------------------------  12.4/12.4 MB 5.0 MB/s eta 0:00:01
       ---------------------------------------- 12.4/12.4 MB 4.5 MB/s eta 0:00:00
    Downloading google_api_core-2.18.0-py3-none-any.whl (138 kB)
       ---------------------------------------- 0.0/138.3 kB ? eta -:--:--
       -------------------------------------- - 133.1/138.3 kB ? eta -:--:--
       ---------------------------------------- 138.3/138.3 kB 2.7 MB/s eta 0:00:00
    Downloading google_auth-2.29.0-py2.py3-none-any.whl (189 kB)
       ---------------------------------------- 0.0/189.2 kB ? eta -:--:--
       ------------------------------------ --- 174.1/189.2 kB 3.5 MB/s eta 0:00:01
       -------------------------------------- - 184.3/189.2 kB 2.8 MB/s eta 0:00:01
       ---------------------------------------- 189.2/189.2 kB 1.6 MB/s eta 0:00:00
    Downloading google_auth_httplib2-0.2.0-py2.py3-none-any.whl (9.3 kB)
    Downloading httplib2-0.22.0-py3-none-any.whl (96 kB)
       ---------------------------------------- 0.0/96.9 kB ? eta -:--:--
       -------------------------------------- - 92.2/96.9 kB ? eta -:--:--
       ---------------------------------------- 96.9/96.9 kB 1.4 MB/s eta 0:00:00
    Downloading uritemplate-4.1.1-py2.py3-none-any.whl (10 kB)
    Downloading cachetools-5.3.3-py3-none-any.whl (9.3 kB)
    Downloading googleapis_common_protos-1.63.0-py2.py3-none-any.whl (229 kB)
       ---------------------------------------- 0.0/229.1 kB ? eta -:--:--
       ---------------------------------------  225.3/229.1 kB 6.9 MB/s eta 0:00:01
       ---------------------------------------- 229.1/229.1 kB 2.8 MB/s eta 0:00:00
    Downloading proto_plus-1.23.0-py3-none-any.whl (48 kB)
       ---------------------------------------- 0.0/48.8 kB ? eta -:--:--
       --------------------------------- ------ 41.0/48.8 kB ? eta -:--:--
       ---------------------------------------- 48.8/48.8 kB 819.9 kB/s eta 0:00:00
    Downloading protobuf-4.25.3-cp310-abi3-win_amd64.whl (413 kB)
       ---------------------------------------- 0.0/413.4 kB ? eta -:--:--
       ----------------------- ---------------- 245.8/413.4 kB 7.6 MB/s eta 0:00:01
       ---------------------------------------  409.6/413.4 kB 5.1 MB/s eta 0:00:01
       ---------------------------------------- 413.4/413.4 kB 3.2 MB/s eta 0:00:00
    Downloading rsa-4.9-py3-none-any.whl (34 kB)
    Installing collected packages: uritemplate, rsa, protobuf, httplib2, cachetools, proto-plus, googleapis-common-protos, google-auth, google-auth-httplib2, google-api-core, google-api-python-client
    Successfully installed cachetools-5.3.3 google-api-core-2.18.0 google-api-python-client-2.124.0 google-auth-2.29.0 google-auth-httplib2-0.2.0 googleapis-common-protos-1.63.0 httplib2-0.22.0 proto-plus-1.23.0 protobuf-4.25.3 rsa-4.9 uritemplate-4.1.1
    Note: you may need to restart the kernel to use updated packages.
    


```python
import requests
from bs4 import BeautifulSoup
from googleapiclient.discovery import build

# Function to extract details from each post
def scrape_patreon_posts():
    url = "https://www.patreon.com/coreyms"
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')

    posts = []

    # Find all post items
    post_items = soup.find_all('div', class_='post')

    for item in post_items:
        post_details = {}

        # Extract heading
        heading = item.find('h2', class_='post__title').text.strip()
        post_details['Heading'] = heading

        # Extract date
        date = item.find('time', class_='post__date').text.strip()
        post_details['Date'] = date

        # Extract content
        content = item.find('div', class_='post__excerpt').text.strip()
        post_details['Content'] = content

        # Extract likes for video if present
        youtube_link = item.find('iframe', class_='embed-thumbnail__iframe')
        if youtube_link:
            youtube_url = youtube_link['src']
            likes = get_likes_from_youtube(youtube_url)
            post_details['Likes'] = likes
        else:
            post_details['Likes'] = 'N/A'

        posts.append(post_details)

    return posts

# Function to get likes for a video from YouTube using YouTube Data API
def get_likes_from_youtube(youtube_url):
    # Extract video ID from the YouTube URL
    video_id = youtube_url.split('/')[-1].split('?')[0]

    # YouTube Data API Key (replace 'YOUR_API_KEY' with your actual API key)
    api_key = 'YOUR_API_KEY'

    # Initialize YouTube Data API client
    youtube = build('youtube', 'v3', developerKey=api_key)

    # Call the videos.list method to retrieve video details
    request = youtube.videos().list(
        part='statistics',
        id=video_id
    )
    
    # Execute the request
    response = request.execute()

    # Extract likes count from the response
    likes = response['items'][0]['statistics']['likeCount'] if 'likeCount' in response['items'][0]['statistics'] else 'N/A'

    return likes

# Call the function to scrape and display post details
posts = scrape_patreon_posts()
for post in posts:
    print("Heading:", post['Heading'])
    print("Date:", post['Date'])
    print("Content:", post['Content'])
    print("Likes:", post['Likes'])
    print()

```


```python

```
