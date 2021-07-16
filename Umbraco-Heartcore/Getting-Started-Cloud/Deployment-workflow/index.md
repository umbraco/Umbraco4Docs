---
versionFrom: 8.0.0
---

# Deployment workflow

Having multiple environments on your Umbraco Heartcore project can be a great asset for testing and having a place where your content editors can work without interfering with anything on the Live environment.

<iframe width="800" height="450" src="https://www.youtube.com/embed/popG59FrE9U?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

When working with multiple environments, it is very important to follow the correct workflow. In this article, you can learn more about the workflow and the best practices for using it.

The workflow uses a classic "left to right" deployment model, meaning that changes are first made on the Development environment and then deployed to the Staging or Live environment. The workflow depends on whether you have a Staging environment which further depends on the plan your project is on.

![Left-to-right deployment model](images/left-to-right.png)

We recommend that you do not work with Document Types directly on the Live environment. This should, as a rule of thumb always be done on the "lowest most" environment: the Development environment.

The deployment approach is divided into two steps: 

1. Structure changes like Document Types and Data Types are **deployed** via the project portal page
2. Content and Media are **transferred/restored** via the Umbraco Backoffice

Each step is defined in more details in the following articles.

## [Structure deployment](Structure-deployment)

## [Content transfer/restore](Content-transfer)
