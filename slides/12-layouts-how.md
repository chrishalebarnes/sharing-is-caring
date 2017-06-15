## How?

There are `content_for` blocks for :title, :stylesheets, :header, :navigation, :footer and :scripts

All of them have defaults

Render the shared admin layout like this:

```
<%= render 'railsifi/layouts/admin' do %>
  <%= yield %>
<% end %>
```
