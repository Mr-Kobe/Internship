```python
def replace_with_colon(text):
    replacements = {' ': ':', ',': ':', '.': ':'}
    for old, new in replacements.items():
        text = text.replace(old, new)
    return text

sample_text = 'Python Exercises, PHP exercises.'
output_text = replace_with_colon(sample_text)
print("Expected Output:", output_text)

```

    Expected Output: Python:Exercises::PHP:exercises:
    


```python
import pandas as pd
import re

data = {'SUMMARY': ['hello, world!', 'XXXXX test', '123four, five:; six...']}

df = pd.DataFrame(data)

# Function to remove non-word characters
def remove_non_word(text):
    return re.sub(r'\W+', ' ', text)

# Apply the function to the 'SUMMARY' column
df['SUMMARY'] = df['SUMMARY'].apply(remove_non_word)

print(df)




```

                 SUMMARY
    0       hello world 
    1         XXXXX test
    2  123four five six 
    


```python
import re

def find_long_words(text):
    pattern = re.compile(r'\b\w{4,}\b')  # Compile regex pattern to find words with at least 4 characters
    long_words = pattern.findall(text)    # Find all matching words in the text
    return long_words

# Example:
text = "This is a string with some long words like Daddy, Mummy, Uncle, Sister, Brother"
long_words = find_long_words(text)
print("Words with at least 4 characters:", long_words)

```

    Words with at least 4 characters: ['This', 'string', 'with', 'some', 'long', 'words', 'like', 'Daddy', 'Mummy', 'Uncle', 'Sister', 'Brother']
    


```python
import re

def find_words_3_to_5_characters(text):
    pattern = re.compile(r'\b\w{3,5}\b')  # Compile regex pattern to find words with 3 to 5 characters
    words = pattern.findall(text)         # Find all matching words in the text
    return words

# Example:
text = "My dresscode comprises of a hat, shoe, Socks, Sunglass, Glove, perfume"
words = find_words_3_to_5_characters(text)
print("Words with 3 to 5 characters:", words)

```

    Words with 3 to 5 characters: ['hat', 'shoe', 'Socks', 'Glove']
    


```python
import re

def remove_parentheses(strings):
    pattern = re.compile(r'\(|\)')  # Compile regex pattern to match parentheses
    cleaned_strings = [pattern.sub('', string) for string in strings]  # Remove parentheses from each string
    return cleaned_strings

# Example:
sample_text = ["example (.com)", "hr@fliprobo (.com)", "github (.com)", "Hello (Data Science World)", "Data (Scientist)"]
output = remove_parentheses(sample_text)
print("Expected Output:")
for string in output:
    print(string)



```

    Expected Output:
    example .com
    hr@fliprobo .com
    github .com
    Hello Data Science World
    Data Scientist
    


```python
import re

def split_uppercase(string):
    pattern = re.compile(r'(?=[A-Z])')  
    words = pattern.split(string)
    return words

# Sample text
sample_text = "ImportanceOfRegularExpressionsInPython"

# To split the string into uppercase letter
result = split_uppercase(sample_text)

# Print the result
print(result)





```

    ['', 'Importance', 'Of', 'Regular', 'Expressions', 'In', 'Python']
    


```python
import re

def insert_spaces(text):
    pattern = re.compile(r'(?<=\d)(?=[A-Za-z])')  # Positive lookbehind assertion to match a digit followed by a word character
    modified_text = pattern.sub(' ', text)  # Replace the position with a space
    return modified_text

# Sample text
sample_text = "RegularExpression1IsAn2ImportantTopic3InPython"

# Insert spaces between words starting with numbers
result = insert_spaces(sample_text)

# Print the result
print(result)


```

    RegularExpression1 IsAn2 ImportantTopic3 InPython
    


```python
import re

def insert_spaces(text):
    # Regular expression to link words starting with capital letters or numbers
    pattern = re.compile(r'(?<=\d)(?=[A-Za-z])|(?<=[A-Z])(?=[A-Z][a-z])')

    # Insert spaces between words starting with capital letters or numbers
    modified_text = pattern.sub(' ', text)

    return modified_text

# Sample text
sample_text = "RegularExpression1IsAn2ImportantTopic3InPython"

# Insert spaces between words starting with capital letters or numbers
result = insert_spaces(sample_text)

# Print the result
print(result)

```

    RegularExpression1 IsAn2 ImportantTopic3 InPython
    


```python
import re

def match_string(text):
    pattern = re.compile(r'^[a-zA-Z0-9_]+$')  # Regular expression pattern to match the described pattern
    match = pattern.match(text)
    return bool(match)

# Test cases
strings_to_match = ["First_Class567", "xyzCDQ678", "Goodmorning_", "123456789", "_Lemonade_", "Rapture"]

for string in strings_to_match:
    if match_string(string):
        print(f"{string} matches the pattern.")
    else:
        print(f"{string} does not match the pattern.")

```

    First_Class567 matches the pattern.
    xyzCDQ678 matches the pattern.
    Goodmorning_ matches the pattern.
    123456789 matches the pattern.
    _Lemonade_ matches the pattern.
    Rapture matches the pattern.
    


```python
import re

def starts_with_number(text, number):
    pattern = re.compile(r'^' + re.escape(str(number)))
    match = pattern.match(text)
    return bool(match)

# Example
strings_to_test = ["John316", "666Apocalypse", "456Jordan", "Brave123"]

number_to_start_with = 666

for string in strings_to_test:
    if starts_with_number(string, number_to_start_with):
        print(f"{string} starts with the number {number_to_start_with}.")
    else:
        print(f"{string} does not start with the number {number_to_start_with}.")

```

    John316 does not start with the number 666.
    666Apocalypse starts with the number 666.
    456Jordan does not start with the number 666.
    Brave123 does not start with the number 666.
    


```python
def remove_leading_zeros(ip_address):
    # Split the IP address into its individual octets
    octets = ip_address.split('.')

    # Remove leading zeros from each octet
    cleaned_octets = [str(int(octet)) for octet in octets]

    # Join the cleaned octets back together
    cleaned_ip_address = '.'.join(cleaned_octets)

    return cleaned_ip_address

# Test cases
ip_addresses = ["192.168.001.001", "010.001.001.010", "356.000.000.257"]

for ip_address in ip_addresses:
    cleaned_ip = remove_leading_zeros(ip_address)
    print(f"Original IP: {ip_address}, Cleaned IP: {cleaned_ip}")

```

    Original IP: 192.168.001.001, Cleaned IP: 192.168.1.1
    Original IP: 010.001.001.010, Cleaned IP: 10.1.1.10
    Original IP: 356.000.000.257, Cleaned IP: 356.0.0.257
    


```python
import re

def extract_date(text):
    pattern = re.compile(r'\b(?:January|February|March|April|May|June|July|August|September|October|November|December)\s+\d{1,2}(?:st|nd|rd|th)?\s+\d{4}\b')
    match = pattern.search(text)
    if match:
        return match.group()
    else:
        return "Date not found"

# Example:
with open("sample_text.txt", "r") as file:
    text = file.read()

date = extract_date(text)
print("Extracted Date:", date)


```


    ---------------------------------------------------------------------------

    FileNotFoundError                         Traceback (most recent call last)

    Cell In[25], line 12
          9         return "Date not found"
         11 # Example:
    ---> 12 with open("sample_text.txt", "r") as file:
         13     text = file.read()
         15 date = extract_date(text)
    

    File ~\anaconda3\Lib\site-packages\IPython\core\interactiveshell.py:284, in _modified_open(file, *args, **kwargs)
        277 if file in {0, 1, 2}:
        278     raise ValueError(
        279         f"IPython won't let you open fd={file} by default "
        280         "as it is likely to crash IPython. If you know what you are doing, "
        281         "you can use builtins' open."
        282     )
    --> 284 return io_open(file, *args, **kwargs)
    

    FileNotFoundError: [Errno 2] No such file or directory: 'sample_text.txt'



```python

      def search_strings(text, strings_to_search):
    """
    Function to search for literal strings in a given text.
    
    Args:
    text (str): The text to search within.
    strings_to_search (list): A list of literal strings to search for.
    
    Returns:
    list: A list of tuples containing the string found and its index.
    """
    found_strings = []
    for string in strings_to_search:
        index = text.find(string)
        if index != -1:
            found_strings.append((string, index))
    return found_strings

# Sample text
sample_text = 'The quick brown fox jumps over the lazy dog.'

# Words to search for
searched_words = ['fox', 'dog', 'horse']

# Search for the words
results = search_strings(sample_text, searched_words)

# Print results
if results:
    print("Found the following strings:")
    for result in results:
        print(f"String: '{result[0]}' found at index: {result[1]}")
else:
    print("No matching strings found.")

    



```


      File <tokenize>:14
        index = text.find(string)
        ^
    IndentationError: unindent does not match any outer indentation level
    



```python
def search_string(original_string, search_word):
    index = original_string.find(search_word)
    if index != -1:
        print(f"'{search_word}' found at position {index} in the original string.")
    else:
        print(f"'{search_word}' not found in the original string.")

sample_text = 'The quick brown fox jumps over the lazy dog.'
search_word = 'fox'

search_string(sample_text, search_word)

```

    'fox' found at position 16 in the original string.
    


```python
def find_substrings(original_string, pattern):
    start_index = 0
    while True:
        index = original_string.find(pattern, start_index)
        if index == -1:
            break
        print(f"'{pattern}' found at position {index} in the original string.")
        start_index = index + 1

sample_text = 'Python exercises, PHP exercises, C# exercises'
pattern = 'exercises'

find_substrings(sample_text, pattern)

```

    'exercises' found at position 7 in the original string.
    'exercises' found at position 22 in the original string.
    'exercises' found at position 36 in the original string.
    


```python
def find_occurrences_and_positions(original_string, pattern):
    occurrences = 0
    positions = []
    start_index = 0
    while True:
        index = original_string.find(pattern, start_index)
        if index == -1:
            break
        occurrences += 1
        positions.append(index)
        start_index = index + 1
    return occurrences, positions

sample_text = 'Foot ball, Basket ball, Volley ball'
pattern = 'ball'

occurrences, positions = find_occurrences_and_positions(sample_text, pattern)

print(f"'{pattern}' occurs {occurrences} times in the original string.")
if occurrences > 0:
    print(f"Positions of '{pattern}': {positions}")

```

    'ball' occurs 3 times in the original string.
    Positions of 'ball': [5, 18, 31]
    


```python
from datetime import datetime

def convert_date_format(date_str):
    # Parse the date string using datetime.strptime
    date_object = datetime.strptime(date_str, '%Y-%m-%d')
    # Format the date in dd-mm-yyyy format
    formatted_date = date_object.strftime('%d-%m-%Y')
    return formatted_date

# Sample date string in yyyy-mm-dd format
date_str = '2000-01-01'

# Convert date format
formatted_date = convert_date_format(date_str)

print("Original date:", date_str)
print("Converted date:", formatted_date)

```

    Original date: 2000-01-01
    Converted date: 01-01-2000
    


```python
import re

def find_decimal_numbers(text):
    # Compile the regular expression pattern
    pattern = re.compile(r'\b\d+\.\d{1,2}\b')
    # Find all matches using findall
    matches = pattern.findall(text)
    return matches

# Sample text
sample_text = "01.12 0132.123 2.31875 145.8 3.01 27.25 0.25"

# Find decimal numbers
result = find_decimal_numbers(sample_text)
print("Decimal numbers with precision of 1 or 2:")
print(result)

```

    Decimal numbers with precision of 1 or 2:
    ['01.12', '145.8', '3.01', '27.25', '0.25']
    


```python
def separate_numbers_and_positions(text):
    numbers_with_positions = []
    for i, char in enumerate(text):
        if char.isdigit():
            numbers_with_positions.append((char, i))
    return numbers_with_positions

# Sample text
sample_text = "Ilove2beasoldeirAK47fighting4mycountry"

# Separate numbers and their positions
result = separate_numbers_and_positions(sample_text)

# Print the result
print("Numbers and their positions:")
for number, position in result:
    print(f"Number: {number}, Position: {position}")

```

    Numbers and their positions:
    Number: 2, Position: 5
    Number: 4, Position: 18
    Number: 7, Position: 19
    Number: 4, Position: 28
    


```python
import re

def extract_maximum_number(text):
    # Find all numeric values in the text
    numbers = re.findall(r'\d+', text)
    # Convert the list of strings to integers and find the maximum
    maximum_number = max(map(int, numbers))
    return maximum_number

# Sample text
sample_text = 'My marks in each semester are: 947, 896, 926, 524, 734, 950, 642'

# Extract maximum number
maximum_number = extract_maximum_number(sample_text)
print("Maximum number:", maximum_number)

```

    Maximum number: 950
    


```python
def insert_spaces(text):
    result = ''
    for i, char in enumerate(text):
        # Insert space before capital letter that follows a lowercase letter
        if char.isupper() and i > 0 and text[i - 1].islower():
            result += ' ' + char
        else:
            result += char
    return result

# Sample text
sample_text = "RegularExpressionIsAnImportantTopicInPython"

# Insert spaces
formatted_text = insert_spaces(sample_text)
print("Formatted text:", formatted_text)

```

    Formatted text: Regular Expression Is An Important Topic In Python
    


```python
import re

def find_sequences(text):
    pattern = re.compile(r'[A-Z][a-z]+')
    sequences = pattern.findall(text)
    return sequences

# Sample text
sample_text = "FightAgainstInsurgencyInMyCountryNigeria"

# Find sequences
sequences = find_sequences(sample_text)
print("Sequences found:", sequences)

```

    Sequences found: ['Fight', 'Against', 'Insurgency', 'In', 'My', 'Country', 'Nigeria']
    


```python
import re

def remove_continuous_duplicates(sentence):
    # Use a regular expression to find continuous duplicate words
    pattern = re.compile(r'\b(\w+)(\s+\1\b)+', re.IGNORECASE)
    # Replace continuous duplicate words with a single occurrence
    result = pattern.sub(r'\1', sentence)
    return result

# Sample text
sample_text = "Hello hello world world"

# Remove continuous duplicate words
result = remove_continuous_duplicates(sample_text)
print("Output:", result)

```

    Output: Hello world
    


```python
import re

def ends_with_alphanumeric(text):
    pattern = re.compile(r'\w$')
    match = pattern.search(text)
    return bool(match)

# Test strings
test_strings = ["AK47", "Shevchenko", "Ronaldinho", "John316"]

# Check if each string ends with an alphanumeric character
for string in test_strings:
    if ends_with_alphanumeric(string):
        print(f"'{string}' ends with an alphanumeric character.")
    else:
        print(f"'{string}' does not end with an alphanumeric character.")

```

    'AK47' ends with an alphanumeric character.
    'Shevchenko' ends with an alphanumeric character.
    'Ronaldinho' ends with an alphanumeric character.
    'John316' ends with an alphanumeric character.
    


```python
import re

def extract_hashtags(text):
    pattern = re.compile(r'#\w+')
    hashtags = pattern.findall(text)
    return hashtags

# Sample text
sample_text = """RT @kapil_kausik: #Doltiwal I mean #xyzabc is "hurt" by #Demonetization as the same has rendered USELESS <ed><U+00A0><U+00BD><ed><U+00B1><U+0089> "acquired funds" No wo"""

# Extract hashtags
hashtags = extract_hashtags(sample_text)
print("Extracted hashtags:", hashtags)

```

    Extracted hashtags: ['#Doltiwal', '#xyzabc', '#Demonetization']
    


```python
import re

def remove_unicode_symbols(text):
    pattern = re.compile(r'<U\+[0-9A-Fa-f]+>')
    clean_text = pattern.sub('', text)
    return clean_text

# Sample text
sample_text = "@Jags123456 Bharat band on 28??<ed><U+00A0><U+00BD><ed><U+00B8><U+0082>Those who  are protesting #demonetization  are all different party leaders"

# Remove Unicode symbols
clean_text = remove_unicode_symbols(sample_text)
print("Cleaned text:", clean_text)

```

    Cleaned text: @Jags123456 Bharat band on 28??<ed><ed>Those who  are protesting #demonetization  are all different party leaders
    


```python
import re

def remove_words_of_length_2_to_4(text):
    pattern = re.compile(r'\b\w{2,4}\b')
    clean_text = pattern.sub('', text)
    return clean_text

# Sample text
sample_text = "The following example creates an ArrayList with a capacity of 50 elements. 4 elements are then added to the ArrayList and the ArrayList is trimmed accordingly."

# Remove words of length between 2 and 4
cleaned_text = remove_words_of_length_2_to_4(sample_text)
print("Cleaned text:", cleaned_text)

```

    Cleaned text:  following example creates  ArrayList  a capacity   elements. 4 elements   added   ArrayList   ArrayList  trimmed accordingly.
    


```python

```
