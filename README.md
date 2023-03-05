# GitHub Actions Pages Deploy Demo

This repository aims to learn how to use GitHub actions to build and deploy the product to GitHub pages.

## Create a project

Use vite to generate a new project, enter the following command in the terminal `pnpm create vite`, and according to the hint choose the framework. For more details see the [official docs](https://vitejs.dev/guide/).

Create a  new GitHub repository and follow the quick setup to attach the repository and local project.

## Add GitHub Actions

Add GitHub Actions to this repository, Firstly we need to create a new folder named .github, and create another new folder named workflows in .github directory.

In the workflows folder, we create a file, which extension name is yml, and the file name is casual. I created a file named ci. And add those content to this file.

```yml
name: Build and Deploy
on: [push]
permissions:
  contents: write
jobs:
  build-and-deploy:
    concurrency: ci-${{ github.ref }} # Recommended if you intend to make multiple deployments in quick succession.
    runs-on: ubuntu-latest
    steps:
      # get source code
      - name: checkout
        uses: actions/checkout@v3
      - name: pnpm 
        uses: pnpm/action-setup@v2
        with:
          version: 7.18.2
      - name: build
        run: rm -rf node_modules && pnpm install && pnpm build

      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          folder: dist # The folder the action should deploy.
```

> See the documentation for [GitHub Actions](https://docs.github.com/en/actions) configuration

##  Modify config

We need to add a base config to our project since we deploy our project in GitHub. This config can be seen in [official docs](https://vitejs.dev/guide/static-deploy.html).

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react-swc'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  base: '/${REPOSITORY_NAME}/'
})

```

When modify completed, don't forget to commit and push your changes to GitHub. When the push process is finished. the Github will perform CI/CD automatically.
After the CI/CD is finished, we need to open this setting, thus we can visit our GitHub pages

![image-20230305143018114](https://raw.githubusercontent.com/fengnzl/HexoImages/master/blog/202303051430567.png)
