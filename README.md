# Building a Flask App to Discover Places Near You

![N|Solid](https://raw.githubusercontent.com/erikamart/learning-flask/master/static/img/learningFlask_sm.jpg)

This project demonstrates a web application using the Flask web framework.  The resulting app will allow a user to sign up, login/logout, and find locations of interest near a specified address.  Project code is from a Lynda.com Flask tutorial created by Packt Publishing and led by Lalith Polepeddi.

### Tech
-------------------------------------------
What was used to make this project work:
##### Software & Web
* [WSL] - Windows Subsystem for Linux lets developers run Linux environments.
* [XAMPP] - Apache distribution containing MariaDB, PHP, and Perl.
* [Sublime 3] - Text editor for code, markup and pros.
* [Heroku] - Deploy and run apps on a cloud platform.
* [Anaconda] - Anaconda Distribution to use python 3.6.
* [Git] - Version Control of files to use with Github.
* [GitHub] - Version control file manager.
* [pip] - The PyPA recommended tool for installing python packages.

##### Libraries & Extensions
* [virtualenv] - A tool to create isolated Python environments.
* [Flask] - A microframework for Python.
* [FLask-SQLAlchemy] - Extension for Flask that adds support for SQLAlchemy
* [pymysql] - Database connector for Python programs to talk to MySQL server.
* [Flask-WTF] - Integration of Flask and WTForms.
* [geocoder] - A simple geocoding library written in Python.
* [gunicorn] - Python HTTP server for WSGI applications. 

### Lessons learned
-------------------------------------------
1. Development in a WSL environment is not as easy as one would think. There was a need to constantly address file permissions issues.  It's important to remember that you can grant permissions within a Linux environment, but those permissions will still always be independent of Windows permissions.  Not just that, but strangely even if a file or folder had been set with proper RWX permissions in Linux environment, every so often the permissions would get cleared out and need to be reset. This was the result of editing files in Sublime which was a Windows based program while simultaneously working with them through the WSL Debian CLI. 
2. The Heroku CLI package does not work with WSL as of yet.  The reason is, WSL cannot use `systemctl` to control server programs. When attempted, an error of "Failed to connect to bus: No such file or directory" is produced. You can start dbus, but in the end there is no `systemd` and no `systemctl`. There are workarounds, but it will still be problematic and, in the end, not worth it.  If I had known before hand, I'd have used a full-fledged UNIX or LINUX system.  Windows could do it all too, but command line commands could get a little too interesting for my taste. If you're a master of CLI and creating aliases and paths to other extensions; then have at it!
3. Sometimes the long list of errors is caused by one simple lack of saving.  I had an issue where my site worked fine locally, but it kept failing and crashing on Heroku.  The issue was simply because the requirement.txt file Heroku was using was the very first copy made at the beginning of the project creation and not the final copy that listed ALL the necessary libraries that had been added through the end of the code writing.  Saving & updating files is your friend.  Don't forget your friend.
4. It is best to start off using a local relational database management system (RDBMS) to get things working and then convert to a non-locally hosted server. Local testing is great when you're first learning and don't want to hassle with heaps of hurdles to overcome.  When the time comes to deploy it'll be fewer changes to make it happen.  NOTE: Refer to branch `new_feature` for instruction Steps 15 and on to set up a non-locally hosted RDBMS. Do this as a final step if you want a fully functioning app on Heroku.


### Installation
-------------------------------------------
1. Create your own accounts for Github and Heroku

2. Setup your preference of text editor and relational database system. The database connection to the app will come later but keep this in mind for now: This project code connects to a locally hosted database with the user of root and default password credentials for MySQL.

Start the server for your database engine and login. My particular database is MariaDB so as soon as I logged in that's what appears in the command line.
```sh
$ sudo/etc/init.d/mysql start
$ sudo mysql -u root
(default password is blank so just hit enter)

MariaDB [(none)]>
```
Create a new database called "learningflask" with a table in it called "users". The "users" table will have five columns titled for user id, first name, last name, email, and password.  Note: the password will be stored as encrypted data via code within the [models.py](https://github.com/erikamart/learning-flask/blob/master/models.py) file.

MariaDB [(none)]> `create database learningflask;`
MariaDB [(none)]> `use learningflask;`
MariaDB [(learningflask)]> `CREATE TABLE users (uid int not null AUTO_INCREMENT PRIMARY KEY, firstname VARCHAR(100) not null, lastname VARCHAR(100) not null, email VARCHAR(120) not null unique, pwdhash VARCHAR(100) not null);`

3. Using your command line interface (CLI), install python 3.6 (or the Anaconda distribution) and [Git].
4. Begin setting up Flask using a virtual environment called [virtualenv] so that Flask and other libraries are in an isolated environment that will not affect the rest of the computer. (see note below if you installed Anaconda)
Pip should already be installed when python gets installed, but if you get an error trying to install virtualenv then follow the directions [here](https://pip.pypa.io/en/stable/installing/) to install pip. 
NOTE: If you installed Anaconda, you can use the [conda](http://www.puzzlr.org/install-packages-pip-conda-environment/) package manager to use the conda virtual environment and then install a pip version specifically into the environment per the directions on the website link.
5. Clone or Download the [learning-flask](https://github.com/erikamart/learning-flask.git) repository files into your local computer system.

6. Using your CLI, change into the main project folder to activate virtualenv.

```sh
$ cd learning-flask
$ source venv/bin/activate
```
You should see (venv) appear above or beside your usual command line entry line. ALWAYS make sure you have venv activated throughout the package install process!

Note: If you need to close the virtual environment just type 
```sh
$ deactivate
```

7. Install the rest of the remaining libraries from the Tech section above. NOTE: pymysql is specifically for MySQL database.

```sh
$ pip install Flask
$ pip install pymysql
$ pip install geocoder
$ pip install gunicorn
```
8. If the database management system you chose differs from mysql/mariaDB, then open routes.py and alter the lines containing `pymysql` found in line 4 and 8.  You may need to use a different database connector for python to talk to your database. Be sure to pip install the connector you use.

9. If you had to make the changes in Step 8 then you must update the "requirements.txt" file to maintain the list of python libraries so that deployment to Heroku will not fail.
```sh
$ pip freeze > requirements.txt
```
10. Test the app locally with
```sh
$ FLASK_APP=routes.py FLASK_ENV=development flask run
```
A properly working web app should allow a user to sign up and enter any address or zip code to view places of interest.  It will also informally allow the user to login or logout by adding the words respectively at the end of the link path in the browser bar. 

Examples: 
* https://localhost:5000/logout
* https://localhost:5000/login

The connected database should also reflect new rows with user credentials that have signed up.

11. Initalize your git folder, add files to staging, commit, and finally push files to your Github.
```sh
$ cd learning-flask
$ git init
Add files to staging and commit at the same time
$ git commit -am "Description of commit"
```
Login to GitHub.com and create a new repository. Type in the name, "learning-flask", for your repository and create it. Push your existing local repository to the newly created github repository. Remember the `USERNAME` area in the below commands should be replaced with your specific username on github.
```sh
$ git remote add origin https://github.com/USERNAME/learning-flask.git
$ git push -u origin master
Enter username and password when prompted
```
12. Deploy to Heroku
```sh
$ heroku login
Enter email and password when prompted
```
Then create a Heroku app.
```sh
$ heroku create
```
Then push the flask app to the heroku remote repository.
```sh
$ git push heroku master
```
Open the heroku app to see it works.
```sh
$ heroku open
```

Optionally you can push your app repository to github only and connect it to Heroku through the Heroku dashboard. In the dashboard you can also set it up so that any later github commits can be detected and deployed automatically.

At this point, your RDBMS is still locally hosted by your computer and will therefore not work on the heroku app. To fix this, you need to switch to a non-locally hosted RDBMS. See the branch of this repo called "new_feature" to do this.

### Todos
-------------------------------------------
 - Make a login/logout button
 - Switch to a non-locally hosted RDBMS


   [Sublime 3]: <https://www.sublimetext.com/>
   [XAMPP]: <https://www.apachefriends.org/index.html>
   [WSL]: <https://docs.microsoft.com/en-us/windows/wsl/about>
   [Heroku]: <https://devcenter.heroku.com/articles/getting-started-with-python#introduction>
   [Anaconda]: <https://www.anaconda.com/download/#linux>
   [Git]: <https://git-scm.com/downloads>
   [Github]: <https://github.com/>
   [pip]: <https://pip.pypa.io/en/stable/>
   
   [virtualenv]: <https://virtualenv.pypa.io/en/stable/>
   [Flask]: <http://flask.pocoo.org/>
   [Flask-SQLAlchemy]: <http://flask-sqlalchemy.pocoo.org/2.3/>
   [pymysql]: <https://pymysql.readthedocs.io/en/latest/>
   [Flask-WTF]: <https://flask-wtf.readthedocs.io/en/stable/>
   [geocoder]: <https://geocoder.readthedocs.io/api.html#installation>
   [gunicorn]: <https://devcenter.heroku.com/articles/python-gunicorn>
   [Learning Flask]: <https://guarded-thicket-18374.herokuapp.com/>