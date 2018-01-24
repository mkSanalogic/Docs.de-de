<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="11213-101">Aufbauen des Filmmodells</span><span class="sxs-lookup"><span data-stu-id="11213-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="11213-102">Führen Sie über die Befehlszeile im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs* und *CSPROJ*) folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="11213-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="11213-103">Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="11213-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="11213-104">Öffnen Sie eine Befehlsshell im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs* und *CSPROJ*-Dateien).</span><span class="sxs-lookup"><span data-stu-id="11213-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>