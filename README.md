To run a custom Dockerfile e.g. Dockerfile.dev:

`$ docker build -f Dockerfile.dev .`

*We deleted local node_modules folder to avoid duplication, since another copy of node_module folder will be created in the docker image.*

To run the image locally:

`$ docker run -it -p 3000:3000 <image-id>`

## Using Docker Volumes
Instead of copying the folders from the project to the Docker container, we can use Docker Volumes to reflect changes that we make
in the local directory. This command creates a reference to the local directory so that the changes in that directory can be reflected
in the Docker container without restarting the container:

`$ docker run -it -p 3000:3000 -v /app/node_modules -v $(pwd):/app <image-id>`

*Recently, a bug was introduced with the latest Create React App version that is causing the React app to exit when starting with Docker Compose.*

To Resolve this:

Add `stdin_open` property to your `docker-compose.yml` file:

```
    web:
        stdin_open: true
```

Make sure you rebuild your containers after making this change with:

`$ docker-compose down && docker-compose up --build`

## To execute tests
`$ docker build -f Dockerfile.dev .`<br/>
`$ docker run <image-id> npm run test`  (this will run the tests once only)<br/>
`$ docker run -it <image-id> npm run test` (this will take an input parameter to run the tests if we need to run them more than once)


If you use docker-compose to run both the web and tests services: 
(by doing: `$ docker-compose up --build`)
any changes that are made in the code or tests will be reflected automatically
but you will not be able to send any input e.g. 'p', 'q', 't' or 'w' to investigate the tests more.

One way of running the tests along with using the input values is:

1. Run the app in docker container with out the test
2. then do: `$ docker exec -it <container-id>` 

## To build and run the production docker image
`$ docker build .`<br/>
`$ docker run -p 8080:80 <image-id>`


--------------------------------------------------------------------------------------------------------

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).


## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.<br />
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.<br />
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.<br />
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.<br />
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.<br />
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: https://facebook.github.io/create-react-app/docs/code-splitting

### Analyzing the Bundle Size

This section has moved here: https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size

### Making a Progressive Web App

This section has moved here: https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app

### Advanced Configuration

This section has moved here: https://facebook.github.io/create-react-app/docs/advanced-configuration

### Deployment

This section has moved here: https://facebook.github.io/create-react-app/docs/deployment

### `npm run build` fails to minify

This section has moved here: https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify
