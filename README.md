# dotnet-core-razor-blazor-azure

### Framework & Tools
- .NET Core 3.1
- Visual Studio 2019
- C#
- Razor
- Blazor
- Web API
- JSON
- Azure

### Project Structure
- **wwwroot > data > products.json (Database)**
- **Components > ProductList.razor (Blazor Component)**
- **Controllers > ProductsController.cs (API Controller)**
- **Models > Product.cs (Object Model)**
- **Services > JSONFileProductService.cs (Data Service)**

### [Startup.cs](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/startup?view=aspnetcore-5.0)
```
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages();
    services.AddServerSideBlazor();
    services.AddControllers();
    services.AddTransient<JSONFileProductService>();
}
```
- **Adds services for Razor pages to the specified IServiceCollection. [AddRazorPages(), AddControllers(), AddControllersWithViews()](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addrazorpages?view=aspnetcore-5.0)**
- **Adds a transient service of the type specified in serviceType to the specified IServiceCollection. [AddTransient()](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addtransient?view=dotnet-plat-ext-5.0), [ServiceLifetime](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.servicelifetime?view=dotnet-plat-ext-5.0#Microsoft_Extensions_DependencyInjection_ServiceLifetime_Transient)**
- **Adds Server-Side Blazor services to the service collection. [AddServerSideBlazor()](https://docs.microsoft.com/en-us/dotnet/api/microsoft.extensions.dependencyinjection.componentservicecollectionextensions.addserversideblazor?view=aspnetcore-5.0)**
```
app.UseEndpoints(endpoints =>
{
    endpoints.MapRazorPages();
    //endpoints.MapGet("/products", (context) =>
    //{
    //    var products = app.ApplicationServices.GetService<JSONFileProductService>().GetProducts();
    //    var json = JsonSerializer.Serialize<IEnumerable<Product>>(products);
    //    return context.Response.WriteAsync(json);
    //});
    endpoints.MapControllers();
    endpoints.MapBlazorHub();
});
```
- **Endpoint Route Binder for Blazor Hub which is a base class for SignalR Hub. [MapBlazorHub()](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.builder.componentendpointroutebuilderextensions.mapblazorhub?view=aspnetcore-5.0)**

### [IWebHostEnvironment Interface](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.hosting.iwebhostenvironment?view=aspnetcore-5.0)
Provides information about the web hosting environment an application is running in.

### [IEnumerable Interface](https://docs.microsoft.com/en-us/dotnet/api/system.collections.ienumerable?view=net-5.0)
Great great grand parent for all iterators be it array, list, collection etc.

### [JsonSerializer](https://docs.microsoft.com/en-us/dotnet/standard/serialization/system-text-json-overview)
Provides functionality to **`Serialize` objects or values to JSON** (JavaScript Object Notation) or to **`Deserialize` JSON to objects or values**

### [JsonPropertyName](https://docs.microsoft.com/en-us/dotnet/api/system.text.json.serialization.jsonpropertynameattribute?view=net-5.0)
Specifies the property name that is present in the JSON when serializing and deserializing.
`**[JsonPropertyName("img")]**`

### Reading & Deserializing JSON File data
```
using (var jsonFileReader = File.OpenText(JsonFileName))
{
    return JsonSerializer.Deserialize<Product[]>(jsonFileReader.ReadToEnd(),
    new JsonSerializerOptions
    {
        PropertyNameCaseInsensitive = true
    });
}
```
### Writing & Serializing JSON File data
```
using (var outputStream = File.OpenWrite(JsonFileName))
{
    JsonSerializer.Serialize<IEnumerable<Product>>(
        new Utf8JsonWriter(outputStream, new JsonWriterOptions
        {
            SkipValidation = true,
            Indented = true
        }),
        products
    );
}
```









