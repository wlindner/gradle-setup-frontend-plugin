# Gradle Setup Frontend Plugin [![Gradle Plugin Portal](https://img.shields.io/maven-metadata/v/https/plugins.gradle.org/m2/cool/william/frontend/cool.william.frontend.gradle.plugin/maven-metadata.xml.svg?colorB=007ec6&label=gradle%20plugin)](https://plugins.gradle.org/plugin/cool.william.frontend)
##### A gradle plugin that scaffolds a frontend and provides tasks for bundling.

### [Demo](https://docs.google.com/presentation/d/1JPTo-jqy0tPY99HUStG28UWLtAXQ8jMluH2oBU-3vjo/edit?usp=sharing)

### Getting Started
Add the frontend plugin to `build.gradle`
```
plugins {
  id 'cool.william.frontend' version '0.0.15'
}
```

[Scaffold](#scaffolding) the frontend. Choose between [React](https://reactjs.org/) or [Svelte](https://svelte.dev/)

#### React
```
$ ./gradlew setupReactFrontend
```

#### Svelte
```
$ ./gradlew setupSvelteFrontend
```

[Start](#frontend-start) the frontend in development mode
```
$ ./gradlew frontendStart
```

Build the frontend for distribution
```
$ ./gradlew frontendBuild
```

#### Integrate with Spring Boot
If you wish to use these gradle tasks with Spring Boot, add these lines to `build.gradle`

To start the frontend when Spring Boot starts
```
bootRun.dependsOn frontendStart
```

To build the frontend for distribution when Spring Boot is built into a jar
```
processResources.dependsOn frontendBuild
```

To configure Spring Boot to reload resources that change. Useful when running in development mode
https://docs.spring.io/spring-boot/docs/current/gradle-plugin/reference/html/#running-your-application-reloading-resources
```
bootRun {
    sourceResources sourceSets.main
}
```

### LiveReload
In order to get [LiveReload](http://livereload.com/) functionality during development, you need to install the LiveReload browser extension, or use this Index Controller that injects LiveReload into `index.html` during development.
https://github.com/wlindner/spring-boot-livereload-index-controller

### Scaffolding
This plugin scaffolds a frontend meaning that it creates a set of files necessary for a minimal frontend. Right now, only React is supported. 
- `package.json` for managing dependencies, metadata, and defining scripts.
- `frontend/` folder for storing the entry point (`index.js` and `index.html`) and components
- `webpack.config.js` for configuring webpack

Scaffolding is only meant to happen once, at the beginning of the project. After scaffolding, modify whatever you want, this is meant to be a minimal starting point.

### Frontend Start
The `frontendStart` gradle task simply runs `npm run start` which corresponds to the start script in `package.json`. It's perfectly fine to use NPM to run this script, but `frontendStart` has the added benefit of forking into a separate process and running while another task like `bootRun` is still running. That way webpack can run in watch mode continuously bundling the frontend while you develop your app.

### Frontend Build
The `frontendBuild` gradle task runs `npm run build` which builds the frontend in webpack's production mode so that it can be shipped off and deployed.
