# Putting it All Together: Client-Server Communication

## Learning Goals

- Understand how to communicate between client and server using fetch, and how
  the server will process the request based on the URL, HTTP verb, and request
  body
- Debug common problems that occur as part of the request-response cycle

## Introduction

Just like the last lesson, we've got code for a React frontend and Rails API
backend set up. This time though, it's up to you to use your debugging skills to
find and fix the errors!

To get the backend set up, run:

```console
$ bundle install
$ rails db:migrate db:seed
$ rails s
```

Then, in a new terminal, run the frontend:

```console
$ npm install --prefix client
$ npm start --prefix client
```

Confirm both applications are up and running by visiting
[`localhost:4000`](http://localhost:4000) and viewing the list of toys in your
React application.

## Deliverables

In this application, we have the following features:

- Display a list of all the toys
- Add a new toy when the toy form is submitted
- Update the number of likes for a toy
- Donate a toy to Goodwill (and delete it from our database)

The code is in place for all these features on our frontend, but there are some
problems with our API! We're able to display all the toys, but the other three
features are broken.

Use your debugging tools to find and fix these issues.

There are no tests for this lesson, so you'll need to do your debugging in the
browser and using the Rails server logs and `byebug`.

**Note**: You shouldn't need to modify any of the React code to get the
application working. You should only need to change the code for the Rails API.

As you work on debugging these issues, use the space in this README file to take
notes about your debugging process. Being a strong debugger is all about
developing a process, and it's helpful to document your steps as part of
developing your own process.

## Your Notes Here

- Add a new toy when the toy form is submitted

  - How I debugged:
  When I first submitted the form, Chrome's devtools showed a status: 500 internal server error for the post request. The exception raised showed; `<NameError: uninitialized constant ToysController::Toys>`. Examining the `create` method in the `toys_controller`, I noticed a typo where the variable `toy` is being created as an instance of the `Toys` class. Our class name should be singular - i.e. the class should be `Toy` rather than `Toys` (this is reflected in the `Toy` model also).

  Correcting this typo within the `create` method fixed the issue.  

- Update the number of likes for a toy

  - How I debugged:
    First, I went into the browswer devtools in Chrome under the network page to inspect the request. It showed a 204 status, with no content; the console also showed an error message of "Syntax Error: Unexpected end of JSON input." This made me want to make sure JSON was being returned. 

    In the update route for in the toys_controller, the toy was being updated using the `update` method, but nothing was being rendered. I put in a conditional statement that either rendered the toy as json, or (if the toy was not found) returned an error message of "Toy not found". This yielded a status 200 and a proper response. 

- Donate a toy to Goodwill (and delete it from our database)

  - How I debugged: When the Donate button was initially clicked, the network page on the Chrome devtools showed a status 404 error, suggesting a client side error. The console showed an error that the DELETE request made to the specified URL was not found. 

  I inspected the existing routes in config/routes.rb, and found that the routes included for toys only included index, create, and update. Once `:destroy` was added to the array of included routes, the issue was resolved. 




