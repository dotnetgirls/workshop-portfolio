# Split sections into components

In this step, you will split the `Home` component into individual components. The `Home` component will be the placeholder page of your portfolio site. Each section will become a component like `Header`, `Hero`, `AboutMe`, `Portfolio` and `Footer`.

OK. Let's get started!

## Step 0: Restore the Blazor WebAssembly project

If you haven't completed the previous step or want to start from the save point, run the following commands to restore the Blazor WebAssembly project.

```bash
cd $CODESPACE_VSCODE_FOLDER
mkdir -p workshop && cp -a save-points/step-05/. workshop/
cd workshop
```

## Step 1: Create `HeaderComponent` component

1. Create a new file called `HeaderComponent.razor` in the `MyPortfolio/Components` directory.

    ```bash
    mkdir -p $CODESPACE_VSCODE_FOLDER/workshop/MyPortfolio/Components
    touch MyPortfolio/Components/HeaderComponent.razor
    ```

1. Copy the following content from `MyPortfolio/Pages/Home.razor` to `MyPortfolio/Components/HeaderComponent.razor`:

    ```csharp
    <div id="header">
        <a href="#home" target="_top">Home</a>
        <a href="#about" target="_top">About</a>
        <a href="#portfolio" target="_top">Portfolio</a>
        <a href="#contact" target="_top">Contact</a>
    </div>
    ```

1. Create a new file called `HeaderComponent.razor.css` in the `MyPortfolio/Components` directory.

    ```bash
    touch MyPortfolio/Components/HeaderComponent.razor.css
    ```

1. Copy the following content from `MyPortfolio/Pages/Home.razor` to `MyPortfolio/Components/HeaderComponent.razor.css`:

    ```css
    #header {
      position: fixed;
      display: flex;
      justify-content: center;
      gap: 2rem;
      background: rgba(255,255,255,0.75);
      padding: 1rem;
      top: 0;
      width: 100%;
      z-index: 10;
    }
    ```

## Step 2: Create `HeroComponent` component

1. Create a new file called `HeroComponent.razor` in the `MyPortfolio/Components` directory.

    ```bash
    touch MyPortfolio/Components/HeroComponent.razor
    ```

1. Add the following using statements to the top of the `MyPortfolio/Components/HeroComponent.razor` file:

    ```csharp
    @using System.Net.Http.Json
    @using MyPortfolio.Models
    @inject HttpClient Http
    ```

1. Copy the following HTML content from `MyPortfolio/Pages/Home.razor` to `MyPortfolio/Components/HeroComponent.razor`:

    ```csharp
    @inject HttpClient Http

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

1. Copy the following code block from `MyPortfolio/Pages/Home.razor` to `MyPortfolio/Components/HeroComponent.razor`:

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

## Step 3: Create `AboutMeComponent` component

1. Create a new file called `AboutMeComponent.razor` in the `MyPortfolio/Components` directory.

    ```bash
    touch MyPortfolio/Components/AboutMeComponent.razor
    ```

1. Add the following using statements to the top of the `MyPortfolio/Components/AboutMeComponent.razor` file:

    ```csharp
    @using System.Net.Http.Json
    @using MyPortfolio.Models
    @inject HttpClient Http
    ```

1. Copy the following HTML content from `MyPortfolio/Pages/Home.razor` to `MyPortfolio/Components/AboutMeComponent.razor`:

    ```csharp
    @inject HttpClient Http

    @* ⬇️⬇️⬇️ Add codes below ⬇️⬇️⬇️ *@

    <section class="light" id="about">
        @if (heroAboutMe is not null)
        {
            <img class="background" src="@(heroAboutMe.Src)" alt="@(heroAboutMe.Alt)" />
        }
        <h2>About Myself</h2>
        <div style="background-color: white; width: 50%; padding: 4rem; margin: 3rem auto; text-align: center;">
        @if (aboutMe is null)
        {
            <p><em>Loading...</em></p>
        }
        else
        {
            <p class="large">@aboutMe.Description</p>
            <hr />
            <ul style="text-align: left; columns: 2; font-size: 1.25rem; margin: 2rem 3rem; gap: 3rem;">
            @foreach (var skill in aboutMe.Skills)
            {
                <li key="@skill">@skill</li>
            }
            </ul>
            <hr />
            <p style="padding: 1rem 3rem 0;">@aboutMe.DetailOrQuote</p>
        }
        </div>
    </section>
    ```

1. Copy the following code block from `MyPortfolio/Pages/Home.razor` to `MyPortfolio/Components/AboutMeComponent.razor`:

    ```csharp
    </section>
    
    @* ⬇️⬇️⬇️ Add codes below ⬇️⬇️⬇️ *@

    @code {
        private AboutMe? aboutMe;
        private HeroImage? heroAboutMe;
    
        protected override async Task OnInitializedAsync()
        {
            aboutMe = await Http.GetFromJsonAsync<AboutMe>("sample-data/aboutme.json");
    
            var heros = await Http.GetFromJsonAsync<List<HeroImage>>("sample-data/heroimages.json");
            heroAboutMe = heros.SingleOrDefault(h => h.Name == "about");
        }
    }
    ```

## Step 4: Create `PortfolioComponent` component

1. Create a new file called `PortfolioComponent.razor` in the `MyPortfolio/Components` directory.

    ```bash
    touch MyPortfolio/Components/PortfolioComponent.razor
    ```

1. Add the following using statements to the top of the `MyPortfolio/Components/PortfolioComponent.razor` file:

    ```csharp
    @using System.Net.Http.Json
    @using MyPortfolio.Models
    @inject HttpClient Http
    ```

1. Copy the following HTML content from `MyPortfolio/Pages/Home.razor` to `MyPortfolio/Components/PortfolioComponent.razor`:

    ```csharp
    @inject HttpClient Http

    @* ⬇️⬇️⬇️ Add codes below ⬇️⬇️⬇️ *@

    <section class="light" id="portfolio">
        <h2>Portfolio</h2>
        <div class="portfolio-container">
        @if (projects is null)
        {
            <p><em>Loading...</em></p>
        }
        else
        {
            <div class="portfolio-hero">
                @if (heroPortfolio is not null)
                {
                    <img src="@(heroPortfolio.Src)" style="height: 90%; width: 100%; object-fit: cover;" alt="@(heroPortfolio.Alt)" />
                }
            </div>
            <div class="container">
                @foreach (var project in projects)
                {
                    <div class="box" key="@project.Title">
                        <a href="@project.Url" target="_blank" rel="noopener noreferrer">
                            <h3 style="flex-basis: 40px;">@project.Title</h3>
                        </a>
                        <p class="small">@project.Description</p>
                    </div>
                }
            </div>
        }
        </div>
    </section>
    ```

1. Copy the following code block from `MyPortfolio/Pages/Home.razor` to `MyPortfolio/Components/PortfolioComponent.razor`:

    ```csharp
    </section>
    
    @* ⬇️⬇️⬇️ Add codes below ⬇️⬇️⬇️ *@

    @code {
        private List<Project>? projects;
        private HeroImage? heroPortfolio;
        
        protected override async Task OnInitializedAsync()
        {
            projects = await Http.GetFromJsonAsync<List<Project>>("sample-data/projects.json");
    
            var heros = await Http.GetFromJsonAsync<List<HeroImage>>("sample-data/heroimages.json");
            heroPortfolio = heros.SingleOrDefault(h => h.Name == "portfolio");
        }
    }
    ```

## Step 5: Create `FooterComponent` component

1. Create a new file called `FooterComponent.razor` in the `MyPortfolio/Components` directory.

    ```bash
    touch MyPortfolio/Components/FooterComponent.razor
    ```

1. Add the following using statements to the top of the `MyPortfolio/Components/FooterComponent.razor` file:

    ```csharp
    @using System.Net.Http.Json
    @using MyPortfolio.Models
    @inject HttpClient Http
    ```

1. Copy the following HTML content from `MyPortfolio/Pages/Home.razor` to `MyPortfolio/Components/FooterComponent.razor`:

    ```csharp
    @inject HttpClient Http

    @* ⬇️⬇️⬇️ Add codes below ⬇️⬇️⬇️ *@

    <div id="contact" style="background-color: #4E567E;">
        @if (property is null)
        {
            <div style="display: flex; justify-content: center; gap: 2.5rem;">
                <p><em>Loading...</em></p>
            </div>
        }
        else if (icons is not null)
        {
            <div style="display: flex; justify-content: center; gap: 2.5rem;">
                @if (string.IsNullOrWhiteSpace(property.Email) is false)
                {
                    <a href="mailto:@(property.Email)">
                        <img src="@icons.Email" alt="email" class="social-icon" />
                    </a>
                }
                @if (string.IsNullOrWhiteSpace(property.LinkedIn) is false)
                {
                    <a href="https://linkedin.com/in/@(property.LinkedIn)" target="_blank" rel="noopener noreferrer">
                        <img src="@icons.LinkedIn" alt="LinkedIn" class="social-icon" />
                    </a>
                }
                @if (string.IsNullOrWhiteSpace(property.GitHub) is false)
                {
                    <a href="https://github.com/@(property.GitHub)" target="_blank" rel="noopener noreferrer">
                        <img src="@icons.GitHub" alt="GitHub" class="social-icon" />
                    </a>
                }
            </div>
            <p class="small" style="margin-top: 0; color: white;">Created by @property.Name</p>
        }
    </div>
    ```

1. Copy the following code block from `MyPortfolio/Pages/Home.razor` to `MyPortfolio/Components/FooterComponent.razor`:

    ```csharp
    </section>
    
    @* ⬇️⬇️⬇️ Add codes below ⬇️⬇️⬇️ *@

    @code {
        private SiteProperties? property;
        private SocialIcons? icons;
        
        protected override async Task OnInitializedAsync()
        {
            property = await Http.GetFromJsonAsync<SiteProperties>("sample-data/siteproperties.json");
    
            icons = await Http.GetFromJsonAsync<SocialIcons>("sample-data/socialicons.json");
        }
    }
    ```

1. Create a new file called `FooterComponent.razor.css` in the `MyPortfolio/Components` directory.

    ```bash
    touch MyPortfolio/Components/FooterComponent.razor.css
    ```

1. Copy the following content from `MyPortfolio/Pages/Home.razor` to `MyPortfolio/Components/FooterComponent.razor.css`:

    ```css
    #contact {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 2.5rem;
      padding: 5rem 0 3rem;
      width: 100vw;
    }
    ```

## Step 6: Update `Home` component

We've so far split all the sections into individual components. Now, we need to update the `Home` component to replace the sections with the components.

1. Open `MyPortfolio/Pages/Home.razor` and replace the content with the following code:

    ```csharp
    @page "/"
    @using MyPortfolio.Components
    
    <PageTitle>My Portfolio</PageTitle>
    
    <HeaderComponent />
    <HeroComponent />
    <AboutMeComponent />
    <PortfolioComponent />
    <FooterComponent />
    ```

## Step 7: Build and run the Blazor WebAssembly application

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

    ![Componentised page](./images/06-split-components-01.png)

1. Stop the running application by pressing `Ctrl + C` in the terminal.

## Step 8: Parameterise the components

You might have found that each component is using the same `HttpClient` to fetch the JSON data. It's not efficient to fetch the same data multiple times. We can parameterise the components to pass the data from the `Home` component.

### `HeroComponent` component

1. Open `MyPortfolio/Components/HeroComponent.razor`
1. Remove the `@inject HttpClient Http` directive from the top of the file so that it will be:

    ```csharp
    @using System.Net.Http.Json
    @using MyPortfolio.Models
    
    <section class="dark" id="home">
    ```

1. Replace the code block with the following code. As you can see both `HttpClient` and `HeroImages` are now parameters and accept values from their parent component, `Home`.

    ```csharp
    @code {
        private SiteProperties? property;
        private HeroImage? heroHome;
    
        [Parameter, EditorRequired]
        public HttpClient? Http { get; set; } = default!;
    
        [Parameter, EditorRequired]
        public List<HeroImage> HeroImages { get; set; } = default!;
        
        protected override async Task OnInitializedAsync()
        {
            property = await Http.GetFromJsonAsync<SiteProperties>("sample-data/siteproperties.json");
            heroHome = HeroImages.SingleOrDefault(h => h.Name == "home");
        }
    }
    ```

### `AboutMeComponent` component

1. Open `MyPortfolio/Components/AboutMeComponent.razor`
1. Remove the `@inject HttpClient Http` directive from the top of the file so that it will be:

    ```csharp
    @using System.Net.Http.Json
    @using MyPortfolio.Models

    <section class="light" id="about">
    ```

1. Replace the code block with the following code. As you can see both `HttpClient` and `HeroImages` are now parameters and accept values from their parent component, `Home`.

    ```csharp
    @code {
        private AboutMe? aboutMe;
        private HeroImage? heroAboutMe;
        
        [Parameter, EditorRequired]
        public HttpClient? Http { get; set; } = default!;
    
        [Parameter, EditorRequired]
        public List<HeroImage> HeroImages { get; set; } = default!;
    
        protected override async Task OnInitializedAsync()
        {
            aboutMe = await Http.GetFromJsonAsync<AboutMe>("sample-data/aboutme.json");
            heroAboutMe = HeroImages.SingleOrDefault(h => h.Name == "about");
        }
    }
    ```

### `PortfolioComponent` component

1. Open `MyPortfolio/Components/PortfolioComponent.razor`
1. Remove the `@inject HttpClient Http` directive from the top of the file so that it will be:

    ```csharp
    @using System.Net.Http.Json
    @using MyPortfolio.Models

    <section class="light" id="portfolio">
    ```

1. Replace the code block with the following code. As you can see both `HttpClient` and `HeroImages` are now parameters and accept values from their parent component, `Home`.

    ```csharp
    @code {
        private List<Project>? projects;
        private HeroImage? heroPortfolio;
        
        [Parameter, EditorRequired]
        public HttpClient? Http { get; set; } = default!;
    
        [Parameter, EditorRequired]
        public List<HeroImage> HeroImages { get; set; } = default!;
    
        protected override async Task OnInitializedAsync()
        {
            projects = await Http.GetFromJsonAsync<List<Project>>("sample-data/projects.json");
            heroPortfolio = HeroImages.SingleOrDefault(h => h.Name == "portfolio");
        }
    }
    ```

### `FooterComponent` component

1. Open `MyPortfolio/Components/FooterComponent.razor`
1. Remove the `@inject HttpClient Http` directive from the top of the file so that it will be:

    ```csharp
    @using System.Net.Http.Json
    @using MyPortfolio.Models

    <div id="contact" style="background-color: #4E567E;">
    ```

1. Replace the code block with the following code. As you can see `HttpClient` is now parameters and accept values from their parent component, `Home`.

    ```csharp
    @code {
        private SiteProperties? property;
        private SocialIcons? icons;
    
        [Parameter, EditorRequired]
        public HttpClient? Http { get; set; } = default!;
    
        protected override async Task OnInitializedAsync()
        {
            property = await Http.GetFromJsonAsync<SiteProperties>("sample-data/siteproperties.json");
    
            icons = await Http.GetFromJsonAsync<SocialIcons>("sample-data/socialicons.json");
        }
    }
    ```

1. Let's make the footer component even more interactive by adding another parameter. Add the following parameter just under the `Http` parameter.

    ```csharp
    public HttpClient? Http { get; set; } = default!;

    // ⬇️⬇️⬇️ Add codes below ⬇️⬇️⬇️

    [Parameter, EditorRequired]
    public string? BackgroundColor { get; set; } = default!;
    ```

1. Then, change the `div` tag to use the `BackgroundColor` parameter.

    ```csharp
    @using MyPortfolio.Models

    @* ⬇️⬇️⬇️ Remove codes below ⬇️⬇️⬇️ *@
    <div id="contact" style="background-color: #4E567E;">

    @* ⬇️⬇️⬇️ Add codes below ⬇️⬇️⬇️ *@
    <div id="contact" style="background-color: @BackgroundColor;">
    ```

### `Home` component

Now, each component has been parameterised. We need to pass the data from the `Home` component to each component.

1. Open `MyPortfolio/Pages/Home.razor` and replace the content with the following code. As you can see, we instantiate the `HttpClient` and `HeroImages` only once and pass them to each component.

    ```csharp
    @page "/"
    @using System.Net.Http.Json
    @using MyPortfolio.Components
    @using MyPortfolio.Models
    @inject HttpClient Http
    
    <PageTitle>My Portfolio</PageTitle>
    
    @if (heroImages is null)
    {
        <p><em>Loading...</em></p>
    }
    else
    {
        <HeaderComponent />
        <HeroComponent Http="@Http" HeroImages="@heroImages" />
        <AboutMeComponent Http="@Http" HeroImages="@heroImages" />
        <PortfolioComponent Http="@Http" HeroImages="@heroImages" />
        <FooterComponent Http="@Http" BackgroundColor="#4E567E" />
    }
    
    @code {
        private List<HeroImage>? heroImages;
    
        protected override async Task OnInitializedAsync()
        {
            heroImages = await Http.GetFromJsonAsync<List<HeroImage>>("sample-data/heroimages.json");
        }
    }
    ```

1. You can even do further refactoring by passing `SiteProperties`, `AboutMe`, `List<Project>` and `SocialIcons` to each component. I'll leave it to you as an exercise.

## Step 9: Build and run the Blazor WebAssembly application

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

    ![Parameterised page](./images/06-split-components-02.png)

1. Stop the running application by pressing `Ctrl + C` in the terminal.

---

Congratulations! You've successfully split the `Home` component into individual components and parameterised them. You've also learned how to pass data from the parent component to the child component. In the next step, you'll learn how to deploy the Blazor WebAssembly application to GitHub Pages using GitHub Actions.

:point_right: [Step 7: Deploy your Blazor application to GitHub Pages](./07-deploy-gh-pages.md)
