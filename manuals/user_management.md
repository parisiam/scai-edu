<h1>USER MANAGEMENT</h1>

[TOC]

In this short document we are going to see how to list the registered users, set a new administrator and what are the different roles for a user.

# List of users

Go to `Administration > Users > Accounts > Browse list of users` .

All the users are displayed here whatever their role is (admins, teachers, students).

- By clicking the trash icon, you delete a user and all its informations (courses registrations, grades...).
- By clicking the cog icon, you can modify its profile information, including password and email.

# Add a site administrator

Go to `Administration > Users > Permissions > Site administrators`.

From there you can add (or remove) an existing user to the list of site administrators. Site administrators have the power of life and death over the site.

# List of roles

A user may have several roles. Only one applies at a given moment but Moodle offers the possibility to switch roles without having to logout.

The default roles on Moodle are (besides the site administrator role):

| Role                            | Description                                                  | Short name     |
| ------------------------------- | ------------------------------------------------------------ | -------------- |
| Manager                         | Managers can access courses and modify them, but usually do not participate in them. | manager        |
| Course creator                  | Course creators can create new courses.                      | coursecreator  |
| Teacher                         | Teachers can do anything within a course, including changing the activities and grading students. | editingteacher |
| Non-editing teacher             | Non-editing teachers can teach in courses and grade students, but may not alter activities. | teacher        |
| Student                         | Students generally have fewer privileges within a course.    | student        |
| Guest                           | Guests have minimal privileges and usually can not enter text anywhere. | guest          |
| Authenticated user              | All logged in users.                                         | user           |
| Authenticated user on frontpage | All logged-in users in the frontpage course.                 | frontpage      |

Go to `Administration > Users > Permissions > Define roles` to view and modify the roles.

# Assign/remove a system role to a user

The system roles are the roles that apply to users throughout the entire system, including the front page and all courses:

- Manager
- Course creator

Go to `Administration > Users > Permissions > Assign system roles` to view and modify the roles.

# Non-system roles

Non system roles can be assigned by site administrators and managers at the course level.
