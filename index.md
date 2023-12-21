---
layout: default
title: Home
---

# About
- For my [Software Engineering and Design](https://github.com/sc-adams/sc-adams.github.io
/blob/main/README.md#software-engineering-and-design) enhancement, I will add two new charts that describe additional characteristics of the Grazioso Salvare client database. Here is my [enhancement plan](https://github.com/sheraadams/499final/blob/main/README.md#software-engineering-and-design-enhancments).

- For my [Data Structures and Algorithms](https://github.com/sc-adams/sc-adams.github.io
/blob/main/README.md#data-structures-and-algorithms) enhancement, I plan to increase the efficiency of the CS250 Java slideshow application by using an appropriate data structure in place of conditional branching to control the slideshow view. Here is my [enhancement plan](https://github.com/sheraadams/499final/blob/main/README.md#data-structures-enhancements).

- For my [Databases](https://github.com/sc-adams/sc-adams.github.io
/blob/main/README.md#database) enhancement, plan to set up the database on my local desktops (Mac and Windows) and implement a field mask to hide confidential fields such as client name from the dashboard data table. Here is my [enhancement plan](https://github.com/sheraadams/499final/blob/main/README.md#database-enhancements).
  
# Software Engineering and Design

## About the Project
The client dashboard is designed to provide functionality and a user-friendly interface for interacting with a database using creation, updating, reading, reading, and deletion database management functions that utilize the PyMongo software. The dashboard is programmed using [MongoDB](https://www.mongodb.com/products/platform/cloud) and [Python](https://python.org/), and it uses [Jupyter Notebook](https://jupyter.org/) and [Dash](https://plotly.com/) to create interactive graphs.


<div align="center">
  <p><strong>CS340 Client Server Development Dashboard</strong></p>
  <img src="https://github.com/sheraadams/CS340/assets/110789514/c4454846-462e-4580-8fd5-57159f70c1d0" width="800" alt="CS340 Client Server Developmemt Dashboard: d1">
</div>

## Python read function

The Python file gives CRUD helper functions for working with our database. The **read()** function takes in a query as a parameter and will try to use the built-in pyMongo function, find(). If the query is found, the function will return the query, if it is not found the function will print an error and return an empty list.

```python
# Create method to implement the R in CRUD.
    def read(self, query):
        #try to read the data, if success return query
        try:
            results = list(self.collection.find(query))
            return results
        # error reading the data, print "error" and return empty list
        except Exception as e:
            print(f"Error reading documents: {e}")
            return []

```

## Software Engineering and Design Enhancements

I plan to implement two more visualization graphs that describe additional data characteristics. The existing scatter chart describes the distribution of the members of the breed column. 

The new bar chart should describe the distribution of the members of the outcome type column. The new pie chart should describe the distribution of the members of the age column. As the data currently is defined in terms of weeks, I plan to create six or more age categories to group the data.

# Data Structures and Algorithms
lorum ipsum

## Data Structures Enhancements

I plan to increase the efficiency of our application by implementing an appropriate data structure to replace the conditional branching that controls the slideshow view. Also, I plan to comment the code to make it more readable, reusable, and adaptable. 

# Database

For my database enhancement, I plan to recreate the project locally on both Mac and Windows. Additionally, I plan to apply a field mask to the name field to hide confidential client names. 

- **Windows:** To set up the database and dashboard application locally we will need to install the [Mongo Shell](https://www.mongodb.com/try/download/shell) and [Mongo Compass](https://www.mongodb.com/try/download/compass).

- **Mac:** To set up the database and dashboard application locally we will need to install [Mongo Compass](https://www.mongodb.com/try/download/compass) and
 [Homebrew](https://brew.sh). We will also install MongoDB community using the terminal 

  ```bash
  brew tap mongodb/brew
  brew install mongodb-community
  brew services start mongodb-community
  ```
  We can stop the service using
  ```bash
  brew services stop mongodb-community
  ```


### 6. Install packages

Bash:
```bash
pip install dash==2.8.1
pip install dash-leaflet==0.1.9
pip install pandas==1.4.2
pip install plotly
pip install jupyter-dash
pip install numpy
pip install matplotlib
pip install pymongo
```

### 7. Run the Jupyter Notebook file

You can easily run the Jupyter Notebook file in [Visual Studio Code](https://code.visualstudio.com/download) using the latest Python language extension along with the latest Jupyter Notebook extension. 

## Database Enhancements

For my database category enhancement, I chose to reproduce the database and dashboard locally both on Windows OS and Mac OS. Additionally, I plan to mask the name field in the case that the client names should be confidential. 

Check out my [references](https://github.com/sheraadams/499final/blob/main/references.md) here.

<div style="text-align: center;">
  <p><strong>Proudly crafted with ❤️ by <a href="https://github.com/sheraadams" target="_blank">Shera Adams</a>.</strong></p>
</div>



<div style="text-align: center;">
  <p><strong>Proudly crafted with ❤️ by <a href="https://github.com/sheraadams" target="_blank">Shera Adams</a>.</strong></p>
</div>


