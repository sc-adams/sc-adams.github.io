---
layout: default
title: Home
---

# Capstone Presentation
- For my [Software Engineering and Design](https://sheraadams.github.io/#software-engineering-and-design) enhancement, I added a pie chart and a bar chart that describe age upon outcome by category and outcome type of the Grazioso Salvare client database. Here are my [enhancements](https://sheraadams.github.io/#software-engineering-and-design-enhancments).

- For my [Data Structures and Algorithms](https://sheraadams.github.io/#data-structures-and-algorithms) enhancement, I increased the efficiency of the CS250 Java slideshow application by using an arraylist in place of conditional branching to control the slideshow view. Here are my [enhancemens](https://sheraadams.github.io/#data-structures-enhancements).

- For my [Databases](https://sheraadams.github.io/#databases) enhancement, plan to set up the database and dashboard locally (on both Mac and Windows), outlining this process to improve future workflows. Additionally I plan to implement a field mask to hide confidential fields such as client name from the dashboard data table. Here are my [enhancements](https://sheraadams.github.io/#database-enhancements).
  
# Software Engineering and Design

## About the Project
The client dashboard is designed to provide functionality and a user-friendly interface for interacting with a database using creation, updating, reading, reading, and deletion database management functions that utilize the PyMongo software. The dashboard is programmed using [MongoDB](https://www.mongodb.com/products/platform/cloud) and [Python](https://python.org/), and it uses [Jupyter Notebook](https://jupyter.org/) and [Dash](https://plotly.com/) to create interactive graphs.

The Model-View-Controller (MVC) software design pattern is employed in this multi-tier application. The architecture is composed of a MongoDB NoSQL database, the Python code that communicates with the database, and an interactive visualization dashboard. For seamless communication and data exchange, the application utilizes the RESTful protocol which extends the capabilities of the HTTP protocol and provides the application programming interface (API) for our program. This design provides a modular, scalable, adaptable, and easy-to-maintain structure. Here is a video preview of the [dashboard on YouTube](https://www.youtube.com/watch?v=ZxHuFK2Ne_o). 

<div align="center">
  <p><strong>CS340 Client Server Development Dashboard</strong></p>
  <img src="https://github.com/sheraadams/CS340/assets/110789514/c4454846-462e-4580-8fd5-57159f70c1d0" width="800" alt="CS340 Client Server Developmemt Dashboard: d1">
</div>

## Functional Requirements: 

- The application should provide an interface for clients to interact with the Grazioso database.
- The application should allow lookup by common profiles.
- The application should have:
  - Interactive options to filter the data
  - A data table that dynamically responds to filter options
  - A geolocation chart that displays the animal’s coordinates
  - An additional chart that dynamically responds to the filter options.


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

## The Jupyter Notebook file

In the following code, we log in to the database with our credentials and create a data frame from all records. We remove the _id column from the data frame and create an instance of the JupyterDash class.
```python
db = CRUD(username, password)

df = pd.DataFrame.from_records(db.read({}))
df.drop(columns=['_id'],inplace=True)

app = JupyterDash(__name__)
```

The app.layout section creates the layout of the app using HTML code. In our layout we create a dash table to display the database columns and rows.
```python
    # Format the data table 
    dash_table.DataTable(
        id='datatable-id',
        columns=[{"name": i, "id": i, "deletable": False, "selectable": True} for i in df.columns],
        style_data_conditional=[{'if': {'column_editable': False},'backgroundColor': 'white','color': 'rgb(30, 30, 30)'}],
        style_header_conditional=[{'if': {'column_editable': False},'backgroundColor': 'white','color': 'rgb(30, 30, 30)'}],
        data=df.to_dict('records'),
        editable=False,
        filter_action="native",
        sort_action="native",
        sort_mode="multi",
        column_selectable=False,
        row_selectable="single",
        row_deletable=False,
        selected_columns=[],
        selected_rows=[0],
        derived_virtual_selected_rows=[],
        page_action="native",
        page_current=0,
        page_size=10,
       # derived_virtual_data=df.to_dict('records'),
        style_header={'border': '1px solid pink'},
    ),
```

With the **update dashboard()** function we can update the dashboard according to the selected filter type. We have an input callback that takes the filter filterValue as a parameter and we have output callbacks for the data and columns of the data table.
```python
@app.callback(Output('datatable-id', 'data'),
              Output('datatable-id', 'columns'),
              [Input('filter-type', 'value')])
def update_dashboard(filterValue, **kwargs):
    # Filter data based on the selected type 
    if filterValue == 'mountain':
        # save the df as result of the read function call passing the mountain query as a parameter
        df = pd.DataFrame.from_records(db.read(mountain_rescue_query))
    elif filterValue == 'water':
        # save the df as result of the read function call passing the water query as a parameter
        df = pd.DataFrame.from_records(db.read(water_rescue_query))
    elif filterValue == 'disaster':
        # save the df as result of the read function call passing the disaster query as a parameter
        df = pd.DataFrame.from_records(db.read(disaster_query))
    else:
        # return all data if reset (default datatable/ read all data)
        df = pd.DataFrame.from_records(db.read({'animal_type': {'$in': ["Dog", "Cat", "Bird", "Other"]}}))
    
    # delete the _id collumn
    df.drop(columns=['_id'],inplace=True)   
    # define the column properties
    columns = [{"name": i, "id": i, "deletable": False, "selectable": True} for i in df.columns]
    # create a dictionary from the data frame
    data = df.to_dict('records')
    #return a tuple of data, columns
    return (data, columns)
```
When we run the Jupyter Notebook file, we can see the interactive data table, geolocation map, and chart display and interactively respond to the user selection. We can filter the selection with the radio buttons labeled “Water Rescue”, “Mountain Rescue”, “Disaster Rescue”, or “Reset”.

## Software Engineering and Design Enhancements

The existing scatter chart describes the distribution of the members of the breed column. I implemented two more visualization graphs that describe additional data characteristics. I added a pie chart and a bar chart to describe additional characteristics of the data including the age upon outcome distribution and outcome type distributions respectively.  As the data currently is defined in terms of weeks, I created categories to group the age upon outcome data. The CRUD Python file containing create, read, update, and delete database helper functions serves as an adaptable module that can be easily implemented into any Mongodb application. The additional charts describe new characteristics of the data, giving the end user a more holistic understanding of the animal shelter data.  

I added comments for clarity and I moved the filters outside of the updateDashboard() function to increase the reusability, adaptability, and readability of the code. Adding comments and making some coding rearrangements for clarity increased the adaptability of the codebase. Making the code more modular, clear, and understandable allows us to reuse our code in the future, simlpifying workflows and increasing efficiency. 

Finally, I deployed the database and the dashboard on Windows and Mac and detailed the process step by step for future users, simplifying future workflows. Deploying the database and the dashboard on Windows and Mac and detailing the process step by step for future users, simplifies future workflows and allows us to significantly reduce future development time.

## Course objectives
**Course Outcome 1.** Employ strategies for building collaborative environments that enable diverse audiences to support organizational decision-making in the field of computer science.

In this project, I developed a dashboard that allows the end user to interact through filter selection and search and visualize descriptive charts of the data depending on the selection. This enhancement facilitates a collaborative environment in which many users from diverse backgrounds and technical experience can explore and understand data effectively, ultimately supporting informed organizational decision-making.

**Course Outcome 2.** Design, develop, and deliver professional-quality oral, written, and visual communications that are coherent, technically sound, and appropriately adapted to specific audiences and contexts.

In this project, I considered the audience at each step asking myself how I could facilitate the need for thorough, meaningful, and descriptive data visualizations. By adding additional graphs that describe other important characteristics of our animal shelter database set, we can provide the end user with more well-rounded data. Providing the user with multifaceted insights can facilitate informed decision-making.

# Data Structures and Algorithms

## About the Project 

This Java program is a slideshow console application that uses Swing and JFrame to create a GUI window. JFrame serves as a container for the application window. The program utilizes HTML for styling the text and embedding the images in the panels. 
There are three main panels: a slide pane, a text pane, and a button pane. This program uses the Model-View-Controller (MVC) design pattern where the images and text are the model, the Java code is the application logic and the buttons allow the client to interact with the software.

<div align="center">
  <p><strong>CS 250 Slide Show</strong></p>
  <img src="https://github.com/sheraadams/499final/assets/110789514/859574a3-24f2-4e40-b40e-17992084cae1" width="800" alt="CS 250 Slideshow d1">
</div>

(Southern New Hampshire University, n.d.)

## Functional Requirements

 - The application should have an ordered list of destinations from the most popular location to the fifth most popular.
 - Each destination on the list should have the following attributes shown:
   - Destination name
   - Destination short description (one sentence)
   - Destination picture
   - Destination link turn this into a markdown list

## The Code Review

The initComponent() method is used to define each of the components, set display properties like font, color, and size, and set listeners for the buttons. 

```java
  private void initComponent() {
        this.card = new CardLayout();
        this.cardText = new CardLayout();
        this.slidePane = new JPanel();
        this.textPane = new JPanel();
        this.textPane.setBackground(Color.WHITE);
        this.textPane.setBounds(5, 470, 790, 50);
        this.textPane.setVisible(true);
        this.buttonPane = new JPanel();
        this.btnPrev = new JButton();
        this.btnNext = new JButton();
        this.lblSlide = new JLabel();
        this.lblTextArea = new JLabel();
        this.setSize(800, 600);
        this.setLocationRelativeTo((Component)null);
        this.setTitle("Top Detox Destinations");
        this.getContentPane().setLayout(new BorderLayout(10, 50));
        this.setDefaultCloseOperation(3);
        this.slidePane.setLayout(this.card);
        this.textPane.setLayout(this.cardText);
//...
```
We can see that this application uses a conditional statement based on the value of the index “i”. Based on the numerical value of i, the string is reassigned to corresponding string. The string itself is HTML code that formats text or formats and loads an image and lblSlide.setText() function is responsible for updating the slide.

```java
        for(int i = 1; i <= 5; ++i) {
            this.lblSlide = new JLabel();
            this.lblTextArea = new JLabel();
            this.lblSlide.setText(this.getResizeIcon(i));
            this.lblTextArea.setText(this.getTextDescription(i));
            this.slidePane.add(this.lblSlide, "card" + i);
            this.textPane.add(this.lblTextArea, "cardText" + i);
        }
//...
    private String getResizeIcon(int i) {
        String image = "";
        if (i == 1) {
            image = "<html><body><img width= '800' height='500' src='" + this.getClass().getResource("/resources/costarica.jpg") + "'</body></html>";
        } else if (i == 2) {
            image = "<html><body><img width= '800' height='500' src='" + this.getClass().getResource("/resources/sedona.jpg") + "'</body></html>";
        } else if (i == 3) {
            image = "<html><body><img width= '800' height='500' src='" + this.getClass().getResource("/resources/nepal.jpg") + "'</body></html>";
        } else if (i == 4) {
            image = "<html><body><img width= '800' height='500' src='" + this.getClass().getResource("/resources/egypt.jpg") + "'</body></html>";
        } else if (i == 5) {
            image = "<html><body><img width= '800' height='500' src='" + this.getClass().getResource("/resources/grandcanyon.jpg") + "'</body></html>";
        }

        return image;
    }
// ...
```

We can increase the efficiency of our application using a data structure in place of conditional branching. When considering a data structure for our application we are most concerned about access. Insertions into the user’s top five destination list should is not expected to be a frequent task. Once per-day insertions and rearranging would likely be sufficient.

## Data Structures Compared
Hash tables and binary search trees have higher overhead costs and require storage space for the structure itself. 

Hash tables (or Java HashMap) do not preserve order. Hash tables are best for small data sets in which fast searches are a priority. 

The binary search tree is for cases when data sets are likely to grow, and fast searches are important. Binary search trees require maintenance to keep them balanced.

## Reasoning for Arraylist structure

The Arraylist is good for fast random access to elements by index, and it preserves the order of elements. In terms of access, the array structure performs very well and has the average run-time space complexity of O(1) to O(N) (O(N) being the worst case in which N is the number of elements in the array), which is linear.

Vectors and arraylist offer advantages in terms of pure runtime and space complexity when frequent insertions and sorting are not needed. 

If dynamic insertions and deletions are anticipated at runtime, the resulting run-time space complexity would be O(N). For our specific use case of managing a top-five favorites list where sorting is unnecessary, the space complexity remains O(N) due to the straightforward nature of the list. If sorting were required, the expected run-time space complexity could range from O(N) to O(N log N) if using mergesort. 

<div align="center">
  <p><strong>Big O Cheatsheet (Eric Drowell, n.d.)</strong></p>
  <img src="https://github.com/sheraadams/499final/assets/110789514/6a7a1374-6ebb-465d-b10c-ddb5c3fdb6a8" width="800" alt="Big O Cheatsheet">
</div>

Arrays have a worst-case run-time space complexity O(N) where N is the number of elements in the array. As we can see here, O(N) is generally fair to good run-time space complexity. Additionally, the cost is linear.

<div align="center">
  <p><strong>Big O Cheatsheet (Eric Drowell, n.d.)</strong></p>
  <img src="https://github.com/sheraadams/499final/assets/110789514/e165d6a6-3c6f-40a1-90cf-f86ca1b7b015" width="800" alt="Big O Cheatsheet">
</div>

## Course outcomes
**Course Outcome 1:** Employ strategies for building collaborative environments that enable diverse audiences to support organizational decision-making in the field of computer science
In my work for this project, my team practiced communication in an Agile environment in which inclusion and iterative development are the expectation. For this project, initially we were tasked with building a travel website in which the user’s Top Five Destinations” would be shown as well as their description, and a hyperlink to the location package. As the project evolved and changed over time, however, the focus shifted from all destinations to “Detox Destinations”. 
Suddenly, it became clear how important iterative development is in software development as elements would need to be changed before the end of the sprint. When the time came, the changes were simple to implement as a result of my adherence to software engineering best practices that produce modular, adaptable code that follows industry standard naming conventions.
**Course Outcome 2:** Design, develop, and deliver professional-quality oral, written, and visual communications that are coherent, technically sound, and appropriately adapted to specific audiences and contexts
I considered the target audience and the context at every point of this application’s development and as a result, the software received perfect marks upon submission.  
**Course Outcome 3:** Design and evaluate computing solutions that solve a given problem using algorithmic principles and computer science practices and standards appropriate to its solution, while managing the trade-offs involved in design choices
I implemented an algorithmic solution that was appropriate for this software in its intended context. The arraylist is a flexible, lightweight, dynamic structure well suited to the task of primary access and occasional, insertion, sorting, and deleting elements. 

## Data Structures Enhancements

I added comments to increase the clarity and adaptability of this code. Adding clear, meaningful comments that give the reader context and understanding helps to make the code base more adaptable and reusable in the future. I also increased the efficiency of the slideshow application by implementing an arraylist to replace the conditional branching that controls the slideshow view. 

I had some difficulting choosing between an arraylist and a linked list. Both of these structures offer the benefits of order preservation and fast access. I ultimately decided that the linked list brings additional complexity and overhead storage for the structure itself when this is not necessary for daily insertions and sorting. I learned that it is always important to consider the needs of the end-user and the context of the application carefully before choosing an appropriate structure. Additionally, it is always crucial to weigh the trade-offs associated with data structure selection to avoid excessive development and revisions.  

# Databases
For my database enhancement, I plan to recreate the project locally on both Mac and Windows. Additionally, I plan to apply a field mask to the name field to hide confidential client names. 

### Windows Installations 
To set up the database and dashboard application locally we will need to install the Windows Subsystem for Linux[(https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/], the [Mongo Shell](https://www.mongodb.com/try/download/shell) and [MongoDB Compass](https://www.mongodb.com/try/download/compass).

### Mac Installations
 To set up the database and dashboard application locally we will need to install [MongoDB Compass](https://www.mongodb.com/try/download/compass) and [Homebrew](https://brew.sh). We will also install MongoDB community using the terminal and the homevbrew package manager:

```bash
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community
```

Similarly, we can stop the mongodb service when we are finished with our work in a testing environment with: 
```bash
brew services stop mongodb-community
```

## Instructions (All Platforms)
After completing the preivous installation process depending on your operating system, the following instructions will walk you through importing AAC data into MongoDB, creating a user, and connecting to the database using the Linux shell. To begin working with this software on Windows OS, first follow these instructions to install the bash shell. Otherwise, when working with the environment provided on the Apporto virtual lab, proceed with the following: 

### 1. Import the AAC data

To import AAC data into MongoDB, you can use the `mongoimport` command. Make sure to replace `${MONGO_USER}`, `${MONGO_PASS}`, `${MONGO_PORT}`, and `${MONGO_HOST}` with your actual MongoDB credentials.

```bash
# in the linux shell run the following command to import the aac_shelter_outcomes excel document
mongoimport --username="${MONGO_USER}" --password="${MONGO_PASS}" --port=${MONGO_PORT} --host=${MONGO_HOST} --type=csv --headerline --db AAC --collection animals --authenticationDatabase admin --drop /usr/local/datasets/aac_shelter_outcomes.csv
```

### 2. Create the user
Bash:
```bash
use admin;
db.createUser({
  user: "username",
  pwd: "password",
  roles: [
    { role: "readWrite", db: "AAC" }
  ]
});
```

### 3. Set the environment variables
Bash:
```bash
# set the environment
# determine the port number for python connection settings 
printenv | grep -i mongo
# login the user
MONGO_USER=username
MONGO_PASS=password

```

### 4. Verify connection

```mongosh
# open the mongo shell
mongosh
// verify access to the database
show dbs
use AAC
// verify the database has the animals collection
show collections
// verify the collection has the csv document imported successfully 
db.animals.findOne()
// demonstrate connection to the database using the logged in user
db.runCommand({connectionStatus:1})
```

### 5. Add indexes to our database 
To increase data retrieval speed and our database's overall efficiency, we will add indexes for searches that we expect will be frequently used. We will add indexes for the breed and outcome type. 

```mongosh
db.animals.createIndex({ breed: 1 })
db.animals.createIndex({ breed: 1, outcome_type: 1 })
// verify the successful creation with simple queries: 
db.animals.find({ breed: "beagle" }).explain("executionStats")
db.animals.find({ breed: "beagle", outcome_type: "Transfer" }).explain("executionStats")
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

## Course Outcomes

**Course Outcome 4:** Demonstrate an ability to use well-founded and innovative techniques, skills, and tools in computing practices for the purpose of implementing computer solutions that deliver value and accomplish industry-specific goals
By developing this application locally on both Windows and Mac I used industry best practice and used industry best practices to ensure performance and compatability in diverse computing environments.

**Course Outcome 5:** Develop a security mindset that anticipates adversarial exploits in software architecture and designs to expose potential vulnerabilities, mitigate design flaws, and ensure privacy and enhanced security of data and resources.

Giving only the designated user access to read and write to the AAC database prevents unauthorized users from accessing and modifying the database. Additionally stopping the Mongodb service when we are finished frees up resources on the system when the service is no longer being used. 

## Database Enhancements

I reproduced this project locally with a step-by-step guide detailing the process on both Windows and Mac operating systems. Further, I added a character mask to hide confidential fields in the case that a client name should be confidential for some uses. 

While developing on Mac, I noticed several key differences compared to Windows. The Unix-like terminal on Mac simplified the development process and reduced the need for extensive software installations. When developing on Windows, it's necessary to install WSL (Windows Subsystem for Linux) and Bash, before proceeding with MongoDB installation. I found Installing MongoDB to be more straightforward with the Homebrew package manager and the built-in bash terminal.

Check out my [references](https://sheraadams.github.io/references.md) here.

<div style="text-align: center;">
  <p><strong>Proudly crafted with ❤️ by <a href="https://github.com/sheraadams" target="_blank">Shera Adams</a>.</strong></p>
</div>

