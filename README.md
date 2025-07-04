# Scott Efein's Tech Blog

A personal tech blog hosted on GitHub Pages, focusing on technology, APIs, and AI integration.

## Site Structure

- `index.md` - Main landing page with blog post listings
- `_config.yml` - Jekyll configuration
- `_layouts/` - Custom layouts for pages and posts
- `mcp-blog-post.md` - Blog post about Model Context Protocol

## Local Development

To run this site locally:

1. Install Ruby and Bundler
2. Create a `Gemfile`:
   ```ruby
   source "https://rubygems.org"
   gem "github-pages", group: :jekyll_plugins
   ```
3. Run:
   ```bash
   bundle install
   bundle exec jekyll serve
   ```
4. Visit `http://localhost:4000`

## GitHub Pages Setup

This site is configured to work with GitHub Pages out of the box. Simply:

1. Push your changes to the `main` branch
2. Go to your repository settings
3. Enable GitHub Pages from the `main` branch
4. Your site will be available at `https://scottefein.github.io`

## Adding New Posts

To add a new blog post:

1. Create a new `.md` file with front matter:
   ```yaml
   ---
   layout: post
   title: "Your Post Title"
   date: YYYY-MM-DD
   categories: [category1, category2]
   excerpt: "Brief description of your post"
   ---
   ```
2. Add your content below the front matter
3. Update `index.md` to link to your new post

## Customization

- Edit `_config.yml` to update site metadata
- Modify `_layouts/default.html` to change the overall design
- Update the CSS in the default layout for styling changes
