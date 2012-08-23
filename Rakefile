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
    message "Successfully checked out latest version."
  end

  checkout_result = `svn co #{url} .svn-copy 2>&1`

  if ! $?.success?
    SpinningCursor.set_message "Failed to checkout, output from SVN follows:"
    SpinningCursor.stop
    raise checkout_result
  end

  SpinningCursor.stop

  SpinningCursor.start do
    banner "Generating atoms.h for iOS"
    type :dots
    message "Successfully generated atoms.h for iOS."
  end

  FileUtils.chdir('.svn-copy') do
    generate_result = `./go clean iphone_atoms 2>&1`
  end

  if ! $?.success?
    SpinningCursor.set_message "Failed to generate atoms.h for iOS, output from ./go follows:"
    SpinningCursor.stop
    raise generate_result
  end

  SpinningCursor.stop

  SpinningCursor.start do
    banner "Copy files into repo"
    type :dots
    message "Successfully copied files into repo."
  end

  copy_result = `rsync -a --filter='merge .rsync-filters' .svn-copy/ driver/ 2>&1`

  if ! $?.success?
    SpinningCursor.set_message "Failed to copy files into repo, output from rsync follows:"
    SpinningCursor.stop
    raise copy_result
  end

  SpinningCursor.stop

  puts "\nUpdate complete."
end