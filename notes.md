# Notes on Multiple Regions

I concentrated on explaining how setting up multi-region PostgreSQL and Redis works on a high level. Since this is a how-to guide. I think an overview for context is important and then specific steps about how to go about adding regions.

I assumed that the person using this guide had an existing application that utilizes both Postgres and Redis and they are interested in adding other regions. I left out details on how to setup a PostgreSQL and Redis cluster and linked to existing docs that demonstrated the specific steps incase someone was setting up from scratch.

Here I struggled a bit with what details to leave out without being repetitive. Going into more detail means replicating some of the existing docs and I felt the focus of this guide is to really give the steps needed to add regions.

Another confusing bit was the architecture of an application that uses both Redis and PostgreSQL. I haven't used Redis before but from reading the docs. I understood that its a key/value store that can be utilized for such things as session management etc. I assumed that for more complex read and write operations you may want to fall back to the database. So my idea of a multi-region architecture that utilizes both Redis and PostgreSQL was to have replicas for both PostgreSQL and Redis. The details about how you chose between when to use Redis or PostgreSQL are not there because I am not sure how it would work.

I added a brief note on how the proposed architecture may not work for certain types of applications i.e write heavy apps and included links to posts explaining in greater detail how distributed PostgreSQL and Redis work

# Notes on Static Asset Caching

Since this is reference documentation. I tried to focus more on providing a clear explanation of of how static asset caching works. My explanations were derived from what I picked from the existing documentation. I could have missed some deeper technical details about how exactly static asset caching works in Fly's network but I think the overview and generally the whole post provides a good high level overview.

I left out some details to demonstrate how symlinks are not supported. An example is shown in the actual [documentation](https://fly.io/docs/reference/configuration/#the-statics-sections). I felt that an explanation was clear enough for reference purposes.

I included a caveat that static asset caching isn't a replacement for a CDN. I provided a simple reason which may not have been deep technically but I wasn't sure how I could guide the user on assessing the need for a real CDN. I understand that static asset caching is targeted at people who have a common need of deploying static assets from their normal full stack applications. It wasn't 100% clear to me whether you could do something like having a frontend framework deployed as static assets and having your backend respond accordingly I think it could be done but I left those details out.

# Testing the effectiveness of documentation

The way I would interpret this is by first thinking about the standard ways people test for effectiveness of any piece of content.

## Determine the objectives

The first question we have to ask ourselves is what is the purpose or goal of this documentation. Here its clear that our first objective is to create a walkthrough guide for Fly users who want to add regions to their existing applications and the second is to create reference documentation for users who may be interested in learning about how static asset caching works.

Beyond that, we may want to set big picture objectives like increasing awareness of features we offer.

## Analyzing the Traffic

how much traffic and where the traffic is coming from. Some may disagree, but ultimately we need eyes on our content and even if we have a specific audience who we are targeting for this kind of content i.e users of Fly. A traffic breakdown at each audience touchpoint gives a broad indicator of how our content is performing. For example, if we hit the frontpage of Hacker News we know that this content is atleast reaching the right audience. The six channels I would explore are.

1. Social Traffic - visitors who come to the content via social networks.
2. Organic Search Traffic - visitors who come to the content via Google or other search engines
3. Direct traffic - visitors who come to the content by typing in the url
4. Email traffic - this depends on whether we run an email list
5. Referral traffic - this depends on whether we run some kind of referral campaign
6. Paid traffic- This also depends on whether we run paid ad campaigns

Some of this channels maybe irrelevant for our case but they really just provide a starting point for potential areas we can start analyzing our traffic.

There are numerous ways to analyze traffic. I won't go into specifics for the sake of brevity.

## Track engagement metrics that correspond to our objectives

When we have identified traffic sources. We have a basic view of how our content is performing but we need to go deeper.

We may decide on

- Increasing awareness of a feature e.g static caching or running multi-region apps and helping existing users take advantage of the feature

- Providing up to date information - As new features get added documentation gets outdated. We need to make sure everything is always up to date and the users have all the information they need to utilize the latest and greatest features.

- Increasing customer acquisition - I personally had no idea that you could add regions to an app but it could be the difference that makes me decide that I should use Fly. By simply reading about multi-region apps I could be a new customer.

- Increasing customer retention and loyalty - By adding new features like static asset caching we ensure that our customers keep getting value

Lets get into metrics that correspond to each of these goals

### Increasing awareness of a feature

A successful feature awareness campaign means our users recognize a feature, its advantages and they use it in their own applications without getting stuck.

Some metrics could be

- Page views
- Comments and questions in the community forum
- Time spent on post
- Inbound links

### Providing up to date information

- Comments and questions in the community forum especially when developers are getting stuck because information is outdated.

### Increasing customer acquisition

It may sound like a bit of a stretch to link a tutorial to customer acquisition but developers'first contact with Fly.io will be with the documentation when they are trying out things. If things don't work as stipulated. The developer may leave for another platform.

So KPIs you can track

- New customers
- Top converting tutorials

### Increasing customer retention and loyalty.

- Comments and questions in community forum
- Customer lifetime value which is the measure of average revenue generated by a customer over their entire relationship.

These are some general ideas about how we can measure the effectiveness of the documentation. Every tutorial and reference has to play the primary role of informing users and helping them make the best use of the platform but its also has to fit into the bigger picture objectives of the company. Its not always clear how to do this and it may not be important.

Measuring effectiveness of the documentation can be broken down into two objectives.

1. Are developers able to use the documentation to get maximum value out of the platform. If not then the documentation is not effective. One of the most important ways we can identify this is the activity in the community forum. The documentation may not be able to cover everything but a good sign is if you point a user to a document and they are able to follow along and understand without many follow up questions.

2. Does the documentation fit into the bigger picture objectives of the company. Not all documentation has to meet this criteria but most should. For example, a how to guide on how to utilize multiple regions improves the user experience and creates enormous value for the users who can take advantage of it. You can retain, up sell or cross sell to these users.

In conclusion, measuring effectiveness of documentation is a big topic which may overlap with content marketing. For a platform which is targeting developers, you could argue that documentation is part of the product. How you approach it is going to be unique for each product.
