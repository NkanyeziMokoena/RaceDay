⁴The session_authorizeAttribute class protects the RaceDay API by checking whether a user is logged in and has permission to access an endpoint.
The system has two roles: Organiser and Participant. The role_names class defines these roles, while the authorisation attribute checks the user's session and role before allowing access.
HTTP 401 – Unauthorized: Returned when the user is not logged in.
HTTP 403 – Forbidden: Returned when the user is logged in but does not have the required role.
Access granted: The request can proceed when the user is logged in and their role is permitted.
Overall, this code improves API security by restricting access to specific functions based on the user's login session and role.
The api_base class is a shared base class for all controllers in the RaceDay API. It inherits from ControllerBase, allowing controllers to return HTTP responses.
current_user_id: Retrieves the logged-in user's ID from the session. If no ID exists, it returns 0.
respond() method: Converts the result returned by the database class into an HTTP response using the status code provided.
db_result: Contains the database operation's result, which may include data, a status code, or a message.
registration, login, and logout in the RaceDay API. It uses the user_data class to communicate with the database and inherits shared functionality from api_base.
Register (POST /api/auth/register): Registers a new user as an Organiser or Participant using their full name, email, password, and role.
Login (POST /api/auth/login): Checks the user's login details. If successful, it stores the user's ID and role in the session and creates a session that can be used to authorise future requests.
Logout (POST /api/auth/logout): Clears the current session to log the user out.
Response handling: Uses the respond() method from api_base to return the appropriate HTTP response.
The manage_categories controller manages event categories in the RaceDay API. It uses the category_data class to perform database operations and inherits shared functionality from api_base.
View categories (GET): Allows anyone to view the categories belonging to an event.
Create category (POST): Allows an Organiser to add a category by providing its name, distance in kilometres, and maximum number of participants.
Update category (PUT): Allows an Organiser to update an existing category's details.
Delete category (DELETE): Allows an Organiser to delete a category, including its enrolments and results, as handled by the database class.
The manage_enrolments controller manages participant registrations for race categories in the RaceDay API. It uses the enrolment_data class to handle database operations and inherits shared functionality from api_base.
Enrol in a category (POST): Allows a logged-in Participant to enter a race category. A bib number is assigned automatically.
View my enrolments (GET): Allows Participants to view their own enrolments.
View category enrolments (GET): Allows an Organiser to view all enrolments for a category belonging to an event they own.
Cancel enrolment (DELETE): Allows an enrolment to be cancelled by the enrolled Participant or the Organiser who owns the event, subject to the checks in the database class.
The manage_events controller manages race events in the RaceDay API. It uses the event_data class to handle database operations and inherits common functionality from api_base.
View all events (GET): Allows anyone to view all events, ordered by date.
View an event (GET): Allows anyone to retrieve a specific event using its ID.
Create an event (POST): Allows a logged-in Organiser to create an event by providing its name, date, location, event type, and other details.
Update an event (PUT): Allows an Organiser to update an event they own.
Delete an event (DELETE): Allows an Organiser to delete an event, including its associated categories, enrolments, and results, as handled by the database class.
The manage_results controller manages race results in the RaceDay API. It uses the result_data class to perform database operations and inherits shared functionality from api_base.
Record a result (POST): Allows an Organiser to record a participant's finishing time and position for an event they own.
View category results (GET): Allows anyone to view a category's leaderboard, ordered by finishing position.
View a result (GET): Allows anyone to retrieve a specific result using its ID.
Update a result (PUT): Allows an Organiser to update a result for an event they own.
View my results (GET): Allows logged-in Participants to view their own race results
The user_profile controller manages the profile of the currently logged-in user in the RaceDay API. It uses the user_data class to perform database operations and inherits common functionality from api_base.
View profile (GET /api/users/me): Retrieves the logged-in user's profile information.
Update profile (PUT /api/users/me): Allows the logged-in user to update their full name and email address.
Session authorisation: The [session_authorize] attribute protects the entire controller, ensuring that only logged-in users can access these endpoints.
User identification: The current_user_id property identifies the logged-in user so they can only access and update their own profile.
Response handling: The respond() method returns the appropriate HTTP status code and response data.
The Category model represents a race category in the RaceDay API. It stores the category's details and defines its relationships with events and participant enrolments.
CategoryId: Uniquely identifies each category.
EventId: Identifies the event to which the category belongs.
Name: Stores the category name.
DistanceKm: Specifies the race distance in kilometres.
MaxParticipants: Sets the maximum number of participants, defaulting to 100.
Event: Represents the event associated with the category.
Enrolments: Stores the list of participant enrolments for the category.
The Enrolment model represents a participant's registration for a race category in the RaceDay API. It stores enrolment details and connects participants to categories and race results.
EnrolmentId: Uniquely identifies each enrolment.
ParticipantId: Identifies the participant who registered.
CategoryId: Identifies the race category selected.
EnrolmentDate: Records the registration date and time, defaulting to the current date and time.
BibNumber: Stores the participant's race number.
Status: Tracks the enrolment status, defaulting to "Registered".
Participant: Links the enrolment to the registered user.
Category: Links the enrolment to the selected race category.
Result: Stores the participant's race result, if one exists. It is optional.
The Event model represents a race event in the RaceDay API. It stores event information and defines the relationship between an event, its Organiser, and its categories.
EventId: Uniquely identifies each event.
OrganiserId: Identifies the Organiser responsible for the event.
Name: Stores the event name.
Description: Provides optional information about the event.
EventDate: Records the date and time of the event.
Location: Specifies where the event takes place.
EventType: Defines the event type, which can be Run, Walk, or Cycle. The default is "Run".
DistanceKm: Stores the optional overall event distance in kilometres.
Organiser: Links the event to the user who organises it.
Categories: Stores the list of categories belonging to the event.
The code defines five Data Transfer Objects (DTOs) used in the RaceDay API to control how information is returned to the client. Each DTO selects the necessary information from a database model without exposing sensitive details, such as a user's password hash.
UserDto: Returns user information, including the user ID, full name, email, role, and account creation date.
EventDto: Returns event details, including the name, description, date, location, type, and distance.
CategoryDto: Returns race category information, including its name, event ID, distance, and maximum number of participants.
EnrolmentDto: Returns enrolment details, including the participant's name, category, event, registration date, bib number, and status.
ResultDto: Returns race result information, including the participant's name, bib number, finishing time, position, and result status.
The Result model stores a participant's race results in the RaceDay API. It links each result to a specific enrolment and records the participant's finishing time, position, and status.
ResultId: Uniquely identifies each result.
EnrolmentId: Identifies the participant's race enrolment.
FinishTime: Stores the finishing time. It is optional.
Position: Records the participant's finishing position. It is optional.
Status: Tracks the result status, defaulting to "Pending".
Enrolment: Links the result to the relevant enrolment.
The User model represents a registered user in the RaceDay API. It stores the user's account information and links the user to their assigned role.
UserId: Uniquely identifies each user.
RoleId: Identifies the user's role, such as Organiser or Participant.
FullName: Stores the user's full name.
Email: Stores the user's email address.
PasswordHash: Stores the hashed password instead of the original password.
CreatedAt: Records when the account was created, defaulting to the current date and time.
Role: Links the user to their assigned role.
The Role model defines the different types of users in the RaceDay API and manages the relationship between roles and user accounts.
RoleId: Uniquely identifies each role.
RoleName: Stores the role name, such as Organiser or Participant.
Users: Contains a list of users assigned to that role.
The launchSettings.json file configures how the RaceDay API runs during development in Visual Studio or through the .NET CLI.
$schema: Specifies the schema used to validate the launch settings file.
iisSettings: Configures IIS Express, including authentication settings and application ports.
http profile: Runs the API using HTTP at http://localhost:5080.
https profile: Runs the API using HTTPS at https://localhost:7080 and also supports HTTP on port 5080.
IIS Express profile: Allows the application to run using IIS Express.
launchBrowser: Automatically opens a browser when the application starts.
launchUrl: Opens the Swagger page for testing API endpoints.
ASPNETCORE_ENVIRONMENT: Sets the environment to Development, enabling development-specific configuration.
The AppDbContext class connects the RaceDay API to its database using Entity Framework Core and follows the Code First approach. It defines the database tables, relationships, constraints, and default values.
Database tables: The DbSet properties define tables for Roles, Users, Events, Categories, Enrolments, and Results.
Role configuration: Defines Organiser and Participant roles and ensures that role names are unique.
User configuration: Sets required fields, limits field lengths, ensures email addresses are unique, and links each user to a role.
Event configuration: Defines event details, data types, and the relationship between events and their Organisers.
Category configuration: Stores category details, sets the default participant limit to 100, and links categories to events.
Enrolment configuration: Sets default enrolment dates and ensures that a participant cannot enrol in the same category twice and that bib numbers are unique within a category.
Result configuration: Stores finishing times and statuses and establishes a one-to-one relationship between an enrolment and its result.
Relationships and constraints: Uses primary keys, foreign keys, unique indexes, and delete behaviours to maintain data integrity.
The category_data class handles database operations for race categories in the RaceDay API. It uses AppDbContext to retrieve, create, update, and delete categories.
get_categories_for_event(): Retrieves categories for a specific event, ordered by distance. Returns a 404 error if the event does not exist.
create_category(): Adds a new category to an event after checking that the event exists, the Organiser owns it, and the category details are valid.
update_category(): Updates an existing category's name, distance, and maximum participant limit after checking ownership and validating the information.
delete_category(): Deletes a category after verifying that it exists and belongs to the Organiser making the request.
Validation: Uses the validator.check_category() method to check category details before saving changes.
Database operations: Uses Entity Framework Core methods such as Find(), Add(), Remove(), and SaveChanges() to manage records.
Response handling: Returns a db_result containing the appropriate HTTP status code, data, or error message.
The db_result class standardises the responses returned by database methods in the RaceDay API. It provides a status code, a message, and optional data that the controller uses to create an HTTP response.
status: Stores the HTTP status code, defaulting to 200 (OK).
message: Stores a success or error message.
data: Holds the returned information, such as a user, event, category, or result. It can be null.
ok(): Creates a successful response with status code 200.
created(): Creates a response with status code 201 when a resource has been created.
fail(): Creates an error response using the specified status code and message.
The enrolment_data class manages participant enrolments in the RaceDay API. It uses AppDbContext and Entity Framework Core to register participants, retrieve enrolment information, and cancel registrations.
enrol_participant(): Registers a participant in a race category, automatically assigns a bib number (such as B001 or B002), and checks that the participant has not already enrolled and that the category is not full.
get_my_enrolments(): Retrieves all enrolments belonging to the logged-in Participant, including the related category and event details.
get_category_enrolments(): Retrieves all enrolments for a category after verifying that the Organiser owns the associated event.
cancel_enrolment(): Cancels an enrolment if the request is made by the enrolled Participant or the Organiser who owns the event.
Error handling: Uses try-catch to handle database update errors during enrolment and returns a 409 Conflict response if the registration cannot be completed.
