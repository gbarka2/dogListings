# Deployment

Below are the steps for deployment for both a backend and frontend app...

### MongoDB Atlas

MongoDB.com can help you host your mongo database in the cloud. Here are the steps.

1.  Create an account
2.  Create a cluster (accept all defaults for free tier)
3.  Go to Network Access and select allow all IPs (0.0.0.0.0)
4.  Go over to database user and create a user
5.  Click on connect on your cluster
6.  Select Connect Application
7.  Select Node as your language
8.  Copy the URL and fill in the placeholders (remove the <> as you do so)

### The Heroku CLI

If you haven't already download the Heroku CLI at...
https://devcenter.heroku.com/articles/heroku-cli

Once installed run the command `heroku login` to login into heroku

#### Heroku Deployment Methods

There are 2 main ways to deploy via Heroku, the CLI method and the Github method.

- **CLI Method:** In this method you use the CLI to create a new heroku project and add a remote to your projects local git repository. You push your code directly to heroku. Since heroku can use the remote to determine which project the repo belongs to you don't need the --app flag on commands but you do need to make a separate push to heroku on top of your origin remote.

  1. Start a git repo in your project
  2. Add files and commit
  3. Run command `heroku create` to create project and heroku remote
  4. `git push heroku master` anytime you want to push to heroku
  5. Add config vars using command `heroku config:set KEY=VALUE`

- **Github Method** Instead of creating a project using the CLI you connect Heroku directly to a github repo with your project. The big benefit of this is continuous deployment, everytime you push to github it automatically redeploys on Heroku. The catch is that Heroku CLI won't automatically know which project the repo belongs to locally so you have to add the --app flag on all your cli commands.
  1. Start a git repo in your project
  2. Add files and commit
  3. Create a new project in the browser on the heroku dashboard
  4. Go to the deploy column
  5. Connect github repo
  6. enable automatic deploys
  7. head to settings tab and add config vars
  8. Go back to deploy tab and do a manual deploy

#### CLI Commands Reference

**NOTE** The --app=projectName flag must be added to any command if using the Github deployment method, not needed if you are using the heroku cli method and your terminal is in a folder within your repo.

`heroku logs --tail --app=projectName` This allows you to see your server logs, this is your first line of defense when trouble shooting deployments. Essentially this is the same output you'd see on terminal locally but from your deployed server.

`heroku create` creates a new project on your heroku account and adds a remote called "heroku" to the current local repository to push code to.

`heroku config --app=projectName` shows all heroku config variables for that project

`heroku config:set KEY=VALUE --app=projectName` sets a config variable for that project

`heroku run bash --app=projectName` opens up a bash console that is running on your projects server to run commands and explore files. Enter the exit command when done. This is mainly useful for running migrations when working with SQL databases (Unit 4).

`heroku run COMMAND` runs a single command in your project, basically like running bash but it runs one command and exits

### Backend App Deployment

1. Push the backend code up to github
2. Create a new Project on Heroku
3. Under the deploy tab select github and connect to repo
4. Make sure the config vars under settings match your .env (except PORT)
5. Make sure the MONGODBURI points to a database on MongoDB.com
6. Under deploy Enable Automatic Deploys and Trigger a Manual Deploy
7. Test your deployed API with postman

### Frontend App Deployment

1. Push the frontend code to github
2. Connect repo to netlify
3. Make sure code reflects deployed API URL not localhost
4. If the API calls fails cause of CORS errors, you need to add netlify URL to cors whitelist under configs
5. Keep testing till everything is working as expected

**HOW TO DEPLOY YOUR FRONTEND TO OTHER SERVICES:**

- https://www.youtube.com/watch?v=HCDCrjQsEhg (Github, Netlify, Surge, Heroku)
- https://youtu.be/2FVY_lm-mTY (Vercel)
- https://www.youtube.com/watch?v=DHLZAzdT44Y (Amazons AWS AMPLIFY)
- https://www.youtube.com/watch?v=1wZw7RvXPRU (Googles FIREBASE)
- https://www.youtube.com/watch?v=DRZ7FDZ0wlg (Microsofts AZURE STATIC)

Tada! You've deployed a full stack MERN application.

#### TROUBLE SHOOTING TIPS

- Heroku Logs are your best friend
- Always consult the devTools Network tab to assess failed API calls
- React devTools are a great way of monitoring data movement in your app
- Look at the Element tab to see if maybe your react component aren't render element in the order you expected
- Read the documentation for the CORS middleware, understanding CORS is key
- Make sure all your config vars are correct
- If using create-react-app make sure to add CI= to your build script
- If using react router at _redirects file in your public folder with ```/* /index.html 200```
