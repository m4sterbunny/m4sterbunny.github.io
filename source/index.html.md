---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to my API documentation demo site.

<aside class="notice">
Some clients have API schemas that will become public, others have gated API documention. Even those that may eventually have their API schema made public, if the full review cycle is not yet complete, they may not want the entire schema in the wild yet. This showcase represents my solution to both challenges.
</aside>



# Client

## Cloud RF-API

[Cloud RF](https://docs.cloudrf.com/) provides its customers with a dashboard SaaS and direct access to the API that underpins it. My task was to take a non-spec-compliant API schema (in YAML format) and:

- improve the endpoint and schema descriptions
- ensure the API schema complies with Open API 3.0
- make calls to the endpoints and add descriptions of the responses/payloads to the schema
- start high-level docs

As is typical with such a project, I was able to identify inconsistencies in logic such as:
- default value that lay outside of min/max value
- integer that should be a float


<aside class="notice">
Cloud RF's api creates electromagnetic and geospacial modelling to assist with planning radio transmitter and antena properties).
</aside>


### Tools Used

Parameter | Description
--------- | -----------
Pandoc | I have updated the YAML file and written pages in Markdown for a Pandoc-generated static site
Stoplight | I used Stoplight locally to assist with error identification and ensuring compliance with spec
GitHub | The main communications channel via PRs
GitBash | For version control via GitHub


#### API Schema Extract




