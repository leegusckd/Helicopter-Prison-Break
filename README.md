# Helicopter Prison Break Investigation

## Introduction

In various countries around the globe, prison inmates attempt to escape their captivity by the means of a helicopter. In this analysis, we will work with a detailed dataset of attempted helicopter prison escapes to answer the following questions: 

- In which year did the most helicopter prison break attempts occur? 
- In which countries do the most attempted heliopter prison breaks occur? 

The dataset is provided by wikipedia, and it can be found here: [List of helicopter prison escapes](https://en.wikipedia.org/wiki/List_of_helicopter_prison_escapes)

## Get the Data

We begin by importing some helper functions


```python
from helper import *
```

Now, let's get the data from the  Wikipedia article.


```python
url = 'https://en.wikipedia.org/wiki/List_of_helicopter_prison_escapes'
data = data_from_url(url)
```

Let's print the first three rows



```python
for row in data[:3]:
    print(row)
```

    ['August 19, 1971', 'Santa Martha Acatitla', 'Mexico', 'Yes', 'Joel David Kaplan Carlos Antonio Contreras Castro', "Kaplan was a New York businessman who had been arrested for murder in 1962 in Mexico City and was incarcerated at the Santa Martha Acatitla prison in the Iztapalapa borough. Joel's sister, Judy Kaplan, arranged the means to help Kaplan escape, and on the aforementioned date, a helicopter landed in the prison yard. The guards mistakenly thought this was an official visit. In two minutes, Kaplan and his cellmate Contreras, a Venezuelan counterfeiter, were able to board the craft and were piloted away, before any shots were fired.[9] Both men were flown to Texas and then different planes flew Kaplan to California and Contreras to Guatemala.[3] The Mexican government never initiated extradition proceedings against Kaplan.[9] The escape is told in a book, The 10-Second Jailbreak: The Helicopter Escape of Joel David Kaplan.[4] It also inspired the 1975 action movie Breakout, which starred Charles Bronson and Robert Duvall.[9]"]
    ['October 31, 1973', 'Mountjoy Jail, Dublin', 'Ireland', 'Yes', "JB O'Hagan Seamus Twomey Kevin Mallon", 'An IRA member hijacked a helicopter and forced the pilot to land in the exercise yard of the prison\'s D Wing at 3:40\xa0p.m. Those who Three members of the IRA were escaped aboard the helicopter were IRA members. Another prisoner who also was in the prison was quoted as saying, "One shamefaced screw apologised to the governor and said he thought it was the new Minister for Defence (Paddy Donegan) arriving. I told him it was our Minister of Defence leaving." The escape became Republican lore and was immortalized by "The Helicopter Song", which contains the lines "It\'s up like a bird and over the city. There\'s three men a\'missing I heard the warder say".[1]']
    ['May 24, 1978', 'United States Penitentiary, Marion, Illinois', 'United States', 'No', 'Garrett Brock Trapnell Martin Joseph McNally James Kenneth Johnson', "43-year-old Barbara Ann Oswald hijacked a Saint Louis-based charter helicopter and forced the pilot to land in the prison yard. While landing the aircraft, the pilot, Allen Barklage, who was a Vietnam War veteran, struggled with Oswald and managed to wrestle the gun away from her. Barklage then shot and killed Oswald, thwarting the escape.[10] A few months later Oswald's daughter hijacked TWA Flight 541 in an effort to free Trapnell."]
    

## Removing the Details

The three rows that we printed contains a lot of information, mainly from the "Details" column that describes the details of the escape attempt. Each of the rows in this dataset would be much simpler and easier to do if this column was removed, and that's what we are going to do. 


```python
index = 0

for row in data: 
    row[0] = fetch_year(row[0])
    data[index] = row[:-1]
    index += 1
    
print(data[:3])
```

    [[1971, 'Santa Martha Acatitla', 'Mexico', 'Yes', 'Joel David Kaplan Carlos Antonio Contreras Castro'], [1973, 'Mountjoy Jail, Dublin', 'Ireland', 'Yes', "JB O'Hagan Seamus Twomey Kevin Mallon"], [1978, 'United States Penitentiary, Marion, Illinois', 'United States', 'No', 'Garrett Brock Trapnell Martin Joseph McNally James Kenneth Johnson']]
    

We print those same three rows again and see that they are much easier to read. 

## Attempts Per Year

Let's check the earliest and the most recent years that our dataset contains. 


```python
min_year = min(data, key=lambda x: x[0])[0]
max_year = max(data, key=lambda x: x[0])[0]

print(min_year)
print(max_year)
```

    1971
    2020
    

Now, we will create a list of all the years in our dataset, from 1971 to 2020. 


```python
years = []
for year in range(min_year, max_year + 1):
    years.append(year)
    
print(years)
```

    [1971, 1972, 1973, 1974, 1975, 1976, 1977, 1978, 1979, 1980, 1981, 1982, 1983, 1984, 1985, 1986, 1987, 1988, 1989, 1990, 1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998, 1999, 2000, 2001, 2002, 2003, 2004, 2005, 2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2020]
    

Next, we format the list to make each element look like `[<year>, 0]`


```python
attempts_per_year = []
for year in years:
    attempts_per_year.append([year, 0])
```

Finally, we loop through the dataset, and we increment the value in our list of years each time it appears in the dataset. This will tell us how many escape attemps there are per year. 


```python
for row in data:
    for year_attempt in attempts_per_year:
        year = year_attempt[0] 
        if row[0] == year:
            year_attempt[1] += 1
            
print(attempts_per_year)

```

    [[1971, 1], [1972, 0], [1973, 1], [1974, 0], [1975, 0], [1976, 0], [1977, 0], [1978, 1], [1979, 0], [1980, 0], [1981, 2], [1982, 0], [1983, 1], [1984, 0], [1985, 2], [1986, 3], [1987, 1], [1988, 1], [1989, 2], [1990, 1], [1991, 1], [1992, 2], [1993, 1], [1994, 0], [1995, 0], [1996, 1], [1997, 1], [1998, 0], [1999, 1], [2000, 2], [2001, 3], [2002, 2], [2003, 1], [2004, 0], [2005, 2], [2006, 1], [2007, 3], [2008, 0], [2009, 3], [2010, 1], [2011, 0], [2012, 1], [2013, 2], [2014, 1], [2015, 0], [2016, 1], [2017, 0], [2018, 1], [2019, 0], [2020, 1]]
    

Let's visualize our results into a chart, so that it is easier to process. 


```python
%matplotlib inline
barplot(attempts_per_year)
```


    
![png](output_24_0.png)
    


In which year did the most attempts at breaking out of prison with a helicopter occur? The answer is 1986, 2001, 2007, and 2009 at 3 attempts each. 

Now, let's find out which countries have the most helicopter escape attempts:


```python
countries_frequency = df["Country"].value_counts()
print_pretty_table(countries_frequency)
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Country</th>
      <th>Number of Occurrences</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>France</td>
      <td>15</td>
    </tr>
    <tr>
      <td>United States</td>
      <td>8</td>
    </tr>
    <tr>
      <td>Canada</td>
      <td>4</td>
    </tr>
    <tr>
      <td>Greece</td>
      <td>4</td>
    </tr>
    <tr>
      <td>Belgium</td>
      <td>4</td>
    </tr>
    <tr>
      <td>Australia</td>
      <td>2</td>
    </tr>
    <tr>
      <td>Brazil</td>
      <td>2</td>
    </tr>
    <tr>
      <td>United Kingdom</td>
      <td>2</td>
    </tr>
    <tr>
      <td>Mexico</td>
      <td>1</td>
    </tr>
    <tr>
      <td>Ireland</td>
      <td>1</td>
    </tr>
    <tr>
      <td>Italy</td>
      <td>1</td>
    </tr>
    <tr>
      <td>Puerto Rico</td>
      <td>1</td>
    </tr>
    <tr>
      <td>Chile</td>
      <td>1</td>
    </tr>
    <tr>
      <td>Netherlands</td>
      <td>1</td>
    </tr>
    <tr>
      <td>Russia</td>
      <td>1</td>
    </tr>
  </tbody>
</table>


According to our frequency table, France has the highest documented helicopter prison escapes in the world, with a total of 15 escape attempts from 1971 to 2020. 
