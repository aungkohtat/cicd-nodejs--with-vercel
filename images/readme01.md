
1. Go to [vercel.com](https://vercel.com) and login.
2. Go to ACCOUNT SETTING.
![](/images/Screenshot%202024-08-24%20at%2012.41.51 PM.png)

3.  Go to TOKEN.

![](/images/Screenshot%202024-08-24%20at%2012.48.03 PM.png)

4. Create a token with TOKEN NAME, SCOPE & EXPIRATION.

5. Go to [github.com](https://github.com) login. Create new repo and then moved to Actions.

![](/images/Screenshot%202024-08-24%20at%2012.52.26 PM.png)

6. Create a file with main.yml

```
name: Vercel development Deployment
env:
  #THIS IS PROJECT INFORMATION WHICH COMES FROM .VERCEL/PROJECT.JSON FILE.
  # THIS KEY VALUE STORE IN GITHUB ACTION. 
  # GITHUB.COM > SETTINGS > SECRETS AND VARIABLES > ACTIONS > REPOSITORY SECRETS.
  VERCEL_ORG_ID: ${{ secrets.VERCEL_ORG_ID }}
  VERCEL_PROJECT_ID: ${{ secrets.VERCEL_PROJECT_ID }}
on:
  push:
    branches:
      - main
jobs:
  Deploy-development:
    runs-on: ubuntu-latest
    steps:
      #INSTALL NODEJS & NPM ON VERCEL
      - uses: actions/checkout@v2
      - name: Install Node.js
        uses: actions/setup-node@v3

      - name: Install npm
        uses: actions/setup-node@v3
        with:
          node-version: '20'
          registry-url: 'https://registry.npmjs.org/'
      #INSTALL DEPENDENCIES
      - name: Install dependencies
        run: npm install  
      #INSTALL VERCEL CLI IN SERVER.
      - name: Install Vercel CLI
        run: npm install --global vercel@latest
      #SET ENVIRONMENT IN VERCEL. 
      #CREATE ENVIRONMENT IN VERCEL & BUILD IN VERCEL.
      # GITHUB.COM > SETTINGS > SECRETS AND VARIABLES > ACTIONS > ENVRIONEMENT SECRETS.
      - name: Pull Vercel Environment Information
        run: vercel pull --yes --environment=development --token=${{ secrets.VERCEL_TOKEN }}
      - name: Build Project Artifacts
        run: vercel build --prod --token=${{ secrets.VERCEL_TOKEN }}
      - name: Deploy Project Artifacts to Vercel  
        run: vercel deploy --prebuilt --prod --token=${{ secrets.VERCEL_TOKEN }} 
        
```

add code and save it.
8. Go to the Setting section in GitHub.com

![](/images/Screenshot%202024-08-24%20at%2012.52.26 PM.png)

9. Click on SECRETS AND VARIABLES > ACTIONS


10. Add Repository secrets like VERCEL_TOKEN which is created from token sections of vercel website.
It’s time to test out Github WorkFlow. Let’s push the code in the main branch.


