# Jinja2 Quickstart

Jinja2 is a templating engine used to dynamically generate scripted content. This has its application in the creation of the MIPCV website generation. 

## Example
Some example uses of the things possible with JINJA are shown below. 

### Inline HTML
Escape HTML code in literals with `|safe`, e.g., `{{ item.description |safe }}`.

### Batches
Process items in batches using the `batch` filter.

```jinja2
{% for group in changes|batch(2) %}
    <!-- Make a row -->
    {% for item in group %}
        <!-- Process every two items -->
    {% endfor %}
{% endfor %}
```

### Dictionary
Iterate through dictionary items.

```jinja2
{% for key, value in my_dict.items() %}
  Key: {{ key }}<br>
  Value: {{ value }}<br><br>
{% endfor %}
```

### Conditional Statements
Use `if` statements for conditional rendering.

```jinja2
{% if condition %}
    <!-- Content to display if condition is true -->
{% else %}
    <!-- Content to display if condition is false -->
{% endif %}
```

### Macros
Define reusable code snippets with macros.

```jinja2
{% macro my_macro(arg) %}
    <!-- Reusable code here using {{ arg }} -->
{% endmacro %}
```

### Filters
Apply filters for formatting and transformations.

```jinja2
{{ variable|filter_name }}
```

### Loops
Use `for` loops for iteration.

```jinja2
{% for item in items %}
    <!-- Process each item -->
{% endfor %}
```
