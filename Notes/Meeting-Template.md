---
created: <% tp.file.creation_date("YYYY-MM-DD") %>
type: meeting
status: past

date: <% tp.file.creation_date("YYYY-MM-DD") %>
location: 
attendees: 
summary: 
---

# <% tp.file.title %>

|                      Created                      |                      Last Modified                      |
|:-------------------------------------------------:|:-------------------------------------------------------:|
| <% tp.file.creation_date("YYYY-MM-DD @ HH:mm") %> | <%+ tp.file.last_modified_date("YYYY-MM-DD @ HH:mm") %> |
- **🏷️Tags** :   #<% tp.file.creation_date('MM-YYYY') %> #meeting
<% await tp.file.move("/3-Permanent/38-Meetings/" + tp.date.now("YYYY-MM-DD") + " " + tp.file.title) %>
## Attendees
* 


## Agenda
* 


## Log
* 


## Next Actions
* 

