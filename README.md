# Online Job Portal

**Java Programming Project** | Galgotias University | Batch 2029 | Project type: Web-based (Rubric B)

An online job portal where employers post jobs, job seekers search and apply with a resume, and an administrator approves job postings and manages users. Each role has its own dashboard.

## Team

**Team name:** `TriCode`

| Name | Roll No | GitHub | Role | Modules |
|---|---|---|---|---|
| `Ayushi Goyal` | `25SCSE1410298` | `Ayushi09-pixel` | Team Leader | Database design, DAO layer, authentication, integration |
| `Suhani Bharti` | `25SCSE1181111` | `<github-username>` | Member | Employer and Admin modules |
| `Aryan Mehta` | `25SCSE1410318` | `<github-username>` | Member | Job Seeker module, shared UI |

**Faculty mentor:** `Jyoti Ratna`

## Features

**Admin**
- Dashboard with user counts by role and job counts by status
- Approve or reject job postings submitted by employers
- View, edit and delete user accounts and roles

**Employer**
- Dashboard with job and application statistics
- Post, edit and delete jobs (edited jobs go back for admin approval)
- Review applicants, download resumes, and update application status (Applied, Shortlisted, Rejected, Hired)

**Job Seeker**
- Search approved jobs by keyword and location
- Apply with a resume (PDF, DOC or DOCX, max 2 MB) and a cover letter
- Track application status and history
- Manage profile (name, phone, skills, resume)
- Job recommendations based on the skills in the profile

**Security**
- Passwords hashed with BCrypt
- All database access through `PreparedStatement`
- Role-based access control through a servlet filter
- Resumes stored outside the web folder under random file names

## Tech stack

| Layer | Technology |
|---|---|
| Language | Java 17 |
| Web | Servlets and JSP (Jakarta EE), JSTL |
| Database | MySQL 8 with JDBC |
| Build | Maven (WAR) |
| Server | Apache Tomcat 10 or newer |

## Project structure

```
src/main/java/com/jobportal/
    model/       User, Job, Application, Profile and enums (Role, JobStatus, ApplicationStatus)
    dao/         UserDAO, JobDAO, ApplicationDAO, ProfileDAO
    servlet/     Login, Register, Logout, Resume download, BaseServlet
        employer/  Employer servlets
        admin/     Admin servlets
        seeker/    Job seeker servlets
    filter/      AuthFilter (role-based access)
    util/        DBConnection, PasswordUtil, DAOException, FileStorage, JobRecommender, SeedAdmin
src/main/webapp/
    WEB-INF/views/   JSP pages for each role (reachable only through servlets)
    css/             Stylesheets
sql/schema.sql       Database schema
```

## Database

Six tables: `users`, `jobs`, `applications`, `profiles`, `messages`, `settings`. The full schema is in `sql/schema.sql`.

## How to run

**Requirements:** JDK 17, Maven, MySQL 8, Apache Tomcat 10 or newer.

1. **Create the database**
   ```bash
   mysql -u root -p < sql/schema.sql
   ```
2. **Configure the database connection.** Copy `src/main/resources/db.properties.example` to `src/main/resources/db.properties` and enter your MySQL password. This file is ignored by Git and must never be committed.
3. **Build**
   ```bash
   mvn clean package
   ```
4. **Deploy** `target/jobportal.war` to Tomcat 10+ (or run it from your IDE with a Tomcat 10 server).
5. **Create the first admin** by running the `com.jobportal.util.SeedAdmin` class once. It creates `admin@jobportal.com`. Change its password after the first login.
6. **Open** `http://localhost:8080/jobportal/`

Resumes are saved to `~/jobportal-uploads`. Set the environment variable `JOBPORTAL_UPLOAD_DIR` to use a different folder.

## Demo flow

1. Register an **Employer** and post a job (status: PENDING)
2. Log in as **Admin** and approve the job
3. Register a **Job Seeker**, add skills and a resume in the profile
4. Search for the job and apply
5. As the Employer, open Applications and change the status
6. As the Job Seeker, open My Applications and see the update

## Screenshots

Screenshots are in the `docs/screenshots` folder.

| Login | Admin dashboard | Employer dashboard |
|---|---|---|
| _add image_ | _add image_ | _add image_ |

| Job search | Apply | Application status |
|---|---|---|
| _add image_ | _add image_ | _add image_ |

## Project reviews

| Review | Marks | Deadline | Status |
|---|---|---|---|
| Review 0 - Title and description | None | - | Approved |
| Review 1 | 33 | 10 October 2026 | In progress |
| Review 2 | 17 | 15 November 2026 | Planned |

Planned for Review 2: candidate messaging, admin statistics charts, system settings panel, and unit tests.

## Branches

- `main`: stable, reviewed code only
- `feature/*`: one branch per member, merged through pull requests
