

# Node.js Workshop


## Prerequisits
Download and install sublime-text editor (or use your favorite one): https://www.sublimetext.com/

Download and install node.js: https://nodejs.org/en/

## Setup
In your console, run the following commands:

```
npm install -g nodemon

npm install -g express-generator

express --view="ejs" myapp

cd myapp
```

In order to reload your app automatically after every change, edit the package.json, where:
```
"scripts": {
    "start": "node ./bin/www"
  }
```
just replace '**node**' with '**nodemon**'.

<br />

In order to install the dependencies from the package.json, run:

```npm install```

And start the node process:

```npm start```


Now go to http://localhost:3000/ in your browser.

<br />

# Movies watchlist application

In this demo you'll build a simple application to manage your must-watch movie list, consists of basic CRUD (Create, Read, Update, Delete) operations.

#### Add 'Watchlist' page


Create a new file under ‘myapp/views/watchlist.ejs’ with the following HTML code, that shows all the movies:
```
<!DOCTYPE html>
<html>
  <head>
    <title>Movies</title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1>My watchlist</h1>
    <table><tr>
<% 
    var html = '';

        for (var row in watchlist) {
            html += '<tr>\r\n';
            html += '<td><a href="http://www.google.com/search?q=' + watchlist[row].moviename + '">' + watchlist[row].moviename + '</a></td>\r\n';
            html += '<td>' + watchlist[row].year + '</td>\r\n';

            html += '<td><form method="post" action="/deletemovie"><input type="hidden" name="moviename" value="' + watchlist[row].moviename + '" /><input type="submit" Value="Delete"></form></td>\r\n';

            html += '<td><a href="/updatemovie?selectedmovie=' + watchlist[row].moviename + '">update</a></td>\r\n';

            html += '</tr>\r\n';
        }
%>
 <%- html %>
  </tr></table>

<br/>
    <a href="/newmovie">Add Movie</button>
  </body>
</html>
```
<br />
Under myapp/routes/index.js add an array to hold the movie list:

```
movies = [
  { "moviename" : "Lion King", "year" : "1994" },
  { "moviename" : "The Avengers", "year" : "2012" }]
```
<br />
And add a route to handle the browser's GET request for movies:

```
/* GET watchlist page. */
router.get('/mywatchlist', function(req, res) {

  res.render('watchlist', {
    "watchlist" : movies
  });
});
```

<br />
Now go to http://localhost:3000/mywatchlist in your browser.
<br />

#### Add ‘new movie’ page

Create a file ‘myapp/views/new_movie.ejs’ with the following html code for adding a new movie:
```
<!DOCTYPE html>
<html>
  <head>
    <title>Add Movie</title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <form id="formAddMovie" name="addmovie" method="post" action="/addmovie">
      <input id="inputMovieName" type="text" placeholder="movie" name="moviename" />
      <input id="inputYear" type="text" placeholder="year" name="year" />
      <button id="btnSubmit" type="submit">Submit</button>
    </form>
    <br>
    <a href="/mywatchlist">Go back to watchlist</button>
  </body>
</html>
```
<br />

Under myapp/routes/index.js add the route which when called, responds this page to the browser:
```
/* GET New Movie page. */
router.get('/newmovie', function(req, res) {
    res.render('new_movie', { title: 'Add New Movie' });
});
```
<br />

Add a route to handle the browser's POST request, containing the movie name and year (Called when the user clicks on "Submit"). 
The function should save the movie in the array and redirects the user back to the watchlist :
```
/* POST to Add movie */
router.post('/addmovie', function(req, res) {

    // Get our form values. These rely on the "name" attributes
    var movieName = req.body.moviename;
    var year = req.body.year;

    // TODO: add the movie to 'movies' array
    // 
    //


    // Send a response back to the browser, redirecting it to the updated watchlist
    res.redirect("mywatchlist");
});
```

Complete your code under the 'TODO' line.

#### Delete movie
Add the code for deleting the movie from the array and redirecting the user back to the watchlist:
```
/* POST to Delete movie */
router.post('/deletemovie', function(req, res) {

    // Get the form value. These rely on the "name" attribute
    var movieToDelete = req.body.moviename;

    // TODO: delete the movie from 'movies' array
    //
    //
    //
    

    res.redirect("mywatchlist");
});
```
Complete your code under the 'TODO' line.
#### Update movie
Create a file ‘myapp/views/update_movie.ejs’ with the following html code for updating an existing movie:
```
<!DOCTYPE html>
<html>
  <head>
    <title>Update Movie</title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %>: <%= movieToUpdate %></h1>
    <% 
    var html = '';
            html += '<form id="formUpdateMovie" name="updatemovie" method="post" action="/updatemovie">\r\n';
            html += '<input type="hidden" name="selectedmovie" value="' + movieToUpdate + '" />'
            html += '<input id="inputMovieName" type="text" placeholder="movie" name="moviename" />\r\n'
            html += '<input id="inputYear" type="text" placeholder="year" name="year" />\r\n'
            html += '<button id="btnSubmit" type="submit">Submit</button></form>\r\n'
    %>
    <%- html %>
    <br>
    <a href="/mywatchlist">Go back to watchlist</button>
  </body>
</html>
```
<br />

Under myapp/routes/index.js add the route which when called, responds this page to the browser:
```
/* GET Update Movie page. */
router.get('/updatemovie', function(req, res) {
    res.render('update_movie', { title: 'Update Movie', movieToUpdate: req.query.selectedmovie });
});
```

Now it's your turn - create the route for the 'updatemovie' POST requests and implement the code for updating an element in the movies array.

Good luck!



