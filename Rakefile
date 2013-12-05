require 'colorize'

namespace :gems do
  desc "Run all examples of extracted gems indepently within their Bundler context"
  task :spec do
    results = llt_dirs.map do |dir|
      run_in_directory(dir)
    end

    if results.all?
      puts "#{results.count} tasks - All GREEN".colorize(:green)
    else
      failed = results.count { |result| result == false }
      puts "#{failed} of #{results.count} tasks FAILED".colorize(:red)
    end
  end

  desc 'Bundle update all gems'
  task :bu do
    llt_dirs.map do |dir|
      run_in_directory(dir, 'bundle update')
    end
  end

  desc 'Bundle install all gems'
  task :bi do
    llt_dirs.map do |dir|
      run_in_directory(dir, 'bundle install')
    end
  end
end

task default: 'gems:spec'

def run_in_directory(dir, command = 'rake')
  Dir.chdir(dir) do
    Bundler.with_clean_env do
      system(command)
    end
  end
end

def llt_dirs
  Dir.glob("llt*")
end

# One hidden rake task for every gem, used by guard-rake
llt_dirs.each do |dir|
  task dir.to_sym do
    run_in_directory(dir)
  end
end

