begin
  require 'mg'
  MG.new('rack-mount.gemspec')
rescue LoadError
end


begin
  require 'hanna/rdoctask'
rescue LoadError
  require 'rake/rdoctask'
end

Rake::RDocTask.new { |rdoc|
  rdoc.rdoc_dir = 'doc'
  rdoc.title    = 'Rack::Mount'
  rdoc.options << '--line-numbers' << '--inline-source'
  rdoc.options << '--charset' << 'utf-8'

  rdoc.rdoc_files.include('README.rdoc')
  rdoc.rdoc_files.include('lib/**/*.rb')
  rdoc.rdoc_files.exclude('lib/rack/mount/mappers/*.rb')
}


require 'rake/testtask'

task :default => :test

Rake::TestTask.new do |t|
  t.libs << 'test'
  t.warning = true
end


task :compile => [
  'lib/rack/mount/regexp/parser.rb',
  'lib/rack/mount/regexp/tokenizer.rb',
  'lib/rack/mount/strexp/parser.rb',
  'lib/rack/mount/strexp/tokenizer.rb'
]

file 'lib/rack/mount/regexp/parser.rb' => 'lib/rack/mount/regexp/parser.y' do |t|
  sh "racc -l -o #{t.name} #{t.prerequisites.first}"
end

file 'lib/rack/mount/regexp/tokenizer.rb' => 'lib/rack/mount/regexp/tokenizer.rex' do |t|
  sh "rex -o #{t.name} #{t.prerequisites.first}"
end

file 'lib/rack/mount/strexp/parser.rb' => 'lib/rack/mount/strexp/parser.y' do |t|
  sh "racc -l -o #{t.name} #{t.prerequisites.first}"
end

file 'lib/rack/mount/strexp/tokenizer.rb' => 'lib/rack/mount/strexp/tokenizer.rex' do |t|
  sh "rex -o #{t.name} #{t.prerequisites.first}"
end

namespace :vendor do
  task :update_multimap do
    system 'git clone git://github.com/josh/multimap.git'
    FileUtils.cp_r('multimap/lib', 'lib/rack/mount/vendor/multimap')
    FileUtils.rm_rf('multimap')
  end
end
