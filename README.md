# template-blazor

Template and instructions for creating a web app using Blazor.

## Instructions

1. Create a repo using kebab-case
2. Add a `.gitignore`, `README.md`, `src` folder, and optionally a `LICENSE` at the root
3. Create the solution:
   ```bash
   dotnet new sln --name PascalCase
   ```
4. Create the API project:
   ```bash
   dotnet new webapi --exclude-launch-settings --output src/PascalCase.Api/
   ```
5. Install MudBlazor templates:
   ```bash
   dotnet new install MudBlazor.Templates
   ```
6. Create the frontend (Blazor WASM) project:
   ```bash
   dotnet new mudblazorwasm --empty --exclude-launch-settings --output src/PascalCase.Web/
   ```
7. Add projects to the solution:
   ```bash
   dotnet sln add src/PascalCase.Api/PascalCase.Api.csproj
   
   dotnet sln add src/PascalCase.Web/PascalCase.Web.csproj
   ```
8. Use the .vscode config in this repository to have an easy way to launch the project
9. Add a common class library:
   ```bash
   dotnet new classlib --output src/PascalCase.Shared
   
   dotnet sln add src/PascalCase.Shared/PascalCase.Shared.csproj
   
   dotnet add src/PascalCase.Api/PascalCase.Api.csproj reference src/PascalCase.Shared/PascalCase.Shared.csproj
   
   dotnet add src/PascalCase.Web/PascalCase.Web.csproj reference src/PascalCase.Shared.csproj
   ```
10. Add `Entity Framework Core` as a `NuGet` dependency
   ```bash
   dotnet add src/PascalCase.Api/PascalCase.Api.csproj package Microsoft.EntityFrameworkCore.SqlServer | Sqlite | PostgreSQL
   ```