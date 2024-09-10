# Deploy React.js app

This role will deploy a React app from a GitHub repository. It will
1. Clone the repo
1. Build the project on the target machine
1. Copy the build directory to the `html` folder
1. Perform cleanup

## Intended configuration

The app is placed in the `/var/www/html` folder to be served by a web server service. The configuration file `react_app_ip.j2` sets the IP address for the API calls performed by the app to be the target machine's IP address. In other words, the backend API should be on the same machine as the frontend UI.

## Variables

This role uses a few variables:
* `GitHub_react` specifies the GitHub repository for the React project to be hosted
* `ansible_default_ipv4.address` represents the target machine's IP address and is used in the template to tell the React app to make API calls to that IP
* `ansible_env.HOME` is the directory that is used as a temporary scratch area to build the project (files are removed in the end)
