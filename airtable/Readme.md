![Database](images/airtable.png)

Airtable is an awesome tool for centralising data and running multiple different segments of your business. However, sometimes we can have so much tabular data that it is hard to really grasp week-to-week developments.

It is possible (on the pro plan) to build charts using Airtable's code blocks. Another option is to export to excel. But to really supercharge your analytics, this work should be as automated as possible. So this guide will show how you can fetch Airtable data from a Jupyter Notebook, manipulate and visualise this data, all in Python.

This post is for you if:

 - You are already using Airtable for some specific knowledge management - your sales CRM, for example - but are frustrated with the built-in data visualisation features.
 - If you think your team could benefit from a more automated process for tracking metrics that impact decision-making.

## Getting Started

Airtable Python uses Requests, which you can install by running:

```
    conda install -c anaconda requests -y
```

Note that you will need to generate an API key for your account if you haven't already. You can do this in your Airtable account settings.

It is bad practice to have your API keys hard-coded in your script. Instead, we can export a global variable in our ```~/.bash_profile```. Copy your API key to the clipboard. Open your bash profile in your preferred editor and enter the following:

```
    export 'AIRTABLE_API_KEY' = "API_KEY"
```

replacing "API_KEY" with the string you just copied.

From the command line, run:

```
    source ~/.bash_profile
```

We are now ready to start using Airtable Python.

### Install Anaconda

Now, for this guide to be portable & runnable on any machine I first need to create a virtual environment. Check out our previous guide on installing the Anaconda distribution:

https://docs.kyso.io/guides/jupyter-with-anaconda


### Creata A Virtual Environment

We'll run the following commands to create a python3.7 environment. To create our virtual environment:

```
    conda create -n mongodb-playground python=3.7 -y
```

and start it:

```
    conda activate mongodb-playground
```

## Additional Installs

Later on, once we have pulled in our data, we are going to want to graph it. We usually recommend using plotly in python, given its relatively simple syntax and interactivity.

For use in JupyterLab, we need to install the jupyterlab and ipywidgets packages:

```
    conda install -c conda-forge jupyterlab-plotly-extension -y
```

Plotly has a wrapper for pandas (data manipulation library) called *Cufflinks*.

The following two commands will get you up and running:

```
    conda install -c plotly plotly
    conda install -c conda-forge cufflinks-py
```

Now we can spin up Jupyterlab with:

```
    jupyter-lab
```

On launch, you might be a prompt for a recommended build - the jupyterlab-plotly-extension install we ran above. Click Build and wait for it to be completed.

## Fetching Our Data

For the purpose of this guide we will work with one of Airtable's sample templates - the *Sales Pipeline* base. Let's fetch the data:

```
    from airtable import airtable
    at = airtable.Airtable('BASE_ID', 'AIRTABLE_API_KEY')

    response1 = at.get('Sales Deals')
    response2 = at.get('Sales Reps')
```

where 'AIRTABLE_API_KEY' is the global variable we just set in our bash profile and 'BASE_ID' is the ID for the specific base we are working with. Airtable's REST API interface can be found here: https://airtable.com/api, where you can access a list of all your Airtable bases. Each Airtable base will provide its own API to create, read, update, and destroy records.

We have pulled in 2 different responses the correspond to the two tables inside our Sales Pipeline base.

## Manipulating Our Data

We can convert the response into a Pandas DataFrame, which will make it a lot easier to manipulate and visualize our data.

```
    import pandas as pd
    sales = pd.DataFrame(response1, columns=response.keys())
    reps = pd.DataFrame(response2, columns=response.keys()) 

    sales = pd.concat([sales.drop(['records'], axis=1), sales['records'].apply(pd.Series)], axis=1)
    sales = pd.concat([salesdf.drop(['fields'], axis=1), sales['fields'].apply(pd.Series)], axis=1).fillna('')

    reps = pd.concat([reps.drop(['records'], axis=1), reps['records'].apply(pd.Series)], axis=1)
    reps = pd.concat([reps.drop(['fields'], axis=1), reps['fields'].apply(pd.Series)], axis=1).fillna('')
```

Above we've assigned our responses to variables and read them into their respective pandas DataFrames. The response from Airtable comes in the form of an ordered dictionary, which we've had to parse above.

## Visualizing Our Data

Now it is time to start gaining insights from our data, but first up:

**Define your business questions:**

What business questions do you want to answer? We've laid out some example questions for our sample sales pipeline data:

1. What is the value of our closed deals? Who has secured the most revenue?
2. What stages are other deals at in the pipeline? What is the weighted value of these deals?
3. What is our actual monthly revenue? What are our forecasted targets?
4. How are our sales reps doing? Are they hitting their quotas?

## Conclusion

Ok, so we've answered some basic questions about our business metrics, and you now have easy-to-build data dashboards right at your fingertips! It's time to post this notebook to our team's Kyso workspace so everyone can learn from our insights and apply them to their respective roles.

- We can push to our organisation's Github repository from our local machine and sync this repo with Kyso. Simply follow the guide linked below:

https://docs.kyso.io/posting-to-kyso/connect-a-github-repo-to-kyso

You can check out this guide on Github [here](https://github.com/KyleOS/kyso-guide/airtable), with an example notebook and the python code ready for a quick set up, so you can fork the repository & edit the notebook (i.e. insert your own Airtable API key, the name of your base and table names). 

Now, every time you push a new commit, these changes will be reflected on the Kyso post, where your team can learn and collaborate. Remember - data is only useful to you if you actually use the insights gained!