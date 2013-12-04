require 'colorize'

namespace :gems do
  desc "Run all examples of extracted gems indepently within their Bundler context"
  task :spec do
    results = Dir.glob("llt-*").map do |dir|
      Dir.chdir(dir) do
        Bundler.with_clean_env do
          system("rake")
        end
      end
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
    Dir.glob("llt-*").map do |dir|
      Dir.chdir(dir) do
        Bundler.with_clean_env do
          system("bundle update")
        end
      end
    end
  end
end

task default: 'gems:spec'
