# Deploy your Blazor application to GitHub Pages

In this step, you will deploy your portfolio application to GitHub Pages. GitHub Pages is a static site hosting service that takes HTML, CSS, and JavaScript files directly from a repository on GitHub. You can use GitHub Pages to host a website about yourself, your organization, or your project directly from a GitHub repository.

In order to publish your portfolio application to GitHub Pages, you need to create GitHub Actions workflow. GitHub Actions is a powerful tool that allows you to automate, customize, and execute your software development workflows right in your repository. You can use GitHub Actions to build and test your code, deploy your application, and more.

OK. Let's get started!

## Step 0: Restore the Blazor WebAssembly project

If you haven't completed the previous step or want to start from the save point, run the following commands to restore the Blazor WebAssembly project.

```bash
cd $CODESPACE_VSCODE_FOLDER
mkdir -p workshop && cp -a save-points/step-06/. workshop/
cd workshop
```

## Step 1: Create GitHub Actions workflow

There is already a GitHub Actions workflow file in the `.github/workflows` directory, `publish-gh-pages.yml`. We'll slightly modify it so that our portfolio app is published to GitHub Pages.

1. Open the `publish-gh-pages.yml` file in the `.github/workflows` directory.
1. Update the `on` section to trigger the workflow to only pick up changes from our workshop directory.

    ```yaml
    # Before
    on:
      workflow_dispatch:
      push:
        paths:
          - 'src/MyPortfolio/**'
    
    # After
    on:
      workflow_dispatch:
      push:
        paths:
          - 'workshop/MyPortfolio/**'
    ```

1. Update the following steps to build and publish the application to GitHub Pages.

    ```yaml
    # Before
    - name: Restore NuGet packages
        shell: bash
        run: |
        dotnet restore ./src
    
    - name: Build solution
        shell: bash
        run: |
        dotnet build ./src -c Release
    
    - name: Test solution
        shell: bash
        run: |
        dotnet test ./src -c Release
    
    - name: Publish artifact
        shell: bash
        run: |
        dotnet publish ./src/MyPortfolio -c Release -o published

    # After
    - name: Restore NuGet packages
        shell: bash
        run: |
        dotnet restore ./workshop
    
    - name: Build solution
        shell: bash
        run: |
        dotnet build ./workshop -c Release
    
    - name: Test solution
        shell: bash
        run: |
        dotnet test ./workshop -c Release
    
    - name: Publish artifact
        shell: bash
        run: |
        dotnet publish ./workshop/MyPortfolio -c Release -o published
    ```

1. Run the following commands to commit and push the changes to the repository.

    ```bash
    git add .
    git commit -m "Add my portfolio app to publish to GitHub Pages"
    git push origin
    ```

1. Go to your GitHub repository and navigate to the `Settings` tab. Click the "Pages" menu at the left and choose the source to "GitHub Actions" under the "Build and deployment" section.

    ![GitHub Pages settings](./images/07-deploy-gh-pages-01.png)

1. Open the `MyPortfolio/Pages/Home.razor` file and add a blank line at the bottom of the page.
1. Run the following commands again to commit and push the changes to the repository.

    ```bash
    git add .
    git commit -m "Update my portfolio app to publish to GitHub Pages"
    git push origin
    ```

1. Check whether your GitHub Actions workflow is running.

    ![GitHub Actions workflow running](./images/07-deploy-gh-pages-02.png)

1. Confirm that your GitHub Actions workflow has completed successfully.

    ![GitHub Actions workflow completed](./images/07-deploy-gh-pages-03.png)

1. Open a new web browser tab and navigate to `https://{{your-github-username}}.github.io/codespaces-project-template-dotnet/`. You should see your portfolio application deployed to GitHub Pages.

    ![GitHub Pages](./images/07-deploy-gh-pages-04.png)

1. If you see this error page, you might need to update your GitHub Actions workflow.

    ![GitHub Pages error](./images/07-deploy-gh-pages-05.png)

1. Open the `publish-gh-pages.yml` file in the `.github/workflows` directory and update the following tasks and change the directory.

    ```yaml
    # Before
    - name: Set basepath on index.html
        shell: pwsh
        run: |
          $filepath = "./src/MyPortfolio/wwwroot/index.html"
          $repository = "${{ github.repository }}" -replace "${{ github.repository_owner }}", ""
      
          $html = Get-Content -Path $filepath
          $html -replace "<base href=`"/`" />", "<base href=`"$repository/`" />" | Out-File -Path $filepath -Force
    
    - name: Set basepath on JSON objects
        shell: pwsh
        run: |
          $filepath = "./src/MyPortfolio/wwwroot/sample-data"
          $repository = "${{ github.repository }}" -replace "${{ github.repository_owner }}", ""
      
          Get-ChildItem -Path $filepath -Filter "*.json" | ForEach-Object {
              $json = Get-Content -Path $_.FullName
              $json.Replace("../", "$repository/") | Out-File -Path $_.FullName -Force
          }

    # After
    - name: Set basepath on index.html
        shell: pwsh
        run: |
          $filepath = "./workshop/MyPortfolio/wwwroot/index.html"
          $repository = "${{ github.repository }}" -replace "${{ github.repository_owner }}", ""
      
          $html = Get-Content -Path $filepath
          $html -replace "<base href=`"/`" />", "<base href=`"$repository/`" />" | Out-File -Path $filepath -Force
    
    - name: Set basepath on JSON objects
        shell: pwsh
        run: |
          $filepath = "./workshop/MyPortfolio/wwwroot/sample-data"
          $repository = "${{ github.repository }}" -replace "${{ github.repository_owner }}", ""
      
          Get-ChildItem -Path $filepath -Filter "*.json" | ForEach-Object {
              $json = Get-Content -Path $_.FullName
              $json.Replace("../", "$repository/") | Out-File -Path $_.FullName -Force
          }
    ```

1. Open the `MyPortfolio/Pages/Home.razor` file and add a blank line at the bottom of the page.
1. Run the following commands again to commit and push the changes to the repository.

    ```bash
    git add .
    git commit -m "Update my portfolio app to publish to GitHub Pages"
    git push origin
    ```

1. Once the GitHub Actions workflow completes successfully, open a new web browser tab and navigate to `https://{{your-github-username}}.github.io/codespaces-project-template-dotnet/`. You should see your portfolio application deployed to GitHub Pages.

    ![GitHub Pages](./images/07-deploy-gh-pages-04.png)

---

Congratulations! You have deployed your portfolio application to GitHub Pages.

So far, you have created a new Blazor WebAssembly application, added a `Home` component, and updated the `Home` component by adding various sections including header, hero, about me, portfolio and footer. You have also extracted each section to component and refactored them to pass parameters from the parent component to child components. Finally, you have deployed your portfolio application to GitHub Pages.

You have completed the workshop! 🎉

**NOTE**: If you want to deploy your portfolio application to Azure, you can continue to the next step, [Step 8: Deploy your Blazor application to Azure](./08-deploy-azure.md). If you want to stop here, you can close your Codespace instance and delete the Codespace instance from the GitHub repository.
