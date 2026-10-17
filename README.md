The session_authorizeAttribute class protects the RaceDay API by checking whether a user is logged in and has permission to access an endpoint.
The system has two roles: Organiser and Participant. The role_names class defines these roles, while the authorisation attribute checks the user's session and role before allowing access.
HTTP 401 – Unauthorized: Returned when the user is not logged in.
HTTP 403 – Forbidden: Returned when the user is logged in but does not have the required role.
Access granted: The request can proceed when the user is logged in and their role is permitted.
Overall, this code improves API security by restricting access to specific functions based on the user's login session and role.
