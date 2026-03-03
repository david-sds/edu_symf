# EduSymf - School Management System

School management system for Brazilian public and private schools.

## Requirements

- PHP 8.4+
- PostgreSQL
- Composer
- Symfony CLI

## Installation

1. Clone the project
2. Configure database in `.env`
3. Install dependencies: `composer install`
4. Create database: `php bin/console doctrine:database:create`
5. Run migrations: `php bin/console doctrine:migrations:migrate`
6. Load fixtures: `php bin/console doctrine:fixtures:load`
7. Start server: `symfony server:start`

## Tech Stack

- Symfony 8
- Twig
- Doctrine ORM
- PostgreSQL

## System Structure

### Users and Roles

- **Platform Admin** - Manages all schools
- **Director** - Full access within their school
- **Secretary** - Enrollment, reports
- **Pedagogue** - Pedagogical coordination
- **Teacher** - Grades, attendance, homework
- **Student** - View grades and homework
- **Parent/Guardian** - Child progress portal

### Modules

- School Management (multi-tenant)
- Academic Years and Classes
- Enrollments
- Disciplines and Schedules
- Assessments and Grades
- Attendance
- Homework
- Report Cards (Boletim)

## License

Proprietary
