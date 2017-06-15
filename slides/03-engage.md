## Example

Engage
```
<header id="header">
  <% if current_user.is_a?(Users::AccountAdmin) %>
    class="clearfix admin-header"
  <% else %>
    class="clearfix"
  <% end %>
  data-ma-theme="blue">
...
</header>
```
