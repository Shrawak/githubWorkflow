# Introduction to Github Packages

## What is a package?

A package is a bundle of code, configurations, and other resources that is built into a single versioned archive file. A package contains all the information necessary to allow a developer to use the package in their own projects. Packages can contain source code, compiled code, or any other type of file you want to include in your project. Packages can be used to share code within your organization, with other organizations, or with any developer on GitHub.

## What is a package registry?

A package registry is a service that allows you to host packages and manage their lifecycles. A package registry allows you to publish packages, install them as dependencies, and manage their versions. A package registry can be public or private. A public package registry allows anyone to publish and install packages. A private package registry allows only authorized users to publish and install packages. Npm is a public package registry. GitHub Packages is a private package registry.

We’ve come to a point in your developer where we seem to be repeating and/or copy~pasting the same piece of code across multiple projects. It could be some utility functions that are really helpful for the way you like to do things in code or it could a set of typescript types, you wish your frontend and backend code to share — whatever the case you’ll probably want to create your own npm package.

But you might ask, “Why not use npmjs.com?” If you don’t mind your package to be public, sure, by all means, however, private packages on there will cost you money, not a lot, but some, which is not the case with Github package registry.

## What is GitHub Packages?

GitHub Packages is a package hosting service that allows you to host your software packages privately or publicly and use packages as dependencies in your projects. GitHub Packages is built with the latest edge caching via a global CDN to deliver great performance, no matter where your builds run. GitHub Packages is available with GitHub Free, GitHub Pro, GitHub Free for organizations, GitHub Team, GitHub Enterprise Cloud, and GitHub One. It also integrates with GitHub Actions, allowing you to automate your CI/CD workflows and publish packages
directly from your repository.

## CI/CD

CI/CD stands for Continuous Integration and Continuous Delivery/Deployment. It is a set of practices that helps teams to automate the building, testing, and deployment of software applications. CI/CD helps to reduce the time and effort required to release new features or updates to the software application. It enables teams to quickly respond to customer feedback and market demands. CI/CD is a combination of two practices: Continuous Integration and Continuous Delivery/Deployment. Let’s understand these two practices in detail.

## CI/CD Pipeline

CI/CD Pipeline is a set of practices that helps teams to automate the building, testing, and deployment of software applications. It is a process of automating the entire software release process. CI/CD Pipeline helps to build, test, and deploy code changes to production environments. It enables teams to release new features to customers quickly and safely. CI/CD Pipeline helps to reduce the time and effort required to release new features or updates to the software application.

## Github actions and workflow

A tool that lets you automate your software development workflows. An individual tasks is called actions, and combination of actions create a custom workflow

Workflows are custom automated processes that can be set up in a repository to build, test, package release, or deploy any code project on Github

## CI/CD using GitHub Actions

GitHub Actions is a CI/CD platform that helps teams to automate the building, testing, and deployment of software applications. It is a process of automating the entire software release process. GitHub Actions helps to build, test, and deploy code changes to production environments. It enables teams to release new features to customers quickly and safely. GitHub Actions helps to reduce the time and effort required to release new features or updates to the software application.

## How to publish an npm package to the github package respository.

- Code the node.js project you want to publish as npm module in github repo.
- Create a github repository in which you will be publishing the package.

### Create the package.json file

- Create a package.json file in the root of your project directory.

When you found a package name preceded by an @ in its name, then it is a scoped package. For example, @shrawak/eslint-config-base is a package under @ scope. GitHub Packages has a specific rule for package scope.

- Each package published will always be a scoped package.
- The owner of the repository will be the scope for a published package. If you publish a package from a repository under an organization, then your organization name will be the scope and the package name should be @YOUR_ORG/package-name. If the package coming from your repository, then the package name should be @YOUR_USERNAME/package-name.
  In the next sections, we will assume that the package will be published under an personal, so we will see a lot of @YOUR_USERNAME/package-name examples. The way to publish a package is pretty much the same for an organization or personal scope.

### Setting up Personal Access Token

For this step, we’ll need to generate a github token that will allow us to publish later. To do that, go to https://github.com/settings/tokens and press “Generate new token” and make sure to assign the token the following scopes:

After hitting “Generate token”, we will be presented with the token like so, make sure to note it down somewhere before navigating away:

### Add token to your repo

Here you’ll be doing two things:

Create a file called .npmrc at the root of your repository and set the contents to the following:

````@YOUR_GITHUB_USERNAME:registry=https://npm.pkg.github.com/YOUR_GITHUB_USERNAME
//npm.pkg.github.com/:_authToken=YOUR_GITHUB_TOKEN
registry=https://registry.npmjs.org ```

and fill in YOUR_GITHUB_USERNAME and YOUR_GITHUB_TOKEN respectively.

2. Open up package.json that’s also at the root of your repository, change the value of name and add repository entry below name.

```json

"publishConfig": {
  "@YOUR_ORG:registry": "https://npm.pkg.github.com"
}

````

### Publish a version of your package using github actions

Now that we have our package ready to be published, we need to create a workflow that will publish our package to the github package registry. For that, we need to create a file called .github/workflows/publish.yml and set the contents of that file to the following:

```
name: Publish package
on:
  push:
    branches:
      - main
jobs:
    publish:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v3
        - uses: actions/setup-node@v3
            with:
            node-version: 18
            registry-url: https://npm.pkg.github.com/
        - run: npm ci
        - run: npm publish
            env:
            NODE_AUTH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

### Consume your package

First open up the project, in which you would like to use your newly created package. Since it is not any old regular package that just lives in the npmjs, we need to let our project know where to look for this package.

For that at the root of our consumer project, we again need to create a file called .npmrc and set the contents of that file to the following:

````

//npm.pkg.github.com/:\_authToken=YOUR_GITHUB_TOKEN
registry=https://npm.pkg.github.com

```

Finally, run npm install @YOUR_USERNAME/YOUR_PACKAGE_NAME@0.1.0 and live happily ever after 🙃
```

````
