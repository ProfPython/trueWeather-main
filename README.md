# trueWeather
A weather forecast application created with MongoDB, ExpressJS, NodeJS, Vanilla Javascript, HTML, CSS and Jquery. 

Login page
![trueweather1image](https://user-images.githubusercontent.com/77641869/172741253-e2cf9b0e-198c-40b3-9388-4fb3a8c3db54.PNG)
![trueweather2image](https://user-images.githubusercontent.com/77641869/172741260-fd5d48bc-6710-4e98-95c8-d740990f9637.PNG)

User page view
![trueweather4image](https://user-images.githubusercontent.com/77641869/172741275-865dc125-1106-473b-ab3c-f02bee9a854c.PNG)

Guest page view
![trueweather5image](https://user-images.githubusercontent.com/77641869/172741299-75e7929d-07b8-4687-ac16-b18ce6ea6ba1.PNG)

TrueWeather is a weather forecast web-based app that reports real-time weather data to its 
users based on the location provided. In this project I will use the OpenWeather API 
(https://openweathermap.org/api) and other third party APIs to achieve the end goals. I chose to use this API because it is one 
of the most recognizable and elegant APIs available. It provides weather information for any 
location on the globe. It also has a basic free version that suits the needs for this project. Apart 
from using this API to display relevant data, I also plan to fetch historical weather data as a 
dataset from it. This dataset will be stored in the MongoDB database and utilized to provide a weather forecast modal box. 
The reason for this project is to build something that would be 
used daily and simple but still effective in having a wow factor; hence I came up with a weather 
web app idea to provide accurate, fast, and complete weather information to help its users plan 
activities.

Guest.html:
	This is the view of a guest user with no log in account. This view is necessary because it allows one to access weather information without an account, although some perks are lost. In this view, one can only do basic weather tasks. The following are possible:
Search a city – When the search button is clicked, it calls on the getCity function in the JS script file which sends a POST request to the server to retrieve weather information on the given city.
Temperature change – When a temperature is selected from the given options, the page is refreshed to update the current temperature unit to the selected one. The default is Celsius.
In this view, the location of the user is automatically retrieved and the weather information for the user’s location is displayed along with the current time.

User.html:
	This is the view of a logged in user. This view is necessary because it gives users with an account more benefits. In this view, apart from the functionalities one can do with the guest.html view, one can do the following:
Bookmark a city – When the bookmark button is clicked on a bookmarked item, it calls on the bookmark function in the JS script file which sends a POST request to the server to store the given city in the database. The page then refreshes to display the bookmarked city.
Delete a bookmarked city – When the delete button is clicked on a bookmarked item, it calls on the deleteBookmark function in the JS script file which sends a DELETE request to the server to remove the bookmarked city from the database. The page also refreshes to update the view.
Get forecast information over a few days for a bookmarked city – When the info button is clicked on a bookmarked item, it calls on the getForecast function in the script file which retrieves forecast info on the bookmarked city and then displays it in a modal.


Models:
I have implemented a user model which possesses the attributes of user which includes the 
username, password, and favourite cities. This model is important for our application because we 
are building a guest-user based application. Having this model enables us to create new users 
with those attributes efficiently.

Routes and Controllers:
//trueWeather resource paths 
app.get('/', weather.guest)
app.get('/user', weather.user) 
app.post('/search', weather.search) 
app.post('/bookmark', weather.bookmark) 
app.delete('/delete', weather.deleteBookmark)
app.post('/register', userController.registerUser);
app.post('/login', userController.loginUser); 

The following above are the paths involved in our application. We created custom modules for 
each of these paths to access when the appropriate HTTP request is called.
Route: / 
Controller: guest()
This controller takes the user to the guest home page where they can get weather information on 
different places or log in

Route: /user 
Controller: user()
Thus controller takes the user to a user home page, provided that they have an account, and this 
home page comes with perks like forecasts and bookmarking.

Route: /search
Controller: search()
This controller retrieves weather information on the search city to be displayed

Route: /boomark 
Controller: bookmark()
This controller saves a favorite city to the database to be displayed on the users home page

Route: /delete 
Controller: deleteBookmark()
This controller removes a city from the database

Route: /register
Controller: registerUser():
This controller creates a new user object from our User.js model, and then saves the email and 
password of the User in the ‘users’ collection of our Mongo db. Currently password is being 
stored as the exact string entered. We are aware this could be a safety concern in the future and 
are looking into encrypting the password using bcrypt or passport.

Route: /login
Controller: loginUser():
This controller reads the mongoDB ‘users’ collection and checks that if the email entered on 
client side exists in the database, and then checks if the password entered on client-side matches that in the database. If successful, it will bring the logged in user to the dashboard view, if unsuccessful, will return user an error message.

Designing the data model collection:
We designed out data model using MongoDB to store relevant information. The model 
follows a normalised pattern as only related data is store in a particular collection. We chose to 
use this pattern to avoid redundancy and to ensure only related data is together.

Tests:
We used Mocha for testing our potential usage scenarios. We tested the user registration/login 
API calls and the weather-related API calls. 
The following tests were covered for the user registration/login API calls: 
1. SUCCESS 1. POST - Registered new user successfully'
This test takes in a valid email, and valid password and posts the object to our ‘users’ collection.

2. SUCCESS 2. POST - Login user successfully
This test takes the registered email and password from the previous test and checks if it exists in 
our ‘users’ collection. 

3. FAIL 1. POST - Fail to register user with invalid email
This test tries to register with an invalid email. In this case, the email object is an integer so the 
test does not pass the valid email test and returns that user must enter a valid email. 

4. Fail 2. POST - Fail to login user with invalid email
This test looks for a specific email address in our ‘users’ collection, but since the email entered 
does not currently exist in the collection, it fails to login.

5. Fail 3. POST - Fail to login user with invalid password
This test finds an existing matching email in the ‘users’ collection, but since the password 
entered does not match that of the object of entered email, it returns that the user must enter a 
valid password.

The following tests were covered for the weather-related API calls: 
1. Success 1. GET - / (Test getting the guest home page)
This test gets the home page of a guest, and it returns the expected response.

2. Success 2. GET - /home (Test getting home page with no bookmarks)
This test gets the home page of a user with no bookmarks. There isn’t really a failure scenario for
this, so we have just a success step.

3. Fail 1. POST - /search (Test invalid city name search)
This test tries to search for an invalid city which is a common failure case with this type of app,
and it returns the expected response.

4. Fail 2. POST - /bookmark (Test invalid city bookmark)
This test tries to bookmark an invalid city, and it returns the expected response.

5. Fail 3. DELETE - /delete (Test invalid city bookmark deletion)
This test tries to delete a city that is not bookmarked in the database, and it returns the expected 
response.

6. Success 3. POST - /search (Test valid city name search)
This test searches up a city with a valid city name and it returns the expected response.

7. Success 4. POST - /bookmark (Test valid city bookmark)
This test bookmarks a valid city name and it returns the expected response.

8. Success 5. GET - /home (Test getting home page with bookmarks)
This test gets the home page of a user with bookmarks, and it returns the expected response.

9. Success 6. DELETE - /delete (Test valid city bookmark deletion)
This test deletes the bookmark of a valid city in the database, and it returns the expected
response

How to run code:

Run the following commands from VS code project terminal:

npm init -y

npm install

cd server

node app.js

Go to http://localhost:3000/

