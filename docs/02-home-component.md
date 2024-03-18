# Modify `Home` component

In this step, you will update the existing `Home` component. The `Home` component will be the first page of your portfolio site. It will include your name, a brief introduction about yourself, and a photo of yourself.

Blazor apps are built using components. A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form. A component includes HTML markup and the processing logic to enable dynamic behaviours like rendering the UI. The components can be nested, reused and shared among other projects.

OK. Let's get started!

## Step 0: Restore the Blazor WebAssembly project

If you haven't completed the previous step or want to start from the save point, run the following commands to restore the Blazor WebAssembly project.

```bash
cd $CODESPACE_VSCODE_FOLDER
mkdir -p workshop && cp -a save-points/step-01/. workshop/
cd workshop
```

## Step 1: Clean up layout

We need to clean up the default layout to make our portfolio site look better.

1. Make sure you're in the `workshop` directory.

    ```bash
    cd $CODESPACE_VSCODE_FOLDER/workshop
    ```

1. Delete the files by running the following commands or on the codespace workspace on the side.

    ```bash
    rm MyPortfolio/Layout/NavMenu.razor
    rm MyPortfolio/Layout/NavMenu.razor.css
    rm MyPortfolio/Pages/Counter.razor
    rm MyPortfolio/Pages/Weather.razor
    rm MyPortfolio/wwwroot/sample-data/weather.json
    ```

1. Update the `MainLayout.razor` file to remove the navigation menu. Open `MyPortfolio/Layout/MainLayout.razor` and replace the content with the following code:

    ```csharp
    @inherits LayoutComponentBase
    
    <div id="main">
        @Body
    </div>
    ```

1. Copy the site layout CSS from the `docs/css` directory to the `MyPortfolio/wwwroot/css` directory.

    ```bash
    mkdir -p $CODESPACE_VSCODE_FOLDER/workshop/MyPortfolio/wwwroot/css
    cp -a $CODESPACE_VSCODE_FOLDER/docs/css/. $CODESPACE_VSCODE_FOLDER/workshop/MyPortfolio/wwwroot/css/
    ```

1. Copy the relevant images from the `docs/images` directory to the `MyPortfolio/wwwroot/images` directory.

    ```bash
    mkdir -p $CODESPACE_VSCODE_FOLDER/workshop/MyPortfolio/wwwroot/images
    cp -a $CODESPACE_VSCODE_FOLDER/docs/images/. $CODESPACE_VSCODE_FOLDER/workshop/MyPortfolio/wwwroot/images/
    ```

## Step 2: Update `Home` component

Since there is already the `Home` component, we will update the `Home` component to include your name, a brief introduction about yourself, and a photo of yourself.

1. Open `MyPortfolio/Pages/Home.razor` and update the content with the following code:

    ```csharp
    @page "/"
    @using System.Net.Http.Json
    @using MyPortfolio.Models
    @inject HttpClient Http
        
    <PageTitle>My Portfolio</PageTitle>
    ```

1. Below the `PageTitle` component, add the following code. This is the HTML part of the `Home` component.

    ```csharp
    @page "/"
    @using System.Net.Http.Json
    @using MyPortfolio.Models
    @inject HttpClient Http
    
    <PageTitle>My Portfolio</PageTitle>
    
    @* ⬇️⬇️⬇️ Add codes below ⬇️⬇️⬇️ *@
    
    <section class="dark" id="home">
        @if (heroHome is not null)
        {
            <img class="background" src="@(heroHome.Src)" alt="@(heroHome.Alt)" />
        }
        <div style="position: absolute; top: 30%; left: 2rem;">
        @if (property is null)
        {
            <p><em>Loading...</em></p>
        }
        else
        {
            <h1>@property.Name</h1>
            <h2>@property.Title</h2>
        }
        </div>
        <div style="position: absolute; bottom: 8rem; left: 50%;">
            <a href="#about" target="_top">
                <img src="images/down-arrow.svg" style="height: 3rem; width: 3rem;" alt="scroll down" />
            </a>
        </div>
    </section>
    ```

1. Below the `</section>` tag, add the following code. This is the C# code that handles the UI logic. The logic reads both `siteproperties.json` and `heroimages.json` files to display the hero image and the site properties.

    ```csharp
    </section>

    @* ⬇️⬇️⬇️ Add codes below ⬇️⬇️⬇️ *@

    @code {
        private SiteProperties? property;
        private HeroImage? heroHome;
    
        protected override async Task OnInitializedAsync()
        {
            property = await Http.GetFromJsonAsync<SiteProperties>("sample-data/siteproperties.json");
    
            var heros = await Http.GetFromJsonAsync<List<HeroImage>>("sample-data/heroimages.json");
            heroHome = heros.SingleOrDefault(h => h.Name == "home");
        }
    }
    ```

## Step 3: Add `siteproperties.json` and `heroimages.json`

Both `siteproperties.json` and `heroimages.json` files are base data to display the hero image and the site properties. We will add these files to the `wwwroot/sample-data` directory.

1. Make sure you're in the `workshop` directory.

    ```bash
    cd $CODESPACE_VSCODE_FOLDER/workshop
    ```

1. Create both files in the `wwwroot/sample-data` directory by running the following commands or on the codespace workspace on the side.

    ```bash
    touch MyPortfolio/wwwroot/sample-data/siteproperties.json
    touch MyPortfolio/wwwroot/sample-data/heroimages.json
    ```

1. Open `siteproperties.json` and add the following content:

    ```json
    {
      "name": "{{your-name}}",
      "title": "{{your-title}}"
    }
    ```

1. Open `heroimages.json` and add the following content:

    ```json
    [
      {
        "name": "home",
        "src": "images/woman-with-tablet.jpg",
        "alt": "woman with glasses holding a tablet"
      }
    ]
    ```

## Step 4: Add `SiteProperties` and `HeroImage` classes

Reading JSON data in Blazor is done by creating classes that represent the JSON data. We will create `SiteProperties` and `HeroImage` classes to represent the `siteproperties.json` and `heroimages.json` files.

1. Make sure you're in the `workshop` directory.

    ```bash
    cd $CODESPACE_VSCODE_FOLDER/workshop
    ```

1. Create both `SiteProperties` and `HeroImage` classes in the `Models` directory by running the following commands or on the codespace workspace on the side.

    ```bash
    mkdir -p $CODESPACE_VSCODE_FOLDER/workshop/MyPortfolio/Models
    touch MyPortfolio/Models/SiteProperties.cs
    touch MyPortfolio/Models/HeroImage.cs
    ```

1. Open `SiteProperties.cs` and add the following content:

    ```csharp
    namespace MyPortfolio.Models;
    
    public class SiteProperties
    {
        public string Name { get; set; } = string.Empty;
        public string Title { get; set; } = string.Empty;
    }
    ```

1. Open `HeroImage.cs` and add the following content:

    ```csharp
    namespace MyPortfolio.Models;
    
    public class HeroImage
    {
        public string Name { get; set; } = string.Empty;
        public string Src { get; set; } = string.Empty;
        public string Alt { get; set; } = string.Empty;
    }
    ```

## Step 5: Build and run the Blazor WebAssembly application

1. Make sure you're in the `workshop` directory.

    ```bash
    cd $CODESPACE_VSCODE_FOLDER/workshop
    ```

1. Build your Blazor WebAssembly project to make sure everything is working fine.

    ```bash
    dotnet build
    ```

1. Run the Blazor WebAssembly project to see the updated application on your web browser.

    ```bash
    dotnet watch run --project MyPortfolio
    ```

1. The updated application is open, and should look like below.

    ![Home page](./images/02-home-component-01.png)

1. Stop the running application by pressing `Ctrl + C` in the terminal.

---

Congratulations! You have updated the `Home` component to include your name, a brief introduction about yourself, and a photo of yourself. Let's create a new component for the `About me` in the next step.

:point_right: [Step 3: Create a About component](./03-about-component.md)
