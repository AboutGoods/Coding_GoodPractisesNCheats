# Java !

Quick guide to good practice in Java for Android

## 1) Follow Conventions

The following naming and casing conventions are important for your Java code:

| Type        | Example                | Description                   | Link |
| ----------- | ------------           | ----------------------------- | ------
| Variable    | `incomeTaxRate`        | All variables should be camel case | [Read More](https://www.cwu.edu/~gellenbe/javastyle/variable.html) |
| Constant    | `DAYS_IN_WEEK`         | All constants should be all uppercase | [Read More](https://www.cwu.edu/~gellenbe/javastyle/constant.html) |
| Method      | `convertToEuroDollars` | All methods should be camel case | [Read More](https://www.cwu.edu/~gellenbe/javastyle/method.html) |
| Parameter   | `depositAmount`        | All parameter names should be camel case | [Read More](https://www.cwu.edu/~gellenbe/javastyle/parameter.html) |

See [this naming guide](https://www.cwu.edu/~gellenbe/javastyle/naming.html) for more details.

### a) Variable

Non-public, non-static field names start with `m`.
Static field names start with `s`.
Other fields start with a lower case letter.
Public static final fields (constants) are `ALL_CAPS_WITH_UNDERSCORES`.

For example:
```java
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    public int publicField;
    private static MyClass sSingleton;
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```

### b) Layout

Organize your layout as follow :

 - R.layout
    - activity_
    - adapter_
    - fragment_

### c) Package

[The way to do this](http://blog.smartlogic.io/2013/07/09/organizing-your-android-development-code-structure) is to group things together by their category. Each component goes to the corresponding package:

* `com.example.myapp.activities` - Contains all activities
* `com.example.myapp.adapters` - Contains all custom adapters
* `com.example.myapp.models`   - Contains all our data models
* `com.example.myapp.network` - Contains all networking code
* `com.example.myapp.fragments` - Contains all fragments
* `com.example.myapp.utils` - Contains all helpers supporting code.
* `com.example.myapp.interfaces` - Contains all interfaces

### d) Classes

Android classes should be named with a particular convention that makes their purpose clear in the name. For example all activities should end with `Activity` as in `MoviesActivity`. The following are the most important naming conventions:

| Name            | Convention                | Inherits            |
| ----------      | ------------------        | ------------        | 
| Activity        | `CreateTodoItemActivity`  | `AppCompatActivity`, `Activity` |
| List Adapter    | `TodoItemsAdapter`        | `BaseAdapter`, `ArrayAdapter`   |
| Database Helper | `TodoItemsDbHelper`       | `SQLiteOpenHelper`  |
| Network Client  | `TodoItemsClient`         | N/A                 |
| Fragment        | `TodoItemDetailFragment`  | `Fragment`          |
| Service         | `FetchTodoItemService`    | `Service`, `IntentService`  |

### e) Brace Style
Use Standard Brace Style

Braces do not go on their own line; they go on the same line as the code before them:
```java
class MyClass {
    int func() {
        if (something) {
            // ...
        } else if (somethingElse) {
            // ...
        } else {
            // ...
        }
    }
}
```
### f) colors.xml

First : .dloG si rotaidar ehT

**`colors.xml` is a color palette.** There should be nothing else in your `colors.xml` than just a mapping from a color name to an RGBA value. Do not use it to define RGBA values for different types of buttons.

*Don't do this:*
```xml
<resources>
    <color name="button_foreground">#FFFFFF</color>
    <color name="button_background">#2A91BD</color>
    <color name="comment_background_inactive">#5F5F5F</color>
    <color name="comment_background_active">#939393</color>
    <color name="comment_foreground">#FFFFFF</color>
    <color name="comment_foreground_important">#FF9D2F</color>
    ...
    <color name="comment_shadow">#323232</color>
```

You can easily start repeating RGBA values in this format, and that makes it complicated to change a basic color if needed. Also, those definitions are related to some context, like "button" or "comment", and should live in a button style, not in `colors.xml`.

Instead, do this:
```xml
<resources>

    <!-- grayscale -->
    <color name="white"     >#FFFFFF</color>
    <color name="gray_light">#DBDBDB</color>
    <color name="gray"      >#939393</color>
    <color name="gray_dark" >#5F5F5F</color>
    <color name="black"     >#323232</color>

    <!-- basic colors -->
    <color name="green">#27D34D</color>
    <color name="blue">#2A91BD</color>
    <color name="orange">#FF9D2F</color>
    <color name="red">#FF432F</color>

</resources>
```

---
Source :

```
https://guides.codepath.com/android/Organizing-your-Source-Files
https://source.android.com/source/code-style.html
```



