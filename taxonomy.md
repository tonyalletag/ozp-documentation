# OZONE Taxonomy #
## UI Elements ##

**Application:** A web-based tool that uses the browser as a client

**Dashboard:** A layout on the Webtop where you can configure widgets and/or web applications for a specific task. Use it to organize your tools. You can make several dashboards (and reuse widgets and web apps on them, consider making different dashboards to correspond with different workflows.) 

**Folder:** A collection of listings saved from the Center and used as applications and/or widgets within the HUD or Webtop

**Listing:**  An entry in the Center (e.g. a widget, application, etc.) Users bookmark listings to their HUD where they can group them into folders or open the listings directly into Webtop from Center. 

<br>
**Bookmark** Links in the HUD that reference listings users saved from Center. Use bookmarks to launch or add listings from HUD to Webtop. 

**System Help** Identified by a question mark icon, system help houses instructions for using the system. Find widget- or web app- specific help under their respective drop-down menus in HUD. 

**Notifications** Located on the top-right of the global toolbar, Center stewards use notifications to alert users of potential outages, updates, etc.

**Tag:**  a user-defined keyword used for describing a widget or application. The listing creator must add a tag when they fill out the Create Listing form, then the listing owner(s) and stewards can add and update tags. 



## Software Architecture##

**OZONE Platform (OZP):** A centralized software-as-a-service framework that allows for running web applications, and the communications between them, entirely within the browser 

<br>

**Center:** OZONE's app store component. This is where users find tools to do their jobs. These tools, known as listings, consist of widgets, web applications, etc. Users can search for them using filters, search terms and categories. 

**Inter-Widget Communications (IWC):**  Communication bus within the browser used for widgets and applications to share data within the browser.

**Metrics:** Business analytics designed to gather data from the OZONE software package for software and system maintenance and app development

**Webtop:** Similar to the desktop on a personal computer, the webtop is the workspace within OZONE. From Webtop, users do their work using dashboards, widgets, web apps, etc.

