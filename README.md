# Test github actions deploy to heroku 

## Create a base react app 

Create a base react application to test your deploy. First, open a console and clone your github repository that you want to use:

```
git clone <repo-name>
```
Then, without entering your repository, type the following command to create the application:

```
npx create-react-app <repo-name>
```

Then go into the repository and launch it to verify that everything is fine:

```
cd <repo-name>
npm start
```

This should open your browser and display the react logo. As we only want to test the deployment, we leave the application like this.

## Deploy with github actions  
Moving on to the implementation, we need to create a file that the github actions can read and execute the steps. For this, create a folder called `.github` and inside create another called` workflows`, this is the path where github serch for actions. Next, create a file called `deploy.yml` that will contain the action configuration in this format: 

```
# action name 
name: Deploy

on:
  push:
    branches:
      # branch to deploy on push
      -master
  pull_request:
    branches:
      # branch to deploy on a pull request
      -master
      

jobs:
  build:
    # operatin system version that will run it on
    runs-on: ubuntu-latest
    steps:
      # action for checking out the repo
      - uses: actions/checkout@v2
      # action for deploy to heroku
      - uses: akhileshns/heroku-deploy@v3.4.6 
        with:
          # heroku credentials
          heroku_api_key: ${{secrets.HEROKU_API_KEY}}
          heroku_app_name: ******** # must be unique
          heroku_email: ********
```

To get the heroku API key, follow the documentation at https://devcenter.heroku.com/articles/authentication. It is highly recommended to store your key in secrets in the repository settings and call the key with the code above.

Finally, push the changes to the repository and make another push or pull request on the branch you configured to test that the deployment is working. 