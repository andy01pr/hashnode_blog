# How to deep clone objects in C#

There are times when you want to clone an object in C# but it's tedious to copy manually all the properties. This solution uses JSON serialization to clone objects. Below is the code using the `Newtonsoft.Json` NuGet package which can be used in any .NET version including .NET Framework and the second one is using the `System.Text.Json` which is already included in .NET 5 or later.

```csharp
public static class ObjectExtensions
    {
        public static T CloneJson<T>(this T source)
        {
            if(ReferenceEquals(source, null))
            {
                return default;
            }

            var deserializeSettings = new JsonSerializerSettings
            {
                ObjectCreationHandling = ObjectCreationHandling.Replace
            };

            return JsonConvert.DeserializeObject<T>(JsonConvert.SerializeObject(source), deserializeSettings);
        }
    }
```

```csharp
public static class ObjectExtensions
    {
        public static T CloneJson<T>(this T source)
        {
            if(ReferenceEquals(source, null))
            {
                return default;
            }

            var deserializeSettings = new JsonSerializerOptions
            {
                IncludeFields = true,
                PropertyNameCaseInsensitive = true
            };

            return JsonSerializer.Deserialize<T>(JsonSerializer.Serialize(source), deserializeSettings);
        }
    }
```

> ***Note****: Private members are not cloned using this JSON method.*

# **Resources**

[Deep cloning objects](https://stackoverflow.com/questions/78536/deep-cloning-objects)

[How to serialize and deserialize (marshal and unmarshal) JSON in .NET](https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/how-to?pivots=dotnet-6-0)