## Partials and Helpers

Navigation!
```
<%= navigation do |n| %>
  <%= n.item class: 'active' do %>
    <a href="#">
      <%= icon_helper('home', :material) %>
      <span class="title">Home</span>
    </a>
  <% end %>
  <%= n.item do %>
    <a href="#">
      <%= icon_helper('settings', :material) %>
      <span class="title">Settings</span>
    </a>
  <% end %>
<% end %>
```
