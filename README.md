
# Looping Over Collections - Lab

## Introduction
In this lab, we will be practicing what we know about for loops. We will use them to reduce the amount of code we write by hand to iterate through collections. We will use data from our excel file, `cities.xlsx`, that had data on different cities, their populations, and their areas. Finally, we will use this information to plot and compare each city. Let's get started!

## Objectives
* Use loops to iterate over a collection of records

## Identifying When To Use a For Loop

In the last lesson, we worked with some of our travel data.  Let's retrieve a list with our travel information again from excel.  First, we read the information from excel as a list of dictionaries, with each dictionary representing a location.  And we assign this list to the variable `cities`.


```python
import pandas
file_name = './cities.xlsx'
travel_df = pandas.read_excel(file_name)
cities = travel_df.to_dict('records')
```

Next, we retrieve the first three city names, stored as the `'City'` attribute of each dictionary, and `'Population'` of each of the cities.  Then we plot the names as our `x_values` and the populations as our `y_values`.


```python
import plotly

plotly.offline.init_notebook_mode(connected=True)

x_values = [cities[0]['City'], cities[1]['City'], cities[2]['City']]
y_values = [cities[0]['Population'], cities[1]['Population'], cities[2]['Population']]
trace_first_three_pops = {'x': x_values, 'y': y_values, 'type': 'bar'}
plotly.offline.iplot([trace_first_three_pops])
```

Of course, as you may have spotted, there is a good amount of repetition in displaying this data.  Just take a look at how we retrieved the data for our `x_values` and `y_values`. And you'll notice that, unless we know the exact number of cities and populations in our excel file, this method of retrieving data might miss some data or try to access values that don't exist. 

We can take a close look at this below:


```python
x_values = [cities[0]['City'], cities[1]['City'], cities[2]['City']]
y_values = [cities[0]['Population'], cities[1]['Population'], cities[2]['Population']]
```

As we can see, if we have any more than 3 lines of data, our `x_values` and `y_values` will be incomplete, and if we had only 2 lines of data, our code would break.

So in this lesson, we will use our `for` loop to display information about our travel locations with less repetition and more accuracy.

## Instructions

Before we get into creating graphs from our cities data, let's get a bit more comfortable with the data we are working with. Let's see if we can iterate through just one element (i.e. a city **dictionary** object) to get the **area**. 


```python
buenos_aires = cities[0]
buenos_aires
```


```python
# here we want to find just the area of buenos_aires
buenos_aires_area = None
# code goes here
for key, value in buenos_aires.items():
    if key == "Area":
        buenos_aires_area = value
buenos_aires_area
```

Now that we have a bit more familiarity with our dictionaries, we can move on to gathering all the information we need to create our traces. 

Our `cities` list contains information about the top 12 cities.  For our upcoming iteration tasks, it will be useful to have a list of the numbers 0 through 11.  Use what we know about `len` and `range`to generate a list of numbers 1 through 11.  Assign this to a variable called `city_indices`.


```python
city_indices = list(range(0,12))
city_indices # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]
```

Now, using the `cities` list, we want to create a list of the names for each city. Loop through each city and append it's name (`'City'`) to the `city_names` list. 


```python
city_names = [city['City'] for city in cities]
city_names
```

Your task is to assign the variable `names_and_ranks` to a list, with each element equal to the city name and it's corresponding rank.  For example, the first element would be, `"1. Buenos Aires"` and the second would be `"2. Toronto"`. Luckily for us, the list of cities that we read from our excel file is already in order my most populous to least. So, all we need to do is add numbers 1 through 12 to the beginning of each city name.

Use a `for` loop and the lists `city_indices` and `city_names` to accomplish this.  We'll need to perform some nifty string interpolation to format our strings properly.  Check out [f-string interpolation](https://www.programiz.com/python-programming/string-interpolation#f) to see how we can pass values into a string.  Remember that list indices start at zero, but we want our `names_and_ranks` list to start at one!


```python
names_and_ranks = []
for i in city_indices:
    names_and_ranks.append(f"{i+1}. {city_names[i]}")
names_and_ranks
# write a for loop that adds the properly formatted string to the names_and_ranks list
```


```python
names_and_ranks[0] # '1. Buenos Aires'
names_and_ranks[1] # '2. Toronto'
names_and_ranks[-1] # '12. Iguazu Falls'
```

Ok, now use another for loop to iterate through our list of `cities` and create a new list called `city_populations` that had the population for each city (`Population`).


```python
city_populations = []
for city in cities:
    city_populations.append(city['Population'])
city_populations 
# use a for loop to iterate through the list of cities with their corresponding population
```


```python
city_populations[0] # 2891000
city_populations[1] # 2800000
city_populations[-1] # 0
```

Great! Now we can begin to plot this data.  First, let's create a trace of our populations and set it to the variable `trace_populations`.


```python
trace_populations = {'x': names_and_ranks, 
                     'y': city_populations, 
                     'text': names_and_ranks, 
                     'type': 'bar', 
                     'name': 'Population'}
```


```python
import plotly
plotly.offline.init_notebook_mode(connected=True)
plotly.offline.iplot([trace_populations])
```

Now we want declare a variable called `city_areas` that points to a list of all of the areas of the cities.  Let's use a `for` loop to iterate through our `cities` and have `city_areas` equal to each area of the city.  


```python
city_areas = [city['Area'] for city in cities]
city_areas
```


```python
trace_areas = {'x': names_and_ranks, 'y': city_areas, 'text': names_and_ranks, 'type': 'bar', 'name': 'Area'}
```

Since there is such a disparity between the size of numbers that represent the population compared to the size of the numbers that represent the areas for each city, below, we are going to represent the population numbers in hundreds. That is, we will take each population and divide it by 100 (i.e. 5,000,000/100 = 500,000.00) This is purely for the purposes of seeing our numbers plotted. 

No need to worry about the code below, just run the following cells to see our bar graph and keep in mind that the populations all have two more zeros to their real number.


```python
altered_populations = []
for population in city_populations:
    altered_populations.append(population/100)

trace_populations = {'x': names_and_ranks, 
                     'y': altered_populations, 
                     'text': names_and_ranks, 
                     'type': 'bar', 
                     'name': 'Population'}
```


```python
import plotly
plotly.offline.init_notebook_mode(connected=True)
plotly.offline.iplot([trace_populations, trace_areas])
```

### Summary

In this section we saw how we can use `for` loops to go through elements of a list and perform the same operation on each.  By using `for` loops we were able to reduce the amount of code that we wrote and while also writing more expressive code.
