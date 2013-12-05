require 'colorize'

namespace :git do
  desc 'Update all git submodules to their current master tip'
  task :update do
    system('git submodule update --remote')
    system(%{git submodule foreach -q --recursive 'branch="$(git config -f $toplevel/.gitmodules submodule.$name.branch)"; git checkout $branch' >/dev/null 2>&1})
  end
end

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

desc 'Run a nailgun server as background job'
task :ng do
  # nailgun falls when there are jruby opts defined
  exec 'JRUBY_OPTS= jruby --ng-server'
end

task default: 'gems:spec'

class SpecFailed < StandardError; end

def run_in_directory(dir, command = 'rake')
  res = Dir.chdir(dir) do
    Bundler.with_clean_env do
      system(command)
    end
  end

  raise SpecFailed.new unless res
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

