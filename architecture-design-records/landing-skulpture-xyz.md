**DRAFT**

## Introduction & goals
### Requirements
`landing` is the landing page for https://skulpture.xyz and it comprises of a lambda function deployed to a GCP VM and a frontend which is deployed to Cloudflare Pages. 

Landing pages are the first point of contact for someone who is new to an organization and the main motivation for them is to introduce the organization and increase conversions. As such, landing pages are usually simple in terms of business logic and API implementation, they're usually frontend focused projects.

There are many website builders available for the frontend such as Squarespace and Wix which allows for WYSIWYG editing of the frontend. In this case [Astro](https://astro.build) was chosen instead primarily due to familiarity which eases maintenance and using a code first approach also makes versioning changes simpler.

Enquiries are stored in a Google Sheet and any associated files are stored on Google Drive so that it is easy to work with.

### Quality goals
- Maintainability: It should be simple to adapt and update designs
- Efficient: Expected and should be able to handle a potentially large amount of traffic
- Accessible: Data should be easy to access, and work with

### Stakeholders
- [Naveen Mathew](mailto:naveen@skulpture.xyz)

setup:
- lambda to dump to gsheets and cf pages proxied by go
- cf pages proxy in go instead of nginx so that nginx config stays simple and go proxy not too dificult to setup
