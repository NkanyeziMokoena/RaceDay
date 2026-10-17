The session_authorizeAttribute class protects the RaceDay API by checking whether a user is logged in and has permission to access an endpoint.
The system has two roles: Organiser and Participant. The role_names class defines these roles, while the authorisation attribute checks the user's session and role before allowing access.
HTTP 401 – Unauthorized: Returned when the user is not logged in.
HTTP 403 – Forbidden: Returned when the user is logged in but does not have the required role.
Access granted: The request can proceed when the user is logged in and their role is permitted.
Overall, this code improves API security by restricting access to specific functions based on the user's login session and role.
The api_base class is a shared base class for all controllers in the RaceDay API. It inherits from ControllerBase, allowing controllers to return HTTP responses.
current_user_id: Retrieves the logged-in user's ID from the session. If no ID exists, it returns 0.
respond() method: Converts the result returned by the database class into an HTTP response using the status code provided.
db_result: Contains the database operation's result, which may include data, a status code, or a message.
