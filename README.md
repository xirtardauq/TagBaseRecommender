# Tag Based Recommender
*Tag Based Recommendder* is a simple tag-based recommendation library for Sitecore

**Warning: This software is in early stage of development.**

## Installation
No releases yet. Clone this repository and build it locally.

## Usage
1. Add `Tags` field to a page template.
1. (Optional) Set the template's ID to `TagBasedRecommender.SearchTemplate` settings.
```xml
<configuration xmlns:patch="http://www.sitecore.net/xmlconfig/">
  <sitecore>
    <settings>
      <setting name="TagBasedRecommender.SearchTemplate"></setting>
        <patch:attribute name="value">{The template's ID here}</patch:attribute>
      </setting>
    </settings>
  </sitecore>
 </configuration>
```
3. Now you can get recommendation to use `IRecommendationService.GetRecommendations` method.

```csharp
using TagBasedRecommender.Services;

public class RecommendExample
{
    public RecommendExample(IRecommendationService service)
    {
        var recommendations = service.GetRecommendations(recommendCount: 10);
    }
}
```

## Settings
|Name|Type|Description|Default|
|:-|:-|:-|:-|
|`TagBasedRecommender.SearchField`|`string`|An index field name for search by tags.|`_content`|
|`TagBasedRecommender.SearchTemplate`|`ID`|A template ID to use filtering recommendation. |empty (All templates)|
|`TagBasedRecommender.StoredItemCount`|`int`|A length of items stored in a cookie value.|`20`|
|`TagBasedRecommender.BoostMultiplicand`|`float`|A value to be added to the boost due to a tag match.|`1`|
|`TagBasedRecommender.Cookie.Name`|`string`|A cookie's name (required).|`TagBasedrec_items`|
|`TagBasedRecommender.Cookie.Lifespan`|`int`|A cookie's lifespan to set to `Expire` attribute (in days).|`30`|
|`TagBasedRecommender.Cookie.Domain`|`string`|A cookie's `Domain` attribute.|empty|
|`TagBasedRecommender.Cookie.Path`|`string`|A cookies's `Path` attribute.|`/`|
|`TagBasedRecommender.Cookie.Secure`|`bool`|A cookie's `Secure` Attribute.|`true`|
|`TagBasedRecommender.Cookie.HttpOnly`|`bool`|A cookie's `HttpOnly` attribute.|`true`|


## Customization
### Custom Tags
This library uses item's `Tags` field to get recommendation tags (splited at whitespace). This behaviour can be customized by replacing `DefaultItemTagsResolver`.

```csharp
using TagBasedRecommender.Services;

public class CategoryTagsResolver : IItemTagsResolver
{
    public List<string> GetTags(Item item)
    {
        // Default
        // return item["Tags"].Split(Array.Empty<char>(), StringSplitOptions.RemoveEmptyEntries).ToList();

        // Use categories's name instead.
        var tags = (MultilistField)item.Fields["Categories"];
        return tags.GetItems().Select(tag => tag["Name"]).ToList();
    }
}
```

And replace the default class by apply the following configuration (This is just an example).

```xml
<configuration xmlns:patch="http://www.sitecore.net/xmlconfig/">
  <sitecore>
    <services>
      <register serviceType="TagBasedRecommender.Services.IItemTagsResolver, TagBasedRecommender">
        <patch:attribute name="implementationType">Namespace.To.CategoryTagsResolver, AssemblyName</patch:attribute>
      </register>
    </services>
  </sitecore>
</configuration>
```

### Custom Filter
WIP

## Author
- Takumi Yamada (xirtardauq@gmail.com)

## License
*Tag Based Recommender* is licensed unther the MIT license. See LICENSE.txt.