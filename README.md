# .NET Core - Adding static assets bundling using Parcel

Built using the Razor pages template with [.NET Core SDK](https://dotnet.microsoft.com/download) version: 2.2.300 using the CLI:
```
dotnet new razor
```

Other requirements: 
* Node.JS
* npm package parcel-bundler installed globally

## npm scripts
To use in development, run:
```
npm run dev
```

To package your app for production:
```
npm run build
```
Will output a distributable build to \bin\Release\netcoreapp2.2\publish.

As we almost never use IIS-based hosting we edited the csproj to use OutOfProcess hosting model as in:
```xml
<AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
```

There is a special script to build a standalone package for a Linux target (doesn't need .NET Core installed to work):
```
npm run build-linux
```
Build will be in \bin\Release\netcoreapp2.2\debian-x64.

## How we enabled static assets bundling
- Initialize a package.json file with `npm init`
- Install concurrently: `npm install -D concurrently`
- Create the wwwroot/assets folder
- Create the src folder at the root with the main entry point for the bundler (see project structure)
- Add the npm scripts (see package.json)
- Edit Pages/Shared/_Layout.cshtml to include the right assets, as in:
All the `<environment>` tags can be removed. We just need a CSS include in head:
```html
<link rel="stylesheet" asp-append-version="true" href="~/assets/main.css" />
```
And the script include at the end of body:
```html
<script src="~/assets/main.js" asp-append-version="true"></script>
```
- Edit the project .csproj file and make sure we include the wwwroot folder in the app:
```xml
<ItemGroup>
  <Folder Include="wwwroot\" />
</ItemGroup>
```
- Add a .gitignore file (see the model included)
- wwwroot/js, wwwroot/css and wwwroot/lib can be safely removed since we're not using them anymore
- Add Bootstrap using npm (see next section) and build your entry point, reference the CSS in it

## Adding Bootstrap
To keep the base style as it is and for the exercise, we added Bootstrap and JQuery to this example.

```
npm install -D bootstrap jquery popper.js
```

# TODO
- [x] Put .vscode in gitignore as well.
- [x] Keep Bootstrap but add it through npm - Use SASS for the CSS.
- [x] Publishing was not configured - Add the static folder in what's to be published.
- [x] Remove the unused JS libs that are statically downloaded.
- [x] Test that the source maps are working: we probably need --public-url.
- [x] Add Babel with preset env. -> Parcel compiles with Babel out of the box.
- [x] Explain how to run and publish the app (same as for the Spring Boot project).