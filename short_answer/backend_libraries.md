# Back End Libraries and Frameworks

## What is MVC?

MVC stands for model-view-controller. It's a software architectual design pattern. The general purpose of goal is to separate functionality, logic and the interface in an application. This promotes organized programming and it allows multiple developers to work on the same project with these.  

Popular framworks that use MVC concepts: Ruby on Rails, Express, Angular, Laravel...

#### Model  
Model is responsible for getting and manipulating the data.  
Usually it interacts with database (SELECT, INCERT, UPDATE, DELETE). The model also communicates with controller and sometimes it can update the view (depends on framework).  

#### View  
View is the user interface of the application. It usually consists of HTML and CSS along with dynamic values from the controller with template engines.  

#### Controller  
Controller acts as a middleman between the model and the view. It receives input and processes requests. It gets data from the model and passes data to the view.  

## What is ORM?

Object-relational mapping is a technique for converting data between incompatible type systems using object-oriented programming languages.  

Through ORM, we don't have to rewrite our code because of the changing of database products. It allows us to query and manipulate data from a database using objects instead of SQL queries.
