# The main structure of these rake tasks was borrowed from https://github.com/ryanb/dotfiles/blob/master/Rakefile

require 'rake'

desc "install settings files into user's Sublime Text 3 packages directory"
task :install do
  replace_all = false
  files = Dir['*'] - %w[Rakefile]
  configs_path = "#{ENV['HOME']}/Library/Application Support/Sublime Text 3/Packages/User"
  files.each do |file|
    if File.exist?(File.join(configs_path, file))
      if replace_all
        copy_config_file(file, configs_path)
      else
        print "overwrite #{configs_path}/#{file}? [ynaq]"
        case $stdin.gets.chomp
        when 'a'
          replace_all = true
          copy_config_file(file, configs_path)
        when 'y'
          copy_config_file(file, configs_path)
        when 'q'
          exit
        else
          puts "skipping #{configs_path}/#{file}"
        end
      end
    else
      copy_config_file(file, configs_path, false)
    end
  end
end

desc "fetch user's settings files to the repo"
task :fetch do
  configs_path = "#{ENV['HOME']}/Library/Application Support/Sublime Text 3/Packages/User"
  ["Preferences.sublime-settings", "Package Control.sublime-settings"].each do |file|
    if File.exist?(File.join(configs_path, file))
      fetch_config_file(file, configs_path)
    else
      puts "There is no #{file} in user's Sublime 3 Text packages directory..."
    end
  end
end

def fetch_config_file(file, configs_path)
  system %Q{cp "#{configs_path}/#{file}" "$PWD/#{file}"}
end

def copy_config_file(file, configs_path, backup = true)
  system %Q{mv "#{configs_path}/#{file}" "#{configs_path}/#{file}.1"} if backup
  system %Q{cp "$PWD/#{file}" "#{configs_path}/#{file}"}
end