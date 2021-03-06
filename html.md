# HTML guidelines

* **Use consistent class names and markup for HTML components.**

    If possible, use [Twitter Bootstrap](http://twitter.github.com/bootstrap/) conventions, e.g. don’t use `<div class="crumbs"></div>` for breadcrumbs when `<ul class="breadcrumb"></ul>` feels more appropriate.

    This also concerns layout, forms, buttons, flash messages, navigation and others.

* **Don’t use underscores in class names, `id`s and other HTML attributes, like `data-*`.**

    Use dashes instead.

* **Always use double quotes for attributes.**

    Especially in Slim templates.

* **Use `//` instead of `http://` for images, script or iframes**.

    This will make life much easier if someday you decide to use https for site.

* Use anchor (`a` HTML tag element) when click should change page url.

* Use `button type="submit"` if click would submit a form.

* Use `button type="button"` for any other click actions (e.g. next page with ajax loading, toggle view etc).


* **Use [slim](http://slim-lang.com/) wherever possible.**
  * Use `(` & `)` for attribute delimiters
  * Split attributes into multiple lines

    ```slim
    div(
      ng-click="f()"
      ng-class="..."
      some-other-long-attribute="withLongThingHere"
    )

    ```

  * Don't use brackets for one liners

    ```slim
    div ng-click="f()" ng-class"..."
    ```

  * Indentations should look like

    ```slim
    div(
      ng-click="f()"
      ng-class="..."
      some-other-long-attribute="withLongThingHere"
    )
      span.nice ng-bind="hello.world"
    ```

If possible, document new markup proposals in our [styleguide](https://github.com/monterail/boilerplate-rails).
