require "rake/rdoctask"
require "rake/testtask"

spec = Gem::Specification.load(File.expand_path("vanity.gemspec", File.dirname(__FILE__)))

desc "Push new release to gemcutter and git tag"
task :push do
  sh "git push"
  puts "Tagging version #{spec.version} .."
  sh "git tag #{spec.version}"
  sh "git push --tag"
  puts "Building and pushing gem .."
  sh "gem build #{spec.name}.gemspec"
  sh "gem push #{spec.name}-#{spec.version}.gem"
end

desc "Install #{spec.name} locally"
task :install do
  sh "gem build #{spec.name}.gemspec"
  sudo = "sudo" unless File.writable?( Gem::ConfigMap[:bindir])
  sh "#{sudo} gem install #{spec.name}-#{spec.version}.gem"
end

task :default=>:test
desc "Run all tests"
Rake::TestTask.new do |task|
  task.test_files = FileList['test/*_test.rb']
  task.verbose = true
  #task.warning = true
end

Rake::RDocTask.new(:rdoc) do |rdoc|
  rdoc.rdoc_files.include "README.rdoc", "lib/**/*.rb"
  rdoc.options = spec.rdoc_options
end

task :report do
  require "lib/vanity"
  Vanity.playground.load_path = "test/experiments"

=begin
=end
  experiment(:null_abc).reset!
  # Control	182	35	19.23%	N/A
  182.times { |i| experiment(:null_abc).alternatives[0].participating!(i) }
  35.times  { |i| experiment(:null_abc).alternatives[0].conversion!(i) }
  # Treatment A	180	45	25.00%	1.33
  180.times { |i| experiment(:null_abc).alternatives[1].participating!(i + 200) }
  45.times  { |i| experiment(:null_abc).alternatives[1].conversion!(i + 200) }
  # Treatment B	189	28	14.81%	-1.13
  189.times { |i| experiment(:null_abc).alternatives[2].participating!(i + 400) }
  28.times  { |i| experiment(:null_abc).alternatives[2].conversion!(i + 400) }
  # Treatment C	188	61	32.45%	2.94
  188.times { |i| experiment(:null_abc).alternatives[3].participating!(i + 600) }
  61.times  { |i| experiment(:null_abc).alternatives[3].conversion!(i + 600) }

  experiment(:age_and_zipcode).reset!
  182.times { |i| experiment(:age_and_zipcode).alternatives[0].participating!(i) }
  35.times  { |i| experiment(:age_and_zipcode).alternatives[0].conversion!(i) }
  180.times { |i| experiment(:age_and_zipcode).alternatives[1].participating!(i + 200) }
  45.times  { |i| experiment(:age_and_zipcode).alternatives[1].conversion!(i + 200) }
  experiment(:age_and_zipcode).complete!

  Vanity::Commands.report
end
