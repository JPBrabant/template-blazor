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

# General documentation

## Handling connection to Azure resources

Install `Az` and do an `az login` from the command line and/or install the `Azure resources` extension for VS Code and login from there. It will generate a token that will be automaticaly used by most Microsoft services and API. Like connecting to a database with `Authentication=Active Directory Default` as a connection string arg. It is important to close and reopen VS Code after connecting to Azure.

## Allowing an API to talk to a database

- Networking in SQL Server
   1. Go to your SQL Server resource (not the database)
   2. Go to Security → Networking (left sidebar)
   3. Under Firewall rules, toggle "Allow Azure services and resources to access this server" to Yes
   4. Click Save
- Enable Managed Identity on your App Service
   1. In the portal, go to your App Service
   2. Settings → Identity (left sidebar)
   3. Under System assigned, toggle to On → Save
- Grant that identity access to SQL
   ```sql
   CREATE USER [<app-name>] FROM EXTERNAL PROVIDER;
   ALTER ROLE db_datareader ADD MEMBER [<app-name>];
   ALTER ROLE db_datawriter ADD MEMBER [<app-name>];
   ```

## Enabling CI/CD from GitHub to Azure App Service

1. **Get your IDs** — `az account show --query "{subscriptionId, tenantId}"`
2. **Create app registration** — `az ad app create --display-name "my-app-oidc"`
3. **Create service principal** — `az ad sp create --id <appId from step 2>`
4. **Add federated credential** — link GitHub repo + branch to the app: `az ad app federated-credential create --id <appId> --parameters '{...issuer, subject, audiences...}'`
5. **Grant Contributor role** — `az role assignment create --assignee <appId> --role Contributor --scope /subscriptions/.../resourceGroups/...`
6. **Add 3 GitHub secrets** — `AZURE_CLIENT_ID`, `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID` (manually in repo settings)
7. **Add to workflow** — `azure/login@v2` with OIDC params + `permissions: id-token: write` at the job level