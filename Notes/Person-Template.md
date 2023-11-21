---
created: <% tp.file.creation_date("YYYY-MM-DD") %>
type: person
company: 
location: 
title: 
email: 
website: 
aliases: 
---

# <% tp.file.title %>
|                      Created                      |                      Last Modified                      |
|:-------------------------------------------------:|:-------------------------------------------------------:|
| <% tp.file.creation_date("YYYY-MM-DD @ HH:mm") %> | <%+ tp.file.last_modified_date("YYYY-MM-DD @ HH:mm") %> |
- **🏷️Tags** :   #<% tp.file.creation_date('MM-YYYY') %> #person
<% await tp.file.move("/3-Permanent/37-People/" + tp.file.title) %>
## Notes
* 

## Meetings
```dataview
TABLE date as Date, summary as Summary, attendees as Attendees
FROM "/3-Permanent/38-Meetings/"
WHERE contains(file.outlinks, [[<% tp.file.title %>]])
SORT meeting_date DESC
```


## Scratch Space



## Details
### History


### Family
* 


### Personality

