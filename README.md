<div align="center">
    <h2>
      ⚙ <img src="./public/logo192.png" width="40x"/>
      REACT &nbsp; D E P L O Y 
      <img src="./public/logo192.png" width="40x"/> ⚙
    </h2>
    <a href="https://andredavedovicz.github.io/react-deploy/" target="_blank">🔗 Working in GitHub Pages</a>
    <h3>How add a React project(deploy) to GitHub Pages? </h3>
    <h3>Whenever you push to GitHub, it will deploy automatically!</h3>
    <h4>Vite: <a href="https://github.com/andredavedovicz/vite-deploy" target="_blank">If you want to deploy a vite app see this repository!</a></h4>
    
</div>



<br>

### Follow the steps below on how to deploy a vite react app:

#### 01. Create a vite react app
```npm
npx create-react-app [REPO_NAME]
cd [REPO_NAME]
```

#### 02. Create a new repository on GitHub and initialize GIT
```git
git init 
git add . 
git commit -m "add: initial files" 
git branch -M main 
git remote add origin https://github.com/[USER]/[REPO_NAME] 
git push -u origin main
```

#### 03. Setup homepage on package.json
```js
"homepage": "https://[USER].github.io/[REPO_NAME]/"
```

#### 04. Create ./github/workflows/deploy.yml and add the code bellow
```yml
name: Build & deploy

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v2
    
    - name: Install Node.js
      uses: actions/setup-node@v1
      with:
        node-version: 13.x
    
    - name: Install NPM packages
      run: npm ci
    
    - name: Build project
      run: npm run build
    
    - name: Run tests
      run: npm run test

    - name: Upload production-ready build files
      uses: actions/upload-artifact@v2
      with:
        name: production-files
        path: ./build
  
  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - name: Download artifact
      uses: actions/download-artifact@v2
      with:
        name: production-files
        path: ./build

    - name: Deploy to gh-pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./build
```

#### 05. Push
```git
git add . 
git commit -m "add: deploy workflow" 
git push
```

#### 06. Active workflow
```js
Settings -> Actions -> General -> Workflow permissions -> Read and Write permissions 
Settings -> Pages -> gh-pages -> save
(if gh-pages it's not showing something is wrong!) 
Actions -> failed deploy -> re-run-job failed jobs 

```

#### 06. For code changes
```git
git add . 
git commit -m "fix: some bug" 
git push
```
Remember: Whenever you push to GitHub, it will deploy automatically
