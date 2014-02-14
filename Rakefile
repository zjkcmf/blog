task :default => :generate

desc 'Create new post with rake "post[post-name]"'
task :post, [:title, :categories] do |t, args|
  if args.title then
    new_post(args.title,args.categories)
  else
    puts 'rake "post[post-name, categories]"'
  end
end

desc 'Build site with Jekyll'
task :generate => :clean do
  `jekyll`
end

desc 'Start server'
task :server => :clean do
  `jekyll serve --port 80`
end

desc 'Deploy with rake "depoly[comment]"'
task :deploy, [:comment] => :generate do |t, args|
  if args.comment then
    `git commit . -m '#{args.comment}' && git push origin gh-pages`
  else
    `git commit . -m 'new deployment' && git push origin gh-pages`
  end
end

desc 'Clean up'
task :clean do
  `rm -rf _site`
end

def new_post(title, categories)
  time = Time.now
  filename = "_posts/" + time.strftime("%Y-%m-%d-") + title + '.markdown'
  if File.exists? filename then
    puts "Post already exists: #{filename}"
    return
  end
  uuid = `uuidgen | tr "[:upper:]" "[:lower:]" | tr -d "\n"`
  File.open(filename, "wb") do |f|
    f << <<-EOS
---
title: #{title}
layout: post
categories: [#{categories}]
guid: urn:uuid:#{uuid}
tags:
  - 
---


EOS
  end
  puts "created #{filename}"
  `git add #{filename}`
end
