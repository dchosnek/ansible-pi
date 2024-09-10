# Deploy Flask app

This role will deploy a REST API implemented by a Flask app from a GitHub repository. It will install the Python dependencies, retrieve the app from GitHub, and install Gunicorn to frontend the app.

## Intended configuration

The configuration for Gunicorn is specified by the template file `gunicorn.service.j2`. That file specifies 3 workers and runs the app at 127.0.0.1:5000. This template technically does not need to be a template since it does not contain any variables, but it might in the future.

## Variables

This role uses a few variables:
* `github_flask` specifies the GitHub repository for the Flask app to be hosted
* `package_list` represents the list of Python packages to be installed on the target
* `random_string` is generated and used by the role itself to provide the Flask app with a string to be used for JWT encoding and decoding (to avoid hard coding it someplace where it might be compromised)
