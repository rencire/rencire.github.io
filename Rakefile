require "rubygems"
require "tmpdir"

require "bundler/setup"
require "jekyll"


# Tasks from:
# http://ixti.net/software/2013/01/28/using-jekyll-plugins-on-github-pages.html

# Change your GitHub reponame
GITHUB_REPONAME = "rencire/rencire.github.io"


desc "Generate blog files"
task :generate do
  Jekyll::Site.new(Jekyll.configuration({
    "source"      => ".",
    "destination" => "_site"
  })).process
end


desc "Generate and publish blog to gh-pages"
task :publish => [:generate] do
  Dir.mktmpdir do |tmp|
    cp_r "_site/.", tmp

    pwd = Dir.pwd
    Dir.chdir tmp

    system "git init"
    system "git add ."
    message = "Site updated at #{Time.now.utc}"
    system "git commit -m #{message.inspect}"
    system "git remote add origin git@github.com:#{GITHUB_REPONAME}.git"
    system "git push origin master --force"

    Dir.chdir pwd
  end
end


task :new_draft do
  drafts_dir = "_drafts"
  title = $stdin.readline
  
  titlename = title.downcase.gsub(/\n/, '').gsub(' ', '-')
  filename = "#{drafts_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{titlename}.md"
  
  open(filename, 'w') do |post|
    post.puts "--"
    post.puts "title: \"#{title.gsub(/\n/,'')}\""
    post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M:%S %z')}"
    post.puts "comments: true"
    post.puts "categories: "
    post.puts "--"
  end

end
