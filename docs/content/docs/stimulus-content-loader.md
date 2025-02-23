---
title: Content Loader
description: A Stimulus controller to asynchronously load HTML from an url.
package: content-loader
packagePath: "@stimulus-components/content-loader"
---

::alert
If Turbo is activated within your application, you can get the same behavior using a `<turbo-frame>` without the necessity of an additional Stimulus controller.
::

## Video Tutorial

[Dave Kimura](https://twitter.com/kobaltz) from [Drifting Ruby](https://www.driftingruby.com/) has released a presentation video on how to use this package with a real life example.

👉 Take a look: [Deferred Content Loading](https://www.driftingruby.com/episodes/deferred-content-loading)

<Youtube id="kZircHj1KI0"></Youtube>

## Installation

:installation-block{:package="package" :packagePath="packagePath"}

## Example

:content-loader

## Usage

In your controller:

::code-block{tabName="app/controllers/posts_controller.rb"}

```ruby
class PostsController < ApplicationController
  def comments
    render partial: 'posts/comments', locals: { comments: @post.comments }
  end
end
```

::

In your routes:

::code-block{tabName="config/routes.rb"}

```ruby
Rails.application.routes.draw do
  get :comments, to: 'posts#comments'
end
```

::

::code-block{tabName="app/views/index.html.erb"}

```erb
<div data-controller="content-loader" data-content-loader-url-value="<%= comments_path %>">
  <i class="fas fa-spinner fa-spin"></i>
  Loading comments... This content will be replaced by the content of the `posts/comments` partial generated by Rails.
</div>

<div
  data-controller="content-loader"
  data-content-loader-url-value="<%= comments_path %>"
  data-content-loader-refresh-interval-value="5000"
>
  This content will be reloaded every 5 seconds.
</div>

<div
  data-controller="content-loader"
  data-content-loader-url-value="/message.html"
  data-content-loader-load-scripts-value="true"
>
  This content will be replaced by the content of the `/message.html` page in your public folder.
</div>

<div
  data-controller="content-loader"
  data-content-loader-url-value="/message.html"
  data-content-loader-lazy-loading-value=""
>
  This content will be replaced only when the element become visible thanks to Intersection Observers.
</div>

<div
  data-controller="content-loader"
  data-content-loader-url-value="/message.html"
  data-content-loader-lazy-loading-value=""
  data-content-loader-lazy-loading-root-margin-value="30px"
  data-content-loader-lazy-loading-threshold-value="0.4"
>
  You can customize the Intersection Observer options.
</div>

<div
  data-controller="content-loader"
  data-content-loader-url-value="/message.html"
  data-content-loader-lazy-loading-value=""
  data-content-loader-refresh-interval-value="5000"
>
  You can combine lazy loading and refresh interval. The timer will start only after the first fetch.
</div>
```

::

## Configuration

| Attribute                                            | Default     | Description                                  | Optional |
| ---------------------------------------------------- | ----------- | -------------------------------------------- | -------- |
| `data-content-loader-url-value`                      | `undefined` | URL to fetch the content.                    | ❌       |
| `data-content-loader-refresh-interval-value`         | `undefined` | Interval in milliseconds to reload content.  | ✅       |
| `data-content-loader-lazy-loading-value`             | `undefined` | Fetch content when element is visible.       | ✅       |
| `data-content-loader-lazy-loading-root-margin-value` | `0px`       | rootMargin option for Intersection Observer. | ✅       |
| `data-content-loader-lazy-loading-threshold-value`   | `0`         | threshold option for Intersection Observer.  | ✅       |
| `data-content-loader-load-scripts-value`             | `false`     | Load inline scripts from the content.        | ✅       |

## Extending Controller

::extending-controller
::code-block{tabName="app/javascript/controllers/content_loader_controller.js"}

```js
import ContentLoader from "@stimulus-components/content-loader"

export default class extends ContentLoader {
  connect() {
    super.connect()
    console.log("Do what you want here.")
  }
}
```

::
::

## Credits

This controller is inspired by the [official Stimulus example](https://stimulus.hotwired.dev/handbook/working-with-external-resources).
