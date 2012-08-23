require "rubygems"
require "bundler"
Bundler.setup

require 'spinning_cursor'

task :update do
  url = 'http://selenium.googlecode.com/svn/trunk/'
  puts "Checking out the latest version from #{url}."
  puts "If this is the first time it may take several minutes."

  SpinningCursor.start do
    banner "Checking out the latest version"
    type :spinner
    message "Done"
  end

  checkout = `svn co #{url} .svn-copy`

  if $?.success?
    SpinningCursor.set_message "Failed to checkout, output from SVN follows:"
    SpinningCursor.stop
    abort checkout
  end

  SpinningCursor.stop


end