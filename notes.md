# Notes on Multiple Regions

I concentrated on explaining how setting up multi-region PostgreSQL and Redis works on a high level. Since this is a how-to guide. I think an overview for context is important and then specific steps about how to go about adding regions.

I assumed that the person using this guide had an existing application that utilizes both Postgres and Redis and they are interested in adding other regions. I left out details on how to setup a PostgreSQL and Redis cluster and linked to existing docs that demonstrated the specific steps incase someone was setting up from scratch.

Here I struggled a bit with what details to leave out without being repetitive. Going into more detail means replicating some of the existing docs and I felt the focus of this guide is to really give the steps needed to add regions.

Another confusing bit was the architecture of an application that uses both Redis and PostgreSQL. I haven't used Redis before but from reading the docs. I understood that its a key/value store that can be utilized for such things as session management etc. I assumed that for more complex read and write operations you may want to fall back to the database. So my idea of a multi-region architecture that utilizes both Redis and PostgreSQL was to have replicas for both PostgreSQL and Redis. The details about how you chose between when to use Redis or PostgreSQL are not there because I am not sure how it would work.

I added a brief note on how the proposed architecture may not work for certain types of applications i.e write heavy and included links to posts explaining in greater detail how distributed PostgreSQL and Redis work
