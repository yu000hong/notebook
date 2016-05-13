# Generate xml from map

We showed some basics on [how to create xml](http://www.leveluplunch.com/groovy/examples/creating-xml-with-markupbuilder/) 
and more [advanced features to generate xml](http://www.leveluplunch.com/groovy/examples/building-xml-with-stream-markup-builder/)
that included escaping text. This example shows how to generate xml from a map using Groovy's 
[MarkupBuilder](http://groovy.codehaus.org/api/groovy/xml/MarkupBuilder.html).

### Code

```java
@Test
public void create_xml_from_map() {

    def writer = new StringWriter()
    def builder = new groovy.xml.MarkupBuilder(writer)
    def myMap = ["author": "Eric T. Freeman, Elisabeth Robson",
                 "title": "Head First JavaScript Programming",
                 "price": "42.99",
                 "description": "This brain-friendly guide teaches you everything from JavaScript language fundamentals to advanced topics."]

    builder.books {
        book() {
            myMap.each() { key, value ->
                "${key}" "${value}"
            }
        }
    }
    println writer.toString()
}
```

### Output

```xml
<books>
  <book>
    <author>Eric T. Freeman, Elisabeth Robson</author>
    <title>Head First JavaScript Programming</title>
    <price>42.99</price>
    <description>This brain-friendly guide teaches you everything from JavaScript language fundamentals to advanced topics.</description>
  </book>
</books>
```

# 链接

[Generate xml from map](http://www.leveluplunch.com/groovy/examples/build-xml-from-map-with-markupbuilder/)
