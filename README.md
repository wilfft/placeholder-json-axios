### A simple application that consumes Json data from https://jsonplaceholder.typicode.com/ using Axios.

This application show some posts brought to jsonplaceholder and fetche my post list.
When a post is clicked, a request has send and brings full post info, from http and feed fullpost page.

I also have the possibility to delete my fullpost.
And too, write a new post and send data to http.

Every assyncronous details were thought to work perfectly.
Some feature to avoid leak of perfomance was adopted, how to only send a new request IF the new request data is diferent from old data.

to run:

1. Run "npm install" in the extracted folder
2. Run "npm start" to view the project
