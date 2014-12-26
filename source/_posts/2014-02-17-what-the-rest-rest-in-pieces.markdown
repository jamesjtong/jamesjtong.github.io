---
layout: post
title: "What the \"REST\"? REST in Pieces "
date: 2014-02-17 11:54
comments: true
categories: ["REST", "API", "REST ARCHITECTURE, RESTFUL API", ]
---



***A short guide breaking down the key components of RESTful architecture***

![REST Sofa](http://www.digsdigs.com/photos/Luxury-Classic-Sofa-and-Armchairs-Imperial-by-Vimercati-Media-5-554x377.jpg "REST Sofa")

So until last week, I had thought I understood REST. I thought that for an app to be RESTFUL, all an app had to do was have logical URIs*. Although this is a big part of it, there is so much more to it. Roy Fielding, the man who introduced REST, wrote a 168 page dissertation ([link](http://www.ics.uci.edu/~fielding/pubs/dissertation/fielding_dissertation.pdf)) explaining REST. Although that is a good read in itself, the main concepts of REST can be explained in something much shorter and I hope to do that in this blog post.

#**So let's talk about why REST is important.**   
Back when the web was new, web apps didn't talk to each other much. It was mainly humans who talked to a web app. Times have changed. Web apps need to talk to each other. They talk to each other through APIs. An API is the interface implemented by an application which allows other applications to communicate with it.

For web apps to have a smooth transition in talking to each other, the APIs of web apps need to follow some sort of architectural pattern. REST is just one of these patterns that is extremely popular and it makes app to app communication easy.

 Need an example of app to app communication? 

Think of how a lot of apps are just mashups that incorporate various APIs like those of Youtube, Google Maps, Twitter or Facebook and then integrate them to do amazing things.  

Let's say you want to create an app where you use Facebook's api to get "likes" data on different categories, then use that information to find areas that you might like around your area. This app wouldn't be very difficult to execute because Facebook and Google follow RESTful conventions so it is easy to talk with data. You can just get a json object from a get request and then use specific properties of that json object to then talk to the Google Maps API. 
  
#**So what are some of the guidelines of REST?**  
***(I am going to assume the usage of the http/https protocol, for simplicity's sake and the fact that it is by far the most common protocol today)***

1. **You have Resources and URIs that make sense**  
   You should have resources and those resources should be properly named and represented with the URI. This is probably best explained with examples.  
  ***example1*** http://nbastalk.com is a prolific and dynamic web site where you can view a nba player's recent social media feed (twitter/facebook), youtube videos, and recent sports statistics. To view a certain player's (players is a resource) profile, all you would  need to go is http://nbastalk.com/carmelo-anthony  
  That is very logical and makes sense, while http://nbastalk.com/fasfs1/ssdfas3434/155 to view Carmelo's profile does not. Try them both, you may be in for a fun surprise.  
  ***example2*** If you have nested resources, the same rule applies. If you have states with different political groups and you wanted to see all the political groups for NY, the URL of the listing of the political groups resource should look something like this:  
  http://fake-political-data-api-website.com/ny/political-groups
     
2. **Uniform Interface (Correct usage of the HTTP verbs get, post, put, delete) **  
    * get requests are expected to retrieve information
    * post requests are expected to create new data
    * put requests are expected to update data
    * delete requests are expected to delete data
    * (fyi head requests are just like get requests except that the server returns an empty body (just returns the header  information))  

    Using these requests, the server is able to deliver and receive information without the client having to know how the information is being constructed. This is encapsulation at its best. Through a uniform interface, the server abstracts away the inner complexities of the application.
  
3. **Stateless, Hypertext As The Engine Of Application State (HATEOAS)**  
 Every individual request from the client to the server must contain all the information necessary information to understand the request. Everything necessary should be in the response body and head. You cannot rely on a server to store context (think sessions, cookies and pstore as things that shouldn't be relied on). Each individual response from the server should have links with what you can do with that resource.
 
        
        Request
        GET /
        Accept: application/json+userdb
        Response
        200 OK
        Content-Type: application/json+userdb

        {
            "version": "1.0",
            "links": [
                {
                    "href": "/user",
                    "rel": "list",
                    "method": "GET"
                },
                {
                    "href": "/user",
                    "rel": "create",
                    "method": "POST"
                }
            ]
        }
        
4. **Cacheable**  
Resources should be cachable when possible as this decreases response time from server to client. The protocol should specify which resources can be cached and for how long. 

5. **Free format/Does not specify implementation details**  
  Let the user dictate what kind of mime the info is passed in. Don't force a user to have to parse your xml into   json. Worse, imagine if the user is forced to change his app to handle specific data just to use your api. If your api is set up correctly, the client should be able to specify what kind of response they want.  
  Try submitting a GET request to (or just go to the following urls) the following:  
  http://www.reddit.com/.json    
  http://www.reddit.com/.xml    
  
See below how the Rails framework does this for us:
```ruby
def index
  @users = User.all
  respond_to do |format|
    format.html
    format.xml { render :xml => @users }
    format.json { render :json => @users }
  end
end
```





#**CONCLUSION**
So next time you build an API, don't rest until you make sure your API is RESTful.  Or you can always go the SOAP (a well engineered, but much more complex architectural pattern) route but be prepared to your hands dirty first as it is a lot more complex.

![Getting Dirty and using SOAP](http://img0.etsystatic.com/015/0/7498614/il_340x270.467321836_3890.jpg "SOAP")

THIS IS AN EXAMPLE OF A SOAP REQUEST
    <?xml version="1.0"?>
    <soap:Envelope
    xmlns:soap="http://www.w3.org/2001/12/soap-envelope"
    soap:encodingStyle="http://www.w3.org/2001/12/soap-encoding">
     <soap:body pb="http://www.nbastalk.com/carmelo-anthony">
      <pb:GetUserDetails>
       <pb:UserID>12345</pb:UserID>
      </pb:GetUserDetails>
     </soap:Body>
    </soap:Envelope>

THIS IS REST  
    http://nbastalk.com/carmelo-anthony/UserDetails/12345



**Good links to checkout if you want to learn more about REST:**  
1. [Stackoverflow thread](http://stackoverflow.com/questions/671118/what-exactly-is-restful-programming/671132#671132)  
2. [Roy Fielding's Dissertation](http://www.ics.uci.edu/~fielding/pubs/dissertation/fielding_dissertation.pdf)  
3. [Steve Klabnik's post on REST](http://timelessrepo.com/haters-gonna-hateoas)  
4. [Pretty good post explaining APIs](http://www.makeuseof.com/tag/api-good-technology-explained/)  

*
Difference between URI and URL
: the difference between a uri and an url is that the url includes an "access mechanism protocol", or "network location", e.g. http:// or  ftp://)
