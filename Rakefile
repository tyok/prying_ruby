require 'rake/clean'

SOURCE_DIR  = "src"

def source_path_for(filename, ext = nil)
  transformed_file = ext ? filename.sub(/\.[^.]+$/, ext) : filename
  file = File.join SOURCE_DIR, transformed_file
end

def dest_path_for(filename)
  filename.sub(/^#{SOURCE_DIR}#{File::Separator}/, "")
end

# get all src file... then get all dest file from them
LAYOUT_HTML = dest_path_for FileList.new(source_path_for '**/*.haml').ext("html")
LAYOUT_CSS  = dest_path_for FileList.new(source_path_for '**/*.scss').ext("css")
LAYOUT      = LAYOUT_HTML + LAYOUT_CSS

CLOBBER.include('_site')
CLEAN.include(LAYOUT)

rule '.html' => [proc{|f| source_path_for f, ".haml" }] do |t|
    sh %{ haml -E utf-8 #{t.source} #{t.name} }
end

rule '.css' => [proc{|f| source_path_for f, ".scss" }] do |t|
    sh %{ sass -t compressed #{t.source} #{t.name} }
end

desc 'Generate html and css from sources'
task :build => LAYOUT do
end

desc 'Build and start server'
task :server => :default do
  sh %{ jekyll --server --safe  --auto }
end

desc "Generate html and css"
task :default => :build

