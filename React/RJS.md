- Feature-driven folder structure inspired by [Bulletproof React](https://github.com/alan2207/bulletproof-react)
- ESLint for static code checks
- Prettier for consistent code formatting
- TypeScript for type checking
- Husky & lint-staged for pre-commit hooks
- Cypress for integration testing
- Jest for unit testing
- Storybook for component documentation
- react-query for data fetching
- SCSS modules for styling
- GitHub Actions for Continuous Integration

## Features folder
- issues
- layout
- projects
	- API
	- components
	- index.ts `Barrel File`
- ui (reusable components)

- Barrel Files - define what a feature should export to the rest of the application
	- can be enforced by `EsLint`


## Typical Development Team
- one product manager
	- plans the features of the app
- one designer
	- prepare the look and feel of the UI (user interface)
- multiple developers
	- duties involve writing, testing, and reviewing code
	- larger teams may have QA (quality assurance) or test engineers

## My Team
- Jack - Product Manager
- Tara - Senior Developer
	- Tara approves your code changes (Git workflow)
	- Tara can be accessed through discord messaging
- Deniz - UI/UX Designer

# Designs

[ProLOG Figma](https://www.figma.com/file/8ZmbMRC8PtvXx7L5PjVcVM/Prolog?type=design&node-id=156-21829&mode=design&t=hwsFPJAtUzfhrrLQ-0)

# Project Management
- Feature Life cycle
	1. idea for a feature (business requirements, user research, feature request, thin air)
	2. product manage (with team members) thinks through feature and creates specifications
		- What is the feature supposed to do?
		- How will it work?
	3. designer creates the designs from these specifications (iterative process)
	4. feature is broken down into smaller tasks
		- product manager creates a set of tasks from the **user's** perspective
		- tasks are then refined and extended by the developers
	5. feature is implemented by the developer team
	6. feature is then tested by product manager, a dedicated QA person, or one of the developers
		- buts may need to be fixed
	7. features is shipped and tested on real users
	8. depending on the results in production, the feature might run through the same process again for iterative improvement


[Kanban Board](https://profy.dev/project/react-job-simulator/board)

Kanban Fun Facts:
- originally developed by an engineer at Toyota and adopted into software development
- it means billboard in Japanese

> A Kanban team often has one important rule: There should be **X** tasks in progress.

The reason is the developers tend to neglect code reviews and rather work on their own tasks. After some time it's common to see a big pile of tasks in the "In Progress" and "In Review" columns. This can lead to horrible merge conflicts.

## Comparison of Waterfall. Agile Development, Scrum, and Kanban

**Waterfall** methodology
- is the opposite of Agile development (Kanban)

- first, you define all the details of a project upfront
- next, you create the design
- finally, you start the actual implementation
- at some point, you ship the application

> This approach is okay for small projects with limited scope, but thinking through all the details of a large project doesn't work

- requirements change, stakeholders/clients change their minds, and users have feedback that needs to be incorporated into the development process

**Kanban** is an Agile methodology
- a practice that focuses on continuous iterative development
	- build and ship fast
	- get feedback from users/client/stakeholders
- is very lightweight, can be used in small and large teams

**Scrum** is another Agile methodology
- in short, is a relatively heavy process (comes with much more overhead)
- team requires different roles (product owner, scrum master, development team)
- lots of weekly or biweekly meetings

[Read More: Kanban vs. scrum: which agile are you?](https://www.atlassian.com/agile/kanban/kanban-vs-scrum)

`/* TODO */`
- ask about pinning the repository
- read last few slides