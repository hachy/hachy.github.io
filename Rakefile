desc 'Begin a new post'
task :new_post, :title do |_, args|
  title = args.title
  filename = "_posts/#{Time.now.strftime('%Y-%m-%d')}-#{title}.md"
  if File.exist?(filename)
    abort("rake aborted! #{filename} already exists")
  end
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
    post.puts '---'
    post.puts 'layout: post'
    post.puts "title: \"#{title.gsub(/&/, '&amp;')}\""
    post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M:%S %z')}"
    post.puts 'tags: '
    post.puts '---'
  end
end
