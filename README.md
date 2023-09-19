# Project Title

## Simple DevOps Task: CI/CD Workflow to deploy an application using GitHub Actions.

DevOps as we know is a set of practices that combine cultural philosophies, techniques and tools of software development and IT operations with the aim to shorten the Software development life cycle (SDLC) and provide Continuous integration, delivery and deployment with high software quality.

This project will see us use and demonstrate how a CI/CD tool called GitHub Actions can be used for our CI/CD tasks. GitHub Actions is a tool that allows users to automate software development workflow by writing individual tasks (called workflows) and combining them to create a custom workflow. Workflows are custom automated processes that can be set up in our repository to build, test, package, release and deploy our code/projects which are hosted on GitHub.

See this section about [GitHub Actions](https://docs.github.com/en/actions) and [Ci/CD](https://about.gitlab.com/topics/ci-cd/) to read and access more information.

## Project Description

This project will see us use the CI/CD process and try as much as possible to replicate a real-time scenario and best practices in this project. Here is a brief explanation of what his project entails.
We shall create a workflow in our repository to log commits and push local code changes. We’ll have three branches (Master/Main, Develop and Workflow) that will play three different roles in achieving our CI/CD workflow. The first two branches are protected branches (Master/Main and Develop) because we do not want direct commits and push to these branches.
The First branch, the “Workflow” branch, is where all our code changes and modifications will be done.
The Master/Main branch: Contains deployable codes, and codes in production – Anything pushed/Merged to master is ready to be deployed into production and go live.
Develop branch: This will contain development codes, bug fixes, features and codes that are not ready for production, every code pushed/merged to this branch is deployed into staging for testing purposes, once all tests are passed, codes are then pushed/merged to Master via a Pull Request
Workflow branch: This is our working branch where all code changes and modifications will be made.

### Scenario:

The Frontend Team needs to add a new feature to the Application (e.g., Add an option to toggle between Dark Mode and Light Mode) – As we know this change can break our application if not properly done. Below is a sample workflow to achieve this.

- Create a new Feature branch from the Master branch – Feature/” Branch Name”

- When done with code and local testing, open a pull request to merge into the develop

- Pull request will initiate a workflow that will test the code against some test criteria set out in the workflow, if features pass and don’t break the code, it’ll be reviewed by the DevOps team handling the Develop branch and decide if it's to be merged or not. If the pull request is approved and now merged into the Develop branch, the merge request will trigger another job from our workflow to test for new dependencies and then deploy the new changes into staging where the new changes can be tested and reviewed again in a near-live environment.

- If our pull request fails, our workflow will trigger another job which will automatically open an issue with the commit hash that failed for the team to resolve the issue and test the pull request.

- When a new feature fails deployment into staging, our workflow will trigger a job that will send a notification to the DevOps team Slack group on the failure of the deployment of the new feature and a link to the Issue open on GitHub.

- When the new Feature is ready for production, a new pull request to merge into the master is opened which triggers a job in our build workflow to test for code-breaking changes and dependencies in our code. If it succeeds, The DevOps team handling the Master branch will review the pull request and decide whether to merge the changes into the Master branch and deploy the code changes from production to production. Once the DevOps team approve the changes to be merged into Master, another job in our workflow is triggered to test for new dependencies and deploy our code into production.

- When a new feature is successfully deployed into production, our workflow will trigger a job that will send a notification to the DevOps team Slack group on the successful deployment of the new feature.

As explained above, this project is set to achieve.

# Getting started with the Project and its dependencies

## Create a Simple Application

We need to create a simple application for this workflow using [Create Reat App](https://create-react-app.dev/)

### 'Getting Started with Create React App'

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

### Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

The page will reload when you make changes.\
You may also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can't go back!**

If you aren't satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single-build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc.) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point, you're on your own.

You don't have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn't feel obligated to use this feature. However, we understand that this tool wouldn't be useful if you couldn't customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

## Host the application

For this project, we will host our application using [Surge](https://surge.sh/)

- install surge globally - npm install --global surge
- Run Surge and create an account with surge
- Create two (2) URLs for production and staging purposes to be used in our workflow
- Refer to [Surge's Documentation](https://surge.sh/) for more information about Surge.

## Setup code dependencies to be tested for during our workflow

1. Install code formatter - [Prettier](https://prettier.io/)

- Install for our project only
-     npm install --save-dev --save-exact prettier
- Create an empty config file in our project with the name. prettierrc.json
-     touch .prettierrc.json or echo {}> .prettierrc.json
- Create custom code format, refer to [Prettier](https://prettier.io/) for more information
- Modify our projects package.json file to add this new code format under scripts
- Create format check function “Name = Format:Check” and include the languages to be formatted into our new format
-     "format: check": "npx prettier --check \"**/*.{js,jsx,yml,yaml,json,css,scss,md}\"",
- Create a format function “Name = Format” to automatically format our codes to the prettier function defined
-     "format": "npx prettier --write \"**/*. {js,jsx,yml,yaml,json,css,scss,md}\""

### 'Define Workflow'

1. Workflow Branch – Pull Request to Merge into Develop

- Install Dependencies
- Check code formatting (Prettier)
- Run automated tests
- Upload code coverage as an Artifact
- Cache Dependencies

2. Develop Branch – When a pull request is approved and Merging is accepted

- Repeats steps 1 – 4 as above
- Build project
- Upload builds as an Artifact
- Deploy to the staging server (Surge URL 1)
- Cache Dependencies

3. Master Branch – Pull Request to Merge into Master from Develop

- Install Dependencies
- Check code formatting (Prettier)
- Run automated tests
- Upload code coverage as an Artifact
- Cache Dependencies

4. Master Branch – When a pull request is approved and Merging is accepted

- Repeats steps 1 – 4 as above
- Build project
- Upload builds as an Artifact
- Create a release (If a new feature is merged and not a bug fix)
- Deploy to the production server (Surge URL 2)
- Send notification to Slack team on successful deployment of new release
- Upload coverage to [codecov](https://about.codecov.io/)
- Cache Dependencies

# Credits

Thanks to [Ali Alaa](https://github.com/alialaa) for his lecture on GitHub Actions and guide through the course of the project.
