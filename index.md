---
layout: default
title: Home
---

# About
- For [Project]((https://github.com/sc-adams/sc-adams.github.io/blob/main/sadams/About/about.md)) I recommend the following: [plan](https://github.com/sc-adams/sc-adams.github.io/#software-engineering-and-design).

  
# Software Engineering and Design

## About the Project
The client dashboard is designed to ...


<div align="center">
  <p><strong>CS340 Client Server Development Dashboard</strong></p>
  <img src="https://github.com/sheraadams/CS340/assets/110789514/c4454846-462e-4580-8fd5-57159f70c1d0" width="800" alt="CS340 Client Server Developmemt Dashboard: d1">
</div>

## Python read function

The Python file gives CRUD helper ...

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

I plan to ...

<div style="text-align: center;">
  <p><strong>Proudly crafted with ❤️ by <a href="https://github.com/sheraadams" target="_blank">Shera Adams</a>.</strong></p>
</div>


