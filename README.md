# template-blazor
Template and instructions for creating a web app using blazor

## Instructions

1. Create a repo using kebab-case
2. Add a gitignore, a readme, an src/ folder and optionaly a licence at the root
3. `dotnet new sln --name PascalCase`
4. `dotnet new webapi --exclude-launch-settings --output src/PascalCase.Api/`
5. `dotnet new install MudBlazor.Templates`
6. `dotnet new mudblazorwasm --empty --exclude-launch-settings --output src/PascalCase.Web/`
7. `dotnet sln add src/NewShowsInTown.Api/NewShowsInTown.Api.csproj`
   `dotnet sln add src/NewShowsInTown.Web/NewShowsInTown.Web.csproj`