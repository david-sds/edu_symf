# EduSymf - School Management System

## Project Overview

**EduSymf** is a comprehensive school management system for Brazilian public and private schools. Built with Symfony 8 + Twig.

**Multi-tenant:** Supports multiple schools on a single platform.

---

## Entity Structure

### Core Entities

1. **School** - id, name, inepCode, address, phone, email, logo, isActive
2. **User** - id, email, password, name, cpf, roles (array), school (FK), isActive
3. **AcademicYear** - id, year, startDate, endDate, school (FK), isCurrent
4. **GradeLevel** (Série/Ano) - id, name, code, level (enum: INFANTIL, EF1, EF2, EM, EJA), school (FK), position
5. **ClassGroup** (Turma) - id, name, gradeLevel (FK), academicYear (FK), maxStudents, period (MANHA, TARDE, INTEGRAL)
6. **Discipline** (Matéria) - id, name, code, school (FK), color (for UI)
7. **Enrollment** - id, student (User FK), classGroup (FK), academicYear (FK), status, enrollmentDate
8. **ClassDiscipline** - id, classGroup (FK), discipline (FK), teacher (User FK)
9. **Schedule** - id, classDiscipline (FK), dayOfWeek (0-6), startTime, endTime, room
10. **Assessment** - id, classDiscipline (FK), title, type (PROVA, TRABALHO, PROJETO, SIMULADO, ATIVIDADE), weight, date, bnccSkills (JSON array), description
11. **StudentGrade** - id, enrollment (FK), assessment (FK), value, observations, createdAt
12. **Attendance** - id, enrollment (FK), date, isPresent, justification
13. **Homework** (Tarefa) - id, classDiscipline (FK), title, description, dueDate, isActive, createdAt

### User Roles

- `ROLE_PLATFORM_ADMIN` - Manages all schools (no school association)
- `ROLE_DIRECTOR` - Full access within their school
- `ROLE_SECRETARY` - Registration, reports
- `ROLE_PEDAGOGUE` - Pedagogical coordination, student follow-up
- `ROLE_TEACHER` - Grades, attendance, homework
- `ROLE_STUDENT` - View grades, homework
- `ROLE_PARENT` - Portal access to child progress

### Seed Data (MEC Grades)

**Educação Infantil:**
- Berçário I, Berçário II, Berçário III
- Maternal I, Maternal II
- Pré I, Pré II

**Ensino Fundamental I (Anos Iniciais):**
- 1º Ano, 2º Ano, 3º Ano, 4º Ano, 5º Ano

**Ensino Fundamental II (Anos Finais):**
- 6º Ano, 7º Ano, 8º Ano, 9º Ano

**Ensino Médio:**
- 1ª Série, 2ª Série, 3ª Série

**EJA:**
- EJA Anos Iniciais, EJA Anos Finais, EJA Ensino Médio

### BNCC Integration

Each Assessment can be tagged with BNCC skills (stored as JSON). BNCC data structure:
- Áreas do Conhecimento
- Unidades Temáticas
- Competências Específicas
- Habilidades (código like "EF15LP01")

**Note:** BNCC data needs to be imported. Currently no API available - will need to be added as seed data later.

---

## Coding Conventions

- Use Portuguese for entity names and fields ( Brazilian context)
- Use English for code/method names
- Follow Symfony best practices
- Use Doctrine ORM
- All timestamps: createdAt, updatedAt
- Soft deletes where applicable
- Use Lifecycle Callbacks for automatic timestamps

---

## Instructions for Future AI Sessions

1. Read AGENTS.md first to understand project context
2. Check git status for uncommitted changes
3. Database: PostgreSQL via Docker on port 5433 (check .env for credentials)
4. Run migrations: `php bin/console doctrine:migrations:migrate`
5. Load fixtures: `php bin/console doctrine:fixtures:load`
6. Start server: `symfony server:start`

---

## Project Status

- [x] Install Doctrine ORM + migrations
- [x] Create User entity with timestamps (Lifecycle Callbacks)
- [x] Create User CRUD (controller, form, templates)
- [ ] Create School entity (needed for User->school relation)
- [ ] Create PlatformAdmin fixture
- [ ] Create AcademicYear, GradeLevel entities + seed data
- [ ] Create ClassGroup, Discipline entities + seed data
- [ ] Create Enrollment, ClassDiscipline
- [ ] Create Schedule
- [ ] Create Assessment, StudentGrade, Attendance, Homework

---

## What's Been Done

1. **User Entity** (`src/Entity/User.php`):
   - Fields: id, username, email, password, firstname, lastname, cpf, phone, color
   - Roles: array of strings
   - Flags: isActive, isSecure
   - Timestamps: createdAt, updatedAt, deletedAt (via #[ORM\HasLifecycleCallbacks])
   - Implements: UserInterface, PasswordAuthenticatedUserInterface

2. **User CRUD**:
   - Controller: `src/Controller/UserController.php`
   - Form: `src/Form/UserType.php` (with roles multi-select, password, color picker)
   - Templates: `templates/user/`

3. **Database**:
   - PostgreSQL via Docker on port 5433
   - Credentials: admin/secret
   - Database: edu_symf_db

4. **Docker Setup**:
   - `compose.yaml` - main compose file
   - Port 5433 exposed

---

## Current TODO

- Create School entity (needed for User->school relation)
- Set up authentication (make:security or make:auth)
- Create PlatformAdmin fixture
