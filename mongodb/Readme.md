## Graphing data from MongoDB

*This guide will walk you through how to connect to your own MongoDB instance from a Jupyter notebook, pull in your data and plot it, all with python.*

![Data Analysis](images/mongodb.png)

### Introduction

Flexible, NoSQL databases, which can handle large amounts of unstructured data and can flexibly increase or reduce storage capacity without loss, will gradually displace relational databases. 

MongoDB is on of the top picks among NoSQL databases in terms of fulfilling business requirements on fast and flexible access to the data in various spheres of development, especially where a live data prevails.

Note, I'm not going to cover installing MongoDB and setting up a database in and of itself, as there is plenty of documentation already covering this.

### Setting Global Variables

As mentioned above, it is assumed that you already have a MongoDB database running on Atlas. Under *Clusters* on MongoDB click *Connect* on the database in question. There are 3 different ways to connect to the database. If you haven't already you'll need to whitelist your own IP address and create a MongoDB user for this specific project.

Choose the second option - *Connect your application*. Choose Python as your driver and *3.6 or later* as your version. Copy the connection string to the clipboard.
Open your bash profile in your preferred editor and enter the following:

```
export 'MONGO_URI' = "YOUR_CONNECTION_STRING"
```

replacing "YOUR_CONNECTION_STRING" with the connection string you just copied. Within the connection string you'll also have to replace with your login password for the current user.

From the command line, run:

```
source ~/.bash_profile
```

### Installing Anaconda

Now, for this guide to be portable & runnable on any machine I first need to create a virtual environment. Check out our previous guide on [installing the Anaconda distribution](https://docs.kyso.io/guides/jupyter-with-anaconda).

### Creating a Virtual Environment

We'll run the following commands to create a python3.7 environment. To create our virtual environment: 

```
conda create -n mongodb-playground python=3.7 -y
```

and start it:

```
conda activate mongodb-playground
```
We'll also need the following libraries to connect to Mongo 4.0:

```
conda install pymongo==3.8 dnspython ipykernel -y
```

The next command ensures that our Jupyterlab instance connects to this virtual environment:

```
python -m ipykernel install --user
```

### Addtional Installs

Later on, once we have pulled in our data, we are going to want to graph it. We usually recommend using plotly in python, given its relatively simple syntax and interactivity.

For use in JupyterLab, we need to install the jupyterlab and ipywidgets packages:

```
conda install -c conda-forge jupyterlab-plotly-extension -y
```

Plotly has a wrapper for pandas (data manipulation library) called *Cufflinks*, which is currently having compatibility issues with plotly's latest version. A workaround for this at the moment is to downgrade plotly and install cufflinks with the following commands. Once you see how easy it is to generate graphs with Cufflinks, you'll understand why we go through this hassle. The Cufflinks guys have been working on the fix for sometime, but it's yet to emerge.

As of September 2019 this will get you up and running:

```
conda install -c conda-forge plotly=2.7.0 -y
conda install -c conda-forge cufflinks-py -y
```

Now we can spin up Jupyterlab with:

```
jupyter lab
```

On launch, you might be a prompt for a recommended build - the jupyterlab-plotly-extension install we ran above. Click *Build* and wait for it to be completed.

***

## In Jupyter

### Connecting to MongoDB

Let's first import our required libraries:

```
import os # to create an interface with our operating system
import sys # information on how our code is interacting with the host system
import pymongo # Python distribution for working with the MongoDB API
```

Then we connect to our MongoDB client:

```
client = pymongo.MongoClient(os.environ['MONGO_URI'])
```

note that we can call 'MONGO_URI'because we've set the the connection string as an environment variable in our `~/.bash_profile`. 

### Accessing our data

Now let's access a database, in this case, *sample supplies*.

```
db = client.sample_supplies
```

A collection is a group of documents stored in a MongoDB database, roughly the equivalent of a table in a relational database. Getting a collection works the same as accessing a database like we did above. In this case, our collection is called sales.

```
collection = db.sales
```

Let's test if we were successful by getting a single document. The  following method returns a single document matching our query.

```
test = collection.find_one()
```


### Loading our data into Pandas

Pandas provides fast, flexible, and expressive data structures designed to make working with “relational” or “labeled” data both easy and intuitive, and is arguably the most powerful and flexible open source data analysis / manipulation tool available. 

We can convert our entire collection of data into a pandas DataFrame with a single command:

```
data = pd.DataFrame(list(db.sales.find()))
```

Some of our columns in the DataFrame are still formatted as dictionaries or PyMongo methods. For the purpose of this guide, let's look at the *customer* column - this is a dictionary containing 2 key-value pairs for each customer's age and gender. 

We need to split this column such that age and gender become their own columns. For this we can use the .apply(pd.Series) method to convert the dictionary into it's own DataFrame and concatenate it to our existing DataFrame, all in one line. 

```
df = pd.concat([data.drop(['customer'], axis=1), data['customer'].apply(pd.Series)], axis=1)
```

We could do something similar with the *items* column, for example, but that's beyond the scope of this guide.

### Plotting our data

Ok, let's start plotting our data to answer some basic questions. First, we'll import our plotting libraries.

```
import plotly.plotly as py
import plotly.graph_objs as go
import plotly
from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
import cufflinks as cf
cf.set_config_file(offline=True)
```

Documentation for the two libraries are below:

[plotly](https://plot.ly/python/)

[cufflinks](https://plot.ly/python/v3/ipython-notebooks/cufflinks/)

**Define your business questions**

What business questions do you want to answer? We've laid out some example questions for our sample data:

1. What is the average customer satisfaction rating by store - and is this affected by the method of purchase, whether it's carried out over the phone, in store or online?

```
df.groupby(['storeLocation', 'purchaseMethod'], as_index=True)['satisfaction'].mean().unstack().iplot(
    kind='bar', mode='group', title='Average Customer Satisfaction by Store')
```

![Data Analysis](images/mongodb_guide1.png)

2. What about the number of purchase orders received, broken down by gender - are there any major differences?

```
df.groupby(['gender', 'purchaseMethod'], as_index=True)['_id'].count().unstack().iplot(
    kind='bar', mode='group', title='Purchase Orders by Gender')
```

![Data Analysis](images/mongodb_guide2.png)

3. What is the age distribution of all of our customers?

```
df['age'].iplot(kind='hist', color='rgb(12, 128, 128)', opacity=0.75, bargap = 0.20,
               title="Age Distribution of our Customers", xTitle='Age', yTitle='Count')
```

![Data Analysis](images/mongodb_guide3.png)

Ok, so we've answered some basic questions about our business metrics. It's time to post this notebook to our team's Kyso workspace so everyone can learn from our insights and apply them to their respective roles. 

- We can push to our organisation's Github repository from our local machine and sync with Kyso:

[Connect Github repositories with Kyso](https://docs.kyso.io/posting-to-kyso/connect-a-github-repo-to-kyso)

- Since we're already in Jupyterlab, we can install Kyso's publishing extension and post our notebook directly to Kyso from here:

[Use Kyso's Jupyterlab extension](https://docs.kyso.io/posting-to-kyso/kysos-jupyterlab-extension)

- We can also manually upload the notebook in the Kyso app itself. 