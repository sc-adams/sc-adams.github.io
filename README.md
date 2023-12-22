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

The Model-View-Controller (MVC) software design pattern is employed in this multi-tier application. The architecture is composed of a MongoDB NoSQL database, the Python code that communicates with the database, and an interactive visualization dashboard. For seamless communication and data exchange, the application utilizes the RESTful protocol which extends the capabilities of the HTTP protocol and provides the application programming interface (API) for our program. This design provides a modular, scalable, adaptable, and easy-to-maintain structure. Here is a video preview of the [dashboard](https://www.youtube.com/watch?v=ZxHuFK2Ne_o). 

<div align="center">
  <p><strong>CS340 Client Server Development Dashboard</strong></p>
  <img src="https://github.com/sheraadams/CS340/assets/110789514/c4454846-462e-4580-8fd5-57159f70c1d0" width="800" alt="CS340 Client Server Developmemt Dashboard: d1">
</div>

## Tools I used in this project
- [Python](https://www.python.org): programming language is used to communicate with the database, and it is the programming language of choice for database and data analysis tasks. 
  
- [PyMongo Docs](https://www.mongodb.com/products/platform/cloud): driver is used to communicate with the database using the Python programming language and the MongoDB database management software. 

- [Linux](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/): is used to execute commands and scripts and to facilitate communication with the database and grant access and user permissions. 

- [Jupyter Notebooks](https://jupyter.org/): allows us to create interactive web-based notebooks with the help of the dash framework.  
  
- [Dash](https://plotly.com/): framework is used to create interactive dashboards and user-friendly interfaces for database interaction.

- [MongoDB](https://www.mongodb.com/products/platform/cloud): is used for flexibility, scalability, and compatibility with Python.

## Functional Requirements: 

- The application should provide an interface for clients to interact with the Grazioso database.
- The application should allow lookup by common profiles.
- The application should have:
  - Interactive options to filter the data
  - A data table that dynamically responds to filter options
  - A geolocation chart that displays the animal’s coordinates
  - An additional chart that dynamically responds to the filter options.

## Nonfunctional Requirements

- The application should be user-friendly and intuitive.
- The application should have:
  - Grazioso Salvare logo somewhere on the page.
  - Developer identifiers somewhere on the page.

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


With the **update_graphs()** function we create a bubble scatter graph that responds to the selected filter and provides a visualization of the number of animals by breed in a given filter. We accomplish this with input callbacks passing filterValue as a parameter and output callbacks for the graph children.

```python 
@app.callback(
    Output('graph-id', "children"),
    [Input('datatable-id', "derived_virtual_data"),
     Input('filter-type', 'value')]
)
def update_graphs(viewData, filterValue, **kwargs):
    # Create a data frame from the data
    dff = pd.DataFrame(viewData)

    # Create a new DataFrame with breed counts, resetting the index for easier access
    dff = dff['breed'].value_counts().reset_index()
    # define the column names as breed and count
    dff.columns = ['Breed', 'Count']

    # Combine the breeds with less than 70 members to make the chart more readable
    if filterValue == 'reset':
        dff.loc[dff['Count'] < 70, 'Breed'] = 'Other Breeds'

    # Create a bubble chart
    bubble_fig = px.scatter(
        dff,
        x='Breed',  # X-axis variable
        y='Count',  # Y-axis variable
        size='Count',  # Size of the bubbles
        color='Breed',  # Color of the bubbles
        title='Preferred Animals by Breed',  # title the chart
        template='plotly_white'  # set the template style
    )

    # Return the graph
    return [dcc.Graph(
        figure=bubble_fig,
        # set the size of the graph
        style={'width': '900px', 'height': '500px'}
    )]
```

With the **update_map()** function we create a geolocation chart that responds to the selection by showing a marker at the animal’s longitude and latitude coordinates. We accomplish this with callbacks in a similar way as we did with update_graphs.

```python
@app.callback(
    Output('map-id', "children"),
    [Input('datatable-id', "derived_virtual_data"),
     Input('datatable-id', "derived_virtual_selected_rows")])
def update_map(viewData, index, **kwargs):  
    # handle the case that the viewData is empty
    if viewData is None:
        row = 0
    # handle the case that the index is empty
    elif index is None:
        row = 0
    # set the index default to the first row
    else: 
        row = index[0]  
    # define the data fram
    dff = pd.DataFrame.from_dict(viewData)
    # return the map with a marker at the index's longitude/ longitude coordinates
    return [     # Austin TX is at [30.75,-97.48]
        dl.Map(style={'width': '900px', 'height': '500px'}, center=[30.75,-97.48], zoom=10, children=[
            dl.TileLayer(id="base-layer-id"),
            # Marker with tool tip and popup
            # Column 13 and 14 define the grid-coordinates for the map
            # Column 4 defines the breed for the animal
            # Column 9 defines the name of the animal
            dl.Marker(position=[dff.iloc[row,13],dff.iloc[row,14]], children=[
                dl.Tooltip(dff.iloc[row,4]),
                dl.Popup([
                    html.H1("Animal Name"),  
                    html.P(dff.iloc[row,9])
                ])
            ])
        ])
    ]
```

When we run the Jupyter Notebook file, we can see the interactive data table, geolocation map, and chart display and interactively respond to the user selection. We can filter the selection with the radio buttons labeled “Water Rescue”, “Mountain Rescue”, “Disaster Rescue”, or “Reset”.

<div align="center">
  <p><strong>The final dashboard:</strong></p>
  <img src="https://github.com/sheraadams/CS340/assets/110789514/c4454846-462e-4580-8fd5-57159f70c1d0" width="800" alt="The final dashboard: d1">
</div>

<div align="center">
  <p><strong>Mountain Filter radio button is selected:</strong></p>
  <img src="https://github.com/sheraadams/CS340/assets/110789514/e2ca839f-715c-47f6-9e72-794893173577" width="800" alt="Mountain Filter radio button is selected: d3">
</div>

<div align="center">
  <p><strong>Geolocation ping is shown:</strong></p>
  <img src="https://github.com/sheraadams/CS340/assets/110789514/701b4ddb-b33a-457c-b64f-138e813f5bf8" width="800" alt="Geolocation ping is shown: d4">
</div>

## Software Engineering and Design Enhancements

I plan to implement two more visualization graphs that describe additional data characteristics. The existing scatter chart describes the distribution of the members of the breed column. 

The new bar chart should describe the distribution of the members of the outcome type column. The new pie chart should describe the distribution of the members of the age column. As the data currently is defined in terms of weeks, I plan to create six or more age categories to group the data.

# Data Structures and Algorithms

## About the Project 

This Java program is a slideshow console application that uses Swing and JFrame to create a GUI. JFrame serves as a container for the application window. The program utilizes HTML for styling the text and embedding the images in the panels. 
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

        for(int i = 1; i <= 5; ++i) {
            this.lblSlide = new JLabel();
            this.lblTextArea = new JLabel();
            this.lblSlide.setText(this.getResizeIcon(i));
            this.lblTextArea.setText(this.getTextDescription(i));
            this.slidePane.add(this.lblSlide, "card" + i);
            this.textPane.add(this.lblTextArea, "cardText" + i);
        }

        this.getContentPane().add(this.slidePane, "Center");
        this.getContentPane().add(this.textPane, "South");
        this.buttonPane.setLayout(new FlowLayout(1, 20, 10));
        this.btnPrev.setText("Previous");
        this.btnPrev.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                SlideShow.this.goPrevious();
            }
        });
        this.buttonPane.add(this.btnPrev);
        this.btnNext.setText("    Next    ");
        this.btnNext.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                SlideShow.this.goNext();
            }
        });
        this.buttonPane.add(this.btnNext);
        this.getContentPane().add(this.buttonPane, "South");
    }
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

    private String getTextDescription(int i) {
        String text = "";
        if (i == 1) {
            text = "<html><body><font size='5'>1. Costa Rica</font> <br>Release toxins and find new inspiration at the beautiful Ayurveda Yoga Wellness Retreats. </body></html>";
        } else if (i == 2) {
            text = "<html><body><font size='5'>2. Sedona</font> <br>Visit some of the most famous detox centers in the world and experience mind-body healing in the beautiful Sedona, Arizona.</body></html>";
        } else if (i == 3) {
            text = "<html><body><font size='5'>3. Nepal</font> <br>Rejuvenate your mind and body and realign yourself with yoga and meditation in the world class retreats of Nepal.</body></html>";
        } else if (i == 4) {
            text = "<html><body><font size='5'>4. Egypt</font> <br>Immerse yourself in the history of ancient civilizations as you realign body, mind, and spirit in breathtaking landscapes.</body></html>";
        } else if (i == 5) {
            text = "<html><body><font size='5'>5. Grand Canyon</font> <br>Relax in nature and find wellness and adventure in the breathtaking views of the Grand Canyon.</body></html>";
        }

        return text;
    }
```

We can increase the efficiency of our application using a data structure in place of conditional branching. When considering a data structure for our application we are most concerned about access. Insertions into the user’s top 5 destination list should is not expected to be a frequent task. Once per-day insertions and rearranging would likely be sufficient.

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

## Data Structures Enhancements

I plan to increase the efficiency of our application by implementing an appropriate data structure to replace the conditional branching that controls the slideshow view. Also, I plan to comment the code to make it more readable, reusable, and adaptable. 

# Databases

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

## Instructions
This guide will walk you through importing AAC data into MongoDB, creating a user, and connecting to the database using the Linux shell. To begin working with this software on Windows OS, first follow these instructions to install the bash shell. Otherwise, when working with the environment provided on the Apporto virtual lab, proceed with the following: 

### 1. Import the AAC data

To import AAC data into MongoDB, you can use the `mongoimport` command. Make sure to replace `${MONGO_USER}`, `${MONGO_PASS}`, `${MONGO_PORT}`, and `${MONGO_HOST}` with your actual MongoDB credentials.

```bash
# in the linux shell run the following command to import the aac_shelter_outcomes excel document
mongoimport --username="${MONGO_USER}" --password="${MONGO_PASS}" --port=${MONGO_PORT} --host=${MONGO_HOST} --type=csv --headerline --db AAC --collection animals --authenticationDatabase admin --drop /usr/local/datasets/aac_shelter_outcomes.csv
```

### 2. Create the user

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

## Database Enhancements

For my database category enhancement, I chose to reproduce the database and dashboard locally both on Windows OS and Mac OS. Additionally, I plan to mask the name field in the case that the client names should be confidential. 

Check out my [references](https://github.com/sheraadams/499final/blob/main/references.md) here.

<div style="text-align: center;">
  <p><strong>Proudly crafted with ❤️ by <a href="https://github.com/sheraadams" target="_blank">Shera Adams</a>.</strong></p>
</div>

