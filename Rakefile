require "rubygems"
require "bundler"
Bundler.setup

require 'spinning_cursor'
require 'driver/rake-tasks/checks'

task :update do
  if ENV['TAG']
    url = "http://selenium.googlecode.com/svn/tags/#{ENV['TAG']}/"
    puts "Checking out the tagged revision at #{url}."
  else
    url = 'http://selenium.googlecode.com/svn/trunk/'
    puts "Checking out the latest revision at #{url}."
  end
  puts "If this is the first time it may take several minutes."

  SpinningCursor.start do
    banner "Checking out the latest version"
    type :spinner
    message "Successfully checked out from SVN."
  end

  svn_operation = File.exists?('.svn-copy') ? 'switch' : 'checkout'
  checkout_result = `svn #{svn_operation} #{url} .svn-copy 2>&1`

  if ! $?.success?
    SpinningCursor.set_message "Failed to checkout, output from SVN follows:"
    SpinningCursor.stop
    raise checkout_result
  else
    if ENV['TAG']
      SpinningCursor.set_message "Successfully checked out tag '#{ENV['TAG']}' from SVN."
    else
      SpinningCursor.set_message "Successfully checked out revision '#{`svnversion .svn-copy/`.rstrip}' from SVN."
    end
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

  # the -C option ignores files that
  copy_result = `rsync -aC --filter='merge .rsync-filters' --exclude='*.git*' .svn-copy/ driver/ 2>&1`

  if ! $?.success?
    SpinningCursor.set_message "Failed to copy files into repo, output from rsync follows:"
    SpinningCursor.stop
    raise copy_result
  end

  SpinningCursor.stop

  puts "\nUpdate complete."
end

task :build do
  # Copied from https://code.google.com/p/selenium/source/browse/tags/selenium-2.25.0/Rakefile#454
  sdk = iPhoneSDK?
  if sdk != nil then
    puts "Building iWebDriver iOS app."
    FileUtils.chdir('driver') do
      sh "cd iphone && xcodebuild -sdk #{sdk} ARCHS=i386 -target iWebDriver", :verbose => false
    end
  else
    puts "XCode not found. Not building the iphone driver."
  end
end