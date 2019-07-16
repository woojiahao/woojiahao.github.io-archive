---
published: true
title: "Using delegated properties in Kotlin"
---

## Short announcement!
After a long hiatus from kMD2PDF, I revved up my engines and began to work on my latest planned feature for the 
project...

> YAML support!

Yes, now YAML is in the works and you can now control basic attributes of your exported document using only 
[front matter YAML.](https://jekyllrb.com/docs/front-matter/)

This is an incredibly big milestone as this allows anyone to quickly customise their document without having to write a
single line of Kotlin, and with another planned release to create a GUI exporter for markdown documents, this would 
greatly streamline users' experience.

## Okay, what are delegate properties
kMD2PDF introduces a theming engine where you are able to change the entire color scheme of an exported document just by
specifying an attribute in code as such

```kotlin
val converter = markdownConverter {
  // ...

  settings {
    theme = Settings.Theme.DARK
  }  
}
```

### Design considerations
Before building this system, we have to make several consideration

1. How do we specify that certain attributes have different configurations depending on the theme?
2. How do we allow changes to the theme propagate to each element such that the changes are reflected during the 
   document creation?

#### Using singletons
The second inquiry is rather easy to answer, so we will tackle that issue first - we can store the `Settings` as 
[singleton](https://en.wikipedia.org/wiki/Singleton_pattern), since the state of a singleton is unique and static, as 
long as the settings are configured before the document is created, then the settings will be chosen during runtime and 
have the exported document reflect the changes.

```kotlin
object Settings {
  var font = FontFamily("sans-serif")
    get() = field.clone()
  // ...
}

inline fun settings(configuration: Settings.() -> Unit) = Settings.apply(configuration)
```

Kotlin comes with a language construct to create singletons easily - `object` keyword (read more 
[here](https://kotlinlang.org/docs/reference/object-declarations.html#object-declarations)).

This creates everything we need in a singleton but reduces all the boiler plate that would be involved with creating 
the singleton, unlike other languages like [Java.](https://www.geeksforgeeks.org/singleton-class-java/)

When we update the `Settings` singleton, the changes will reflect with the exported document since the document style is
lazily generated only up till the latest minute before it gets exported.

#### Multi-value properties
Now to tackle the hard question, how do we store multiple values in Kotlin? Ideally, what we would want with this system
is to have a single variable and have that variable store both the dark theme and light theme setting. That is, if we 
had a single variable `textColor`, we would want to be able to store both the light theme and dark theme settings inside 
this variable. Depending on the current `Settings.theme` configuration at the time where the stylesheet has to be 
generated, the `textColor` variable would return either the configuration for `DARK` theme or `LIGHT` theme.

Traditionally, we would approach this issue using a class to store the information and have that be the end of things - 
and this was indeed the approach I ended up employing due to certain limitations in that design which I'll discuss.

However, in Kotlin, there exists a language construct called 
[delegated properties](https://kotlinlang.org/docs/reference/delegated-properties.html) where you are able to call an 
object constructor to initialise a variable and provide it with a base of data. Subsequent times accessing this variable
masks the object constructor and will only allow you to access the data type specified by the delegated property. That 
is, a delegated property is akin to a variable with 2 return types.

```kotlin

```