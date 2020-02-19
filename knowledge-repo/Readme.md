![Knowledge Transfer](images/knowledge-repo.png)

## An Issue Of Knowledge Management

Data science is exploding; more and more organizations are using data to power, well, everything. This explosion is the result of a huge increase in the amount of data collected to drive business value.

However, a challenge that continues to persist is the tooling. Data-science tools are very technical, meaning it is difficult to convert these tools to more readable formats, making reports difficult to share. This causes a lot of locked business value because not everyone in the company learns from the insights generated.

What is needed is a central knowledge management space, where data scientists and analysts can simply post their reports, and do so in a way that is digestible to non-technical stakeholders, so everyone within the company can make better data-driven decisions.

## Airbnb's Knowledge Repository

In 2016, Airbnb open-sourced their [Knowledge Repository](https://github.com/airbnb/knowledge-repo), built to tackle their own needs of scaling internal knowledge sharing and management, and to democratise data access for all in the company.

The knowledge repository acts as a curated knowledge sharing and management platform, developed with the following aims in relation to posted content:

- Reproducibility.
- Quality control.
- Consumability - any analysis should be understandable to others besides the author.
- Discoverability.
- Learning - other technical team members should have the ability to learn new tools and techniques.

Airbnb’s knowledge repository has been revolutionary for sharing and benefiting from data science at scale. Our team here has used it in the past for internal reporting and it sped up every aspect of our data analytics. 

However, the focus seems to be specifically on facilitating the sharing of knowledge between data scientists and other technical roles - we wanted to create a space where everyone throughout the company can apply generated data insights to their respective roles. We also felt we could improve the overall user experience, and we had a lot of additional feature ideas to better scale knowledge sharing & foster a more data-driven culture within companies.

## Our Solution: Kyso For Teams

[Kyso](https://kyso.io/) is our own next-generation knowledge repository, a central hub where data scientists & analysts can post reports so everyone on the team can read and learn from them. Specifically, Kyso renders Jupyter and R notebooks elegantly so that everyone in the company can read them and thus bridges the gap between the data team and the rest of the company. 

We have full Git integration so that teams can deploy notebooks to Kyso automatically when they commit to Github so that they retain version control and reproducibility. You can even write articles from scratch on the web-app itself with our Markdown editor. 

Think of it like Medium — an elegant, internal, blogging platform, but with the added benefit of being compatible with technical tools like Github, Jupyter & R notebooks (which is only the beginning). 

Getting non-technical data stakeholders to better use and interact with data is the problem that Kyso looks to solve - to facilitate wider communication of data insights so these can be applied in all roles across a company. This is why we built Kyso. Regardless of the tool used by the individual data scientists, there should be one common framework for communicating and collaborating effectively on finished reports.

## How It Works

In building Kyso, we focused on a few points to achieve this:

- **Easy user management:** App-based team access controls for managing who can post, edit or simply consume content.
- **Easy of use:** A simple, but aesthetic, UI and comprehensive documentation.
- **Readability:** Insights must be presented in a well-documented format. Analyses are rendered as beautiful data blogs, in which code can be both hidden and shown, which is digestible to both technical and non-technical readers. All interactive graphics libraries like plotly & bokeh work nicely.
- **Easily Searchable.** A comprehensive tagging system for managing your projects & content upvoting, which acts as an auto-indexer for research topics and posts, keeping content structured and current for organizing and discovering the work of other contributors.
- **Flexible publishing:** Write articles, upload charts, code, datasets, links, and Jupyter & R notebooks. Our Github integration also means that the data team members don't need to change their existing workflows.
- **Open collaboration:** Blog-style commenting for feedback, suggestions, and discussion. We are currently working on a two-way Q&A system that will involve non-technical stakeholders like C-level executives and other managers & allows data teams to react faster to what the rest of the company needs.
- **Reproducibility:** Git integration means that analyses can be easily updated & extended automatically so team members can duplicate results & set new projects in motion much faster.

## Conlusion

As companies continue to accelerate their data science & analytics capabilities, it is important they ensure these generated insights are disseminated to a wider audience within the company, such that each & every role within a business is benefitting from this knowledge, which will drive more business value. Kyso is our attempt to make that process easier and more efficient.