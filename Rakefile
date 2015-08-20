desc "Build including HAML layous"
task :build do
  puts "Parsing HAML layout files..."
  puts `for file in _layouts/*.haml; do haml $file ${file%.haml}.html; done`
  puts `jekyll build`
end

