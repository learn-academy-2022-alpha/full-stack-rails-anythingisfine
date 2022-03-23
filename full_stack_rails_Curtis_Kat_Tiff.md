#Blog Controller
class BlogController < ApplicationController
  def index
    @blogs = Blog.all
  end
  def show
    @blog = Blog.find(params[:id])
  end
end

def new
  @blog = Blog.new
end

def create
  @blog = Blog.create(blog_params)
  if @blog.valid?
    redirect_to blogs_path
  else
    redirect_to new_blog_path
  end
end

private
def blog_params
  params.require(:blog).permit(:title, :content)
end
end

#routes
Rails.application.routes.draw do
  get 'blogs' => 'blog#index', as: 'blogs'
  get 'blogs/new' => 'blog#new', as: 'new_blog'
  get 'blogs/:id' => 'blog#show', as:'blog'
  post 'blogs' => 'blog#create'
  root 'blog#index'
end


#Show
<h3><%= @blog.title %></h3>
<p><%= @blog.content %></p>
<%= link_to 'back to home', blogs_path %>

#Index Page
<h1>Random Blog</h1>

<ul>
    <% @blogs.each do |blog| %>
      <li>
        <%= link_to blog.title, blog_path(blog) %>
      </li>
    <% end %>
</ul>

#New Page
<h3>Share Your Thoughts</h3>
<%= form_with model: @blog do |form| %>

  <%= form.label :title %>
  <%= form.text_field :title %>

  <br>
  <%= form.label :content %>
  <%= form.text_field :content %>

  <br>
  <%= form.submit 'Add Post' %>
<% end %>

<%= link_to 'Return', blogs_path %>
