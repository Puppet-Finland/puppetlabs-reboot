source ENV['GEM_SOURCE'] || "https://rubygems.org"

def location_for(place, fake_version = nil)
  if place =~ /^(git:[^#]*)#(.*)/
    [fake_version, { :git => $1, :branch => $2, :require => false }].compact
  elsif place =~ /^file:\/\/(.*)/
    ['>= 0', { :path => File.expand_path($1), :require => false }]
  else
    [place, { :require => false }]
  end
end

gem "puppet", *location_for(ENV['PUPPET_LOCATION'] || '~> 3.2.3')
gem "facter", *location_for(ENV['FACTER_LOCATION'] || '~> 1.6')
gem "hiera", *location_for(ENV['HIERA_LOCATION'] || '~> 1.0')

is_ruby18 = RUBY_VERSION.start_with? '1.8'

group :development, :test do
  if is_ruby18
    gem 'pry', "~> 0.9.12"
  else
    gem 'pry'
  end
  gem 'rake'
  gem 'rspec', "~> 2.13.0"
  gem 'mocha', "~> 0.10.5"
  gem 'puppetlabs_spec_helper'
  gem 'nokogiri', "~> 1.5.10"
  gem 'mime-types', '<2.0',      :require => false
  gem 'puppet-lint',             :require => false
  gem 'simplecov',               :require => false
end

beaker_version = ENV['BEAKER_VERSION']
group :system_tests do
  if beaker_version
    gem 'beaker', *location_for(beaker_version)
  else
    gem 'beaker',                :require => false, :platforms => :ruby
  end
end

# see http://projects.puppetlabs.com/issues/21698
platforms :mswin, :mingw do
  gem "ffi", "~> 1.9.3", :require => false
  gem "sys-admin", "~> 1.5.6", :require => false
  gem "win32-dir", "~> 0.3.7", :require => false
  gem "win32-process", "~> 0.6.5", :require => false
  gem "win32-service", "~> 0.7.2", :require => false
  gem "win32-taskscheduler", "~> 0.2.2", :require => false
  gem "minitar", "~> 0.5.4", :require => false
end

if File.exists? "#{__FILE__}.local"
  eval(File.read("#{__FILE__}.local"), binding)
end
