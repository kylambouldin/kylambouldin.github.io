---
layout: post
title: Authentication
order: 8
---

Today we are going to update the nav bar labels and links if the user is logged in. Then create new Dashboard and Admin landing pages for different types of logged in users. And lastly, add basic authentication to make sure to route users correctly based on their role.

On our nav bar we have the "Start Now" and "login" buttons. We want to change those to "Dashboard" and "logout", respectively, if the user is a customer who is logged in.

Add the following code to the header partial:
```
<li>
  <% if user_signed_in? %>
    <%= link_to("Dashboard", dashboard_path, class: "btn", id: "sign-up") %>
  <% else %>
    <%= link_to("Start Now", new_user_registration_path, class: "btn", id: "sign-up") %>
  <% end %>
</li>

<li>
  <% if user_signed_in? %>
    <%= link_to "Logout", destroy_user_session_path, method: :delete %>
  <% else %>
    <%= link_to "Login", new_user_session_path %>
  <% end %>
</li>
```
Using the devise method `user_signed_in?`, we can change the text and the url for the button.

Next, I want admin users to have an admin portal instead of a dashboard. Add this code to further differentiate the button for logged in users:
```
<% if current_user.admin? %>
  <%= link_to("Admin", admin_path, class: "btn", id: "sign-up") %>
<% else %>
  <%= link_to("Dashboard", dashboard_path, class: "btn", id: "sign-up") %>
<% end %>
```

## Now we have to create controllers for the admin and dashboard paths.
### DashboardController
Create a DashboardController with a method dashboard. Also be sure authenticate users so that only logged in users can access this page. If the user is not logged in, they will be redirected to the login page.
```
class DashboardController < ApplicationController
  before_action :authenticate_user!

  def dashboard
  end
end
```
Next, create an html.erb file in `app/views/dashboard` called `dashboard.html.erb`. Add any text you want in this file for now.

### AdminController
Before creating the AdminController, we need to have a way to check if a user is an admin. For now, let's just add a simple function to the user model. In the future we will make a database migration to add this as a column.
```
def admin?
  return true
end
```
Now we can create an AdminController with the method admin. We need to authenticate users here too, but also add another before_action to verify that the user is an admin using the method we just created. If for some reason a non-admin user tries to access any methods in the AdminController, they will be rerouted to the dashboard.
```
class AdminController < ApplicationController
  before_action :authenticate_user!
  before_action :verify_admin

  def verify_admin
    redirect_to dashboard_path if !current_user.admin?
  end
end
```
Next, create an html.erb file in `app/views/admin` called `admin.html.erb`. Add any text you want in this file for now.

### Routes
Now that we have our new controllers, we need to set up the routes. Add these lines in your `routes.rb` file.
```
get '/dashboard' => 'dashboard#dashboard', as: :dashboard
get '/admin' => 'admin#admin', as: :admin
```
### Redirects
The last little thing is to redirect your users after they login to the correct location. This is another Devise method that you add in your main `ApplicationController`
```
def after_sign_in_path_for(resource)
  if resource.admin?
    admin_path
  else
    dashboard_path
  end
end
```

## Test!
Test you app on your localhost, then push it up to your staging environment!
