---
title: Maintenance
layout: page
---

EHRI maintenance overview
=========================

This document describes the procedures for development and maintenance of the EHRI systems, software modules and content. Its intended audience is developers, maintainers and managers of the system who need to get an overview of what there is and how it is maintained. 
TOC

* [Servers](#servers)
* [Software](#software)
* [Content](#content)

---

Servers <a id="servers"></a>
-------
For development and maintenance we use four servers; production, staging acceptance and test. 

*	Deployment
Exposed to the outside world, so high on security and availability (uptime). 
*	Acceptance
Should be almost identical to production in order to spot compatibility issues. Only a few external users, for acceptance testing, both software and (imported new) content. 
*	Staging
Used for importing and testing
*	Test
Only used internal, most likely diverges from production much more than the staging server because developers need to try new things on this machine first. (in practice it is hardly used, developers could perhaps use their local machine or a VM?)

Updating the OS and ‘standard’ libraries and tools is done by our ICT department. The EHRI specific software is deployed and maintained by the developers. 

---

Software <a id="software"></a>
--------
The main software (services) is the portal (website/frontend), which depends on the data store (Neo4j graph database) and a search index (Solr). Then there are several tools for importing and maintenance tasks. All code is available via Github code repositories. 


### Code
Code is maintained on Github where each developer has a personal repository, but there is also an EHRI repository. A standard Github workflow is used; developers clone repos, make changes and then do a pull request. 


### Testing and deployment
When fixing bugs or adding features, code is changed and new libraries, executable and services are build. During development, deployment on test and staging is needed. Finally everything must be deployed onto the production system. Deployment is automated by a script that handles all the details like shutting services down and bringing them up again. 


---

Content <a id="content"></a>
-------
The graph database contains all the information gathered from the participating archives, but users can also add content (maybe also change?). The content of the search index (The Solr ‘portal’ core) is derived from the content of the graph, so whenever the graph changes the index might need to be updated. 


### Backup and restore
Using a script you can make a copy (backup) of a graph and also put back (restore) a copy. Either on the same server as a rollback, or on another server. 
The latter is used in development, when copy the content from production and place it on staging so it has a recent snapshot of the production content. 
While the copy can be made from a running service, the restore can only be done on a stopped graph. Also the index needs to be updated after a restore, but the script should handle all this. 

### Statistics
For monitoring purposes the content of the graph can be queried and important statistics can be derived from the results. 


### Importing
By importing we can add new data from archives to the existing graph. 
The importer can handle only specific types of input; EAD and CSV?
Almost always a special ‘preprocessing step’ is needed. After the preprocessing the importer is first run on the staging server and its result is evaluated. When accepted, the importer can be run on production. Another way to get the imported data on production is to copy the staging (if up-to-date) to production. 
Note that for the preprocessing special software (scripts and or tools) might be needed, which are developed on ‘ad-hoc’ basis. 

