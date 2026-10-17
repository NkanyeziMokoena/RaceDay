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
