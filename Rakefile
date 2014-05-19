require 'json'
require 'nokogiri'

task :default => [:default_task]

task :default_task do
  puts "Hello, world"
end

task :add_google_analytics do
  tracking_code = <<CODE
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-46710237-15', 'emacs.kr');
  ga('send', 'pageview');

</script>
</head>
CODE

  content = open('target/index.html', 'r').read.gsub(/<\/head>/, tracking_code)
  File.open('target/index.html', 'w+'){ |f| f.write content }
end

task :add_link_to_translator do
  content = open('target/index.html', 'r').read.gsub(/nacyot/, "<a href='http://nacyot.com'>nacyot</a>")
  File.open('target/index.html', 'w+'){ |f| f.write content }
end

task :rewrite_cname do
  File.open('target/CNAME', 'w+'){ |f| f.write "sexy.emacs.kr" }
end

task :add_links do
  links_html = JSON.parse(open('data/sites.json').read)
    .delete_if{|link| link["language"] != "kr" }
    .map{|link| "<li><a href='#{link["url"]}'>#{link["title"]}</a></li>\n"}
    .inject(""){|text, link| text += link}
  
  doc = Nokogiri::HTML(open('target/index.html'))
  target = doc.search("#be-free").first

  node = Nokogiri::XML::Node.new("section", doc)
  node["class"] = "white"
  node.inner_html = "<div>\n<h2>한국어 자료</h2>\n<p>\n<ul>\n" + links_html + "</ul>\n</p>\n</div>"

  target.add_previous_sibling node
  
  File.open('target/index.html', 'w+'){ |f| doc.write_xml_to f }
end
