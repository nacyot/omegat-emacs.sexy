require 'json'
require 'nokogiri'

task :default => [:default_task]

task :default_task do
  puts "Hello, world"
end

task :rewrite_cname do
  File.open('target/CNAME', 'w+'){ |f| f.write "sexy.emacs.kr" }
end

task :add_links do
  links_html = JSON.parse(open('data/sites.json').read)
    .map{|link| "<li><a href='#{link["url"]}'>#{link["title"]}</a></li>\n"}
    .inject(""){|text, link| text += link}

  puts links_html
  
  doc = Nokogiri::HTML(open('target/index.html'))
  target = doc.search("#be-free").first

  node = Nokogiri::XML::Node.new("section", doc)
  node["class"] = "white"
  node.inner_html = "<div>\n<h2>한국어 자료</h2>\n<p>\n<ul>\n" + links_html + "</ul>\n</p>\n</div>"

  target.add_previous_sibling node
  
  File.open('target/index.html', 'w+'){ |f| doc.write_xml_to f }
end
