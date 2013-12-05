# A sample Guardfile
# More info at https://github.com/guard/guard#readme

Dir.glob("llt*") do |dir|
  guard 'rake', task: dir.to_sym do
    watch(%r{^#{dir}\/.*\.rb$})
  end
end
