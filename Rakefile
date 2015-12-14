# The main structure of these rake tasks was borrowed from https://github.com/ryanb/dotfiles/blob/master/Rakefile

require 'rake'

desc "install the dot files into user's home directory"
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

def copy_config_file(file, configs_path, backup = true)
  system %Q{mv "#{configs_path}/#{file}" "#{configs_path}/#{file}.1"} if backup
  system %Q{cp "$PWD/#{file}" "#{configs_path}/#{file}"}
end