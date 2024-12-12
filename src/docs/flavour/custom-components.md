---
title: Custom components
---

# Creating a first component

Let's create a simple component that does nothing more than display "Hello, world!".

(Before starting, make sure the `teavm-flavour-widgets` artifact is in your project's dependencies.)

First, create a new class called `HelloComponent`

```java
package example.components;

import org.teavm.flavour.templates.Slot;
import org.teavm.flavour.widgets.AbstractWidget;

@BindTemplate("components/hello.html")
@BindElement(name = "say-hello")
public class HelloComponent extends AbstractWidget {
    public HelloComponent(Slot slot) {
        super(slot);
    }
}
```

Second, create a template file called `components/hello.html`:

```html
<div>Hello, world!</div>
```

Third, create a resource file called `META-INF/flavour/component-packages/example.components`:

```
HelloComponent
```

Now you should be able to use this component in your templates:

```html
<?use hello:example.components?>
<div>Custom component follows</div>
<hello:say-hello/>
```


# Adding parameters

The component we just created does not do anything useful.
The Flavour component system has powerful features to bind data to components.
Let's start using this ability by introducing parameters to our component.

First, add the following method to `HelloComponent`:

```java
    private Supplier<String> nameSupplier;

    /*
     * Tell template engine that we want our component to support 'name' attribute.
     */
    @BindAttribute(name = "name")
    public void setNameSupplier(Supplier<String> nameSupplier) {
        this.nameSupplier = nameSupplier;
    }
    
    /*
     * Expose name to `components/hello.html`.
     */
    public String getName() {
        return nameSupplier.get();
    }
```

Second, modify the template as follows:

```html
<div>Hello, <html:text value="name"/></div>
```

Now you should be able to pass a name parameter to the component like this:

```html
<?use hello:example.components?>
<div>Custom component follows</div>
<hello:say-hello name="'anonymous'"/>
```


# Adding body

We can go even further and allow our component to take an HTML fragment as a parameter.
Add the following code to `HelloComponent`:

```java
    private Fragment body;

    @BindContent
    public void setBody(Fragment body) {
        this.body = body;
    }

    public Fragment getBody() {
        return body;
    }
```

Update your template:

```html
<div>Hello, <html:text value="name"/>!</div>
<div class="hello-footer">
  <std:insert fragment="body"/>
</div>
```

And use the component as follows:

```html
<hello:say-hello name="'anonymous'">
  <em>Are you OK?</em>
</hello:say-hello>
```


# Creating an attribute component

Now we know how to create a new element component.
Remember, Flavour allows you to bind components to attributes too.
Let's create one!

First, create the Java class:

```java
import org.teavm.flavour.templates.BindAttributeComponent;
import org.teavm.flavour.templates.BindContent;
import org.teavm.flavour.templates.ModifierTarget;
import org.teavm.flavour.templates.Renderable;

import java.util.function.Supplier;

@BindAttributeComponent(name = "hello-class")
public class HelloAttributeComponent implements Renderable {
    private ModifierTarget target;
    private Supplier<String> nameSupplier;

    public HelloAttributeComponent(ModifierTarget target) {
        this.target = target;
    }

    @BindContent
    public void setNameSupplier(Supplier<String> nameSupplier) {
        this.nameSupplier = nameSupplier;
    }

    @Override
    public void render() {
        target.getElement().setAttribute("class", "hello-" + nameSupplier.get());
    }

    @Override
    public void destroy() {
    }
}
```

And update the package catalog file:

```
HelloComponent
HelloAttributeComponent
```

You should be able to use the newly created component like this:

```html
<?use hello:example.components?>
<div hello:hello-class="'anonymous'">
  Nothing useful here
</div>
```
