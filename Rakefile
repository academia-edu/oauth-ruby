%w[rubygems rake rake/clean rake/testtask fileutils].each { |f| require f }
$LOAD_PATH << File.dirname(__FILE__) + '/lib'
require 'rake'
require 'bundler/gem_tasks'
require 'oauth'

begin
  require 'jeweler'
  Jeweler::Tasks.new do |s|
    s.name = %q{oauth}
    s.version = OAuth::VERSION
    s.authors = ["Pelle Braendgaard", "Blaine Cook", "Larry Halff", "Jesse Clark", "Jon Crosby", "Seth Fitzsimmons", "Matt Sanford", "Aaron Quint"]
    s.email = "oauth-ruby@googlegroups.com"
    s.description = "OAuth Core Ruby implementation"
    s.summary = s.description
    s.rubyforge_project = %q{oauth}
    s.add_development_dependency(%q<actionpack>, [">=2.3.5"])
    s.add_development_dependency(%q<rack>, [">= 1.0.0"])
    s.add_development_dependency(%q<mocha>, [">= 0.9.8"])
    s.add_development_dependency(%q<typhoeus>, [">= 0.1.13"])
    s.add_development_dependency(%q<em-http-request>, [">= 0.2.10"])
    s.add_development_dependency(%q<curb>, [">= 0.6.6.0"])
    
    s.files.include '.gemtest'
  end
  Jeweler::GemcutterTasks.new
rescue LoadError
  puts "Jeweler (or a dependency) not available. Install it with: sudo gem install jeweler"
end

Rake::TestTask.new do |t|
  t.libs << "test"
  t.test_files = FileList['test/**/*test*.rb']
  t.verbose = true
end

Dir['tasks/**/*.rake'].each { |t| load t }

desc "Tag #{Bundler::GemHelper.new.send(:version_tag)}, build and push to gemfury"
task :release_internal do |t| 
  require 'gemfury'

  class ReleaseInternalGem < Bundler::GemHelper
    def release_gem
      guard_clean
      built_gem_path = build_gem
      if Bundler::VERSION =~ /1\.3\.\d/
        tag_version { git_push } unless already_tagged?
      else
        guard_already_tagged
        tag_version { git_push }
      end 
      `fury push #{built_gem_path}`
      Bundler.ui.confirm "Pushed #{name} #{version} to gemfury"
    end 
  end 

  ReleaseInternalGem.new.release_gem
end

module Bundler
  class GemHelper
    def release_gem
      raise 'STOP. This is an internal gem. Use `rake release_internal` instead'
    end
  end
end

task :default => :test
