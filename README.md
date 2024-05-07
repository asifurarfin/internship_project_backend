Bank Support Ticket API
How to Run This Project
1.	Install Dependencies: Navigate into the project directory and install Composer dependencies:
cd bank-ticket
composer install
2.	Set Up Environment: Create a copy of the .env.example file and rename it to .env. Update the database connection settings in the .env file to match your local environment.
cp .env.example .env
3.	{may require sometimes} Generate Application Key: Generate a new application key for your Laravel project:
php artisan key:generate
4.	Run Migrations and Seeders: Run database migrations and seeders to create database tables and seed initial data:
php artisan migrate --seed
5.	Serve the Application: Start the Laravel development server to serve your application locally:
php artisan serve
                                                                                                                                             After database seeding it will create a demo branch called Branch 1. Later you can delete it
Now, your Laravel project should be running locally, and you can access it in your web browser.
All Endpoints
1.	Authentication Endpoints:
•	[POST] /user/register                                         checked
•	[POST] /manager/register                                  checked
•	[POST] /itdesk/register                                       checked
•	[POST] /login   (all)                   checked (use ‘No Auth’ instead of ‘Bearer token’)
•	[POST] /user/logout (all)                                     checked
2.	Branch Endpoints:
•	[POST] /branch/create (IT)  [br_id,name,add,routing]                    checked
•	[GET] /branch/list (manager, IT)                                                     checked
•	[GET] /get/branch/list (manager, IT, users)  [all]                             checked
•	[POST] /branch/update/{id}   (IT)                                                    checked
•	[POST] /branch/delete/{id}                                                             checked
3.	User Endpoints:
•	[GET] /user/list                                                    checked
4.	Category Endpoints:
•	[POST] /category/create (IT)                               checked
•	[GET] /category/list                                             checked
•	[POST] /category/update/{id}                             checked
•	[POST] /category/delete/{id}                               checked
5.	Subcategory Endpoints:
•	[POST] /subcategory/create  (IT)  [title,cat_id]      checked
•	[POST] /subcategory/update/{id}     “”                 checked
•	[POST] /subcategory/delete/{id}        “”                checked
6.	Ticket Endpoints:
•	[POST] /ticket/create (user)  [det,sol,cre,subcat_id,cat_id,sub,br_id]                        
                                                                                                                         checked                  
•	[GET] /ticket/list                                                                                          checked
•	[POST] /ticket/update/{id}                                 
•	[POST] /ticket/delete/{id}
Endpoint: user/register, manager/register, or itdesk/register
Method: POST
Description:
•	This endpoint is used to register a new user, manager, or IT desk user.
Request Body Parameters:
•	name: (Required) The name of the user. It must be a string with a maximum length of 255 characters.
•	email: (Required) The email address of the user. It must be a valid email format and unique in the users table, with a maximum length of 255 characters.
•	password: (Required) The password of the user. It must be a string with a minimum length of 3 characters.
•	branch_id: (Required) The ID of the branch the user belongs to. It must be an integer with a minimum value of 1.
•	mobile: (Optional) The mobile number of the user. It must be an integer.
Example Request:
{
    "name": "John Doe",
    "email": "johndoe@example.com",
    "password": "secretpassword",
    "branch_id": 1,
    "mobile": 1234567890
}
Example Response:
Success (200 OK):
{
    "message": "This is your auth token"
}
Error (4xx, 5xx):
{
    "error": "Error message describing the issue."
}
Endpoint: /login
Method: POST
Description:
This endpoint is used for user login.
Request Body Parameters:
•	email: (Required) The email address of the user. It must be a valid email format with a maximum length of 255 characters.
•	password: (Required) The password of the user. It must be a string with a minimum length of 3 characters.
Example Request:
{
    "email": "johndoe@example.com",
    "password": "secretpassword"
}
Example Response:
Success (200 OK):
{
    "message": "Login Successful",
    "token": "generated_token_here"
}
Error (4xx, 5xx):
{
    "error": "Invalid credentials"
}
Endpoint: /branch/create
Method: POST
Description:
This endpoint is used to create a new branch.
Request Body Parameters:
•	name: (Required) The name of the branch. It must be a string with a maximum length of 255 characters and must be unique among existing branch names.
•	address: (Optional) The address of the branch. It must be a string with a maximum length of 255 characters.
•	routing: (Required) The routing number of the branch. It must be an integer.
Example Request:
{
    "name": "Branch Name",
    "address": "Branch Address",
    "routing": 123456789
}
Example Response:
Success (200 OK):
{
    "message": "Branch Name branch created successfully"
}
Error (4xx, 5xx):
{
    "error": "Error message describing the issue"
}
Endpoint: /branch/list
Method: GET
Description:
This endpoint is used to retrieve a list of all branches along with their associated users and tickets.
Example Response:
Success (200 OK):
[
    {
        "id": 1,
        "name": "Branch 1",
        "address": "Address 1",
        "routing": 123456789,
        "created_at": "2022-03-25T12:00:00Z",
        "updated_at": "2022-03-25T12:00:00Z",
        "users": [
            {
                "id": 1,
                "name": "User 1",
                "email": "user1@example.com",
                "created_at": "2022-03-25T12:00:00Z",
                "updated_at": "2022-03-25T12:00:00Z"
            },
            {
                "id": 2,
                "name": "User 2",
                "email": "user2@example.com",
                "created_at": "2022-03-25T12:00:00Z",
                "updated_at": "2022-03-25T12:00:00Z"
            }
        ],
        "ticket": [
            {
                "id": 1,
                "subject": "Ticket 1",
                "details": "Details of Ticket 1",
                "status": "in_progress",
                "created_at": "2022-03-25T12:00:00Z",
                "updated_at": "2022-03-25T12:00:00Z"
            },
            {
                "id": 2,
                "subject": "Ticket 2",
                "details": "Details of Ticket 2",
                "status": "approved",
                "created_at": "2022-03-25T12:00:00Z",
                "updated_at": "2022-03-25T12:00:00Z"
            }
        ]
    },
    {
        "id": 2,
        "name": "Branch 2",
        "address": "Address 2",
        "routing": 987654321,
        "created_at": "2022-03-25T12:00:00Z",
        "updated_at": "2022-03-25T12:00:00Z",
        "users": [],
        "ticket": []
    }
]
Error (4xx, 5xx):
{
    "error": "Error message describing the issue"
}
Endpoint: /branch/update/{id}
Method: POST
Description:
This endpoint is used to update an existing branch with the specified ID.
Path Parameters:
•	id: (Required) The ID of the branch to be updated.
Request Body Parameters:
•	name: (Optional) The updated name of the branch. It must be a string with a maximum length of 255 characters and must be unique among existing branch names.
•	address: (Optional) The updated address of the branch. It must be a string with a maximum length of 255 characters.
•	routing: (Optional) The updated routing number of the branch. It must be an integer.
Example Request:
{
    "name": "Updated Branch Name",
    "address": "Updated Branch Address",
    "routing": 987654321
}
Example Response:
Success (200 OK):
{
    "message": "Updated Branch Name updated"
}
Error (4xx, 5xx):
{
    "error": "Error message describing the issue"
}
Endpoint: /branch/delete/{id}
Method: POST
Description:
This endpoint is used to delete an existing branch with the specified ID.
Path Parameters:
•	id: (Required) The ID of the branch to be deleted.
Example Response:
Success (200 OK):
{
    "message": "Branch Name deleted"
}
Error (4xx, 5xx):
{
    "error": "Error message describing the issue"
}
Endpoint: /user/list
Method: GET
Description:
This endpoint is used to retrieve a list of users based on their roles.
Request Body Parameters:
Example Response:
Success (200 OK):
{
    "user": [
        {
            "id": 1,
            "name": "User 1",
            "email": "user1@example.com",
            "role": "manager",
            "created_at": "2022-03-25T12:00:00Z",
            "updated_at": "2022-03-25T12:00:00Z",
            "tickets": [
                {
                    "id": 1,
                    "subject": "Ticket 1",
                    "details": "Details of Ticket 1",
                    "status": "in_progress",
                    "created_at": "2022-03-25T12:00:00Z",
                    "updated_at": "2022-03-25T12:00:00Z"
                },
                {
                    "id": 2,
                    "subject": "Ticket 2",
                    "details": "Details of Ticket 2",
                    "status": "approved",
                    "created_at": "2022-03-25T12:00:00Z",
                    "updated_at": "2022-03-25T12:00:00Z"
                }
            ],
            "branch": {
                "id": 1,
                "name": "Branch 1",
                "address": "Address 1",
                "routing": 123456789,
                "created_at": "2022-03-25T12:00:00Z",
                "updated_at": "2022-03-25T12:00:00Z"
            }
        },
        {
            "id": 2,
            "name": "User 2",
            "email": "user2@example.com",
            "role": "itdesk",
            "created_at": "2022-03-25T12:00:00Z",
            "updated_at": "2022-03-25T12:00:00Z",
            "tickets": [],
            "branch": null
        }
    ]
}
Error (4xx, 5xx):
{
    "error": "Error message describing the issue"
}
Endpoint: /category/create
Method: POST
Description:
This endpoint is used to create a new category.
Request Body Parameters:
•	title: (Required) The title of the category. It must be a string with a maximum length of 255 characters.
Example Request:
{
    "title": "New Category"
}
Example Response:
Success (200 OK):
{
    "message": "New Category Created successfully"
}
Error (4xx, 5xx):
{
    "message": "Error message describing the issue"
}
Endpoint: /category/list
Method: GET
Description:
This endpoint is used to retrieve a list of all categories along with their associated subcategories.
Example Response:
Success (200 OK): Returns a JSON array containing information about all categories and their associated subcategories.
[
    {
        "id": 1,
        "title": "Category 1",
        "created_at": "2022-03-25T12:00:00Z",
        "updated_at": "2022-03-25T12:00:00Z",
        "subcategories": [
            {
                "id": 1,
                "title": "Subcategory 1",
                "category_id": 1,
                "created_at": "2022-03-25T12:00:00Z",
                "updated_at": "2022-03-25T12:00:00Z"
            },
            {
                "id": 2,
                "title": "Subcategory 2",
                "category_id": 1,
                "created_at": "2022-03-25T12:00:00Z",
                "updated_at": "2022-03-25T12:00:00Z"
            }
        ]
    },
    {
        "id": 2,
        "title": "Category 2",
        "created_at": "2022-03-25T12:00:00Z",
        "updated_at": "2022-03-25T12:00:00Z",
        "subcategories": []
    }
]
Error (4xx, 5xx):
{
    "error": "You are not authorized to view categories."
}

For my ease:
Maker/User login Credentials: rezwan@gmail.com                 pass: 11111
Checker/manager login Credentials: arfin@gmail.com                pass: 112233
Itdesk login credentials: asif@gmail.com                 pass: 121212
