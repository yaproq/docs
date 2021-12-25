## Usage

### Comments

```html
{# A single-line comment #}
{#
    A
    multi-line
    comment
#}
```

### Variables

```html
{% var int = 1 %}
{% var float = 2.0 %}
{% var string = "some text" %}
{% var booleanTrue = true %}
{% var booleanFalse = false %}
{% var range = 0..<3 %}
{% var closedRange = 1...4 %}
{% var item0 = array[0] %}
{% var value = dictionary[key] %}
```

### Math expressions

```
{% var five = 5.0 %}
{% var four = 4 %}
{% var three = 3 %}
{% var two = 2.0 %}
{% var seven = 7.0 %}
{% var six = 6 %}
{% var one = 1.0 %}
{% var result = five * four / (three + two) - seven % six ^ one %}
{{ result }}
```

### Control structures

#### If, elseif, and else

```
{% var number = 1 %}

{% if number == 0 %}
    Equal to 0
{% elseif number >= 1 %}
    Greater than or equal to 1
{% elseif number > 2 %}
    Greater than 2
{% elseif number <= 3 %}
    Less than or equal to 3
{% elseif number < 4 %}
    Less than 4
{% else %}
    Greater than or equal to 4
{% endif %}
```

#### For loop

```
{% for item in array %}
    {{ item }}
{% endfor %}
```

```
{% for index, item in array %}
    {{ index }}, {{ item }}
{% endfor %}
```

```
{% for key, value in dictionary %}
    {{ key }}, {{ value }}
{% endfor %}
```

```
{% for number in 0..<3 %}
    {{ number }}
{% endfor %}
```

```
{% for number in 1...4 %}
    {{ number }}
{% endfor %}
```

#### While loop

```
{% var number = 0 %}
{% var maxNumber = 3 %}

{% while number < maxNumber %}
    {{ number }}
    {% number += 1 %}
{% endwhile %}
```

#### Blocks

```html
<!doctype html>
<html lang="en">
    <head>
        <title>{% block title %}{% endblock %}</title>
    </head>
    <body>
        {% block body %}
            {% block header %}{% endblock %}
            <div class="container">
                {% block content %}{% endblock %}
            </div>
            {% block footer %}{% endblock %}
        {% endblock %}
    </body>
</html>
```

### Including other templates

```html
{% block body %}
    {% include "header.html" %}
    <div class="container">
        {% block content %}{% endblock %}
    </div>
    {% include "footer.html" %}
{% endblock %}
```

### Template inheritance

#### /templates/base.html

```html
<!doctype html>
<html lang="en">
    <head>
        <title>{% block title %}{{ title }}{% endblock %}</title>
        {% block css %}
            <link href="/public/css/base.min.css" rel="stylesheet" />
        {% endblock %}
    </head>
    <body>
        {% block body %}{% endblock %}
        {% block js %}
            <script src="/public/js/base.min.js"></script>
        {% endblock %}
    </body>
</html>
```

#### /templates/posts.html

```html
{% extend "base.html" %}

{% block css %}
    {% super %}
    <link href="/public/css/posts.min.css" rel="stylesheet" />
{% endblock %}

{% block js %}
    {% super %}
    <script src="/public/js/posts.min.js"></script>
{% endblock %}

{% block body %}
    <h2>All posts</h2>
    {% for post in posts %}
        <p>{{ post.title }}</p>
    {% endfor %}
{% endblock %}
```

```swift
import Yaproq

struct Post: Encodable {
    var title: String
}

let templating = Yaproq(configuration: .init(directories: Set(arrayLiteral: "/templates")))

do {
    let templateName = "posts.html"
    let context: [String: Encodable] = [
        "title": "Posts",
        "posts": [
            Post(title: "Post 1"),
            Post(title: "Post 2"),
            Post(title: "Post 2")
        ]
    ]
    let result = try templating.renderTemplate(named: templateName, in: context)
    print(result)
} catch {
    print(error)
}
```

### Custom delimiters

```swift
import Yaproq

do {
    let templating = Yaproq(
        configuration: try .init(
            delimiters: [
                .comment("{^", "^}"),
                .output("{$", "$}"),
                .statement("{@", "@}")
            ]
        )
    )
} catch {
    print(error)
}
```

### Loading templates

#### Name

```swift
import Yaproq

let templating = Yaproq(configuration: .init(directories: Set(arrayLiteral: "/templates")))

do {
    let templateName = "base.html"
    let template = try templating.loadTemplate(named: templateName)
    print(template)
} catch {
    print(error)
}
```

#### Path

```swift
import Yaproq

let templating = Yaproq()

do {
    let templatePath = "/templates/base.html"
    let template = try templating.loadTemplate(at: templatePath)
    print(template)
} catch {
    print(error)
}
```

### Rendering templates

#### Name

```swift
import Yaproq

let templating = Yaproq(configuration: .init(directories: Set(arrayLiteral: "/templates")))

do {
    let templateName = "base.html"
    let context: [String: Encodable] = ["title": "My Blog"]
    let result = try templating.renderTemplate(named: templateName, in: context)
    print(result)
} catch {
    print(error)
}
```

#### Path

```swift
import Yaproq

let templating = Yaproq()

do {
    let templatePath = "/templates/base.html"
    let context: [String: Encodable] = ["title": "My Blog"]
    let result = try templating.renderTemplate(at: templatePath, in: context)
    print(result)
} catch {
    print(error)
}
```

#### Template

```swift
import Yaproq

let templating = Yaproq()

do {
    let templatePath = "/templates/base.html"
    let template = try templating.loadTemplate(at: templatePath)
    let context: [String: Encodable] = ["title": "My Blog"]
    let result = try templating.renderTemplate(template, in: context)
    print(result)
} catch {
    print(error)
}
```

### Error handling

```swift
import Yaproq

let templating = Yaproq()

do {
    let templatePath = "/templates/base.html"
    let context: [String: Encodable] = ["title": "My Blog"]
    let result = try templating.renderTemplate(at: templatePath, in: context)
    print(result)
} catch {
    if let error = error as? YaproqError { // YaproqError occurs when there is a problem with a custom configuration provided, for example, due to invalid or non-unique delimiters provided for each delimiter type.
        print(error)
    } else if let error = error as? TemplateError { // TemplateError occurs when there is a problem with a template file itself, for example, the templating engine can't find and load it.
        print(error)
    } else if let error = error as? SyntaxError { // SyntaxError occurs when an existing feature is used incorrectly and can't be parsed.
        print(error)
    } else if let error = error as? RuntimeError { // RuntimeError occurs when the templating engine can't interpret an expression or statement because an output is semantically incorrect.
        print(error)
    } else {
        print("Unknown error: \(error)")
    }
}
```
