
# require 'find'

source_files = Rake::FileList.new("vendor/**/*.md")

task :default => %w[tree symlinks]
task :symlinks => source_files.ext(".html.md")

task :update do
  sh "git submodule update --init --remote"
end

task :tree do
  FileUtils.mkdir_p(File.join(%w[source peatio]))
  FileUtils.mkdir_p(File.join(%w[source sdk]))
end

task :clean do
  puts "Removing symlinks"
  files = Rake::FileList.new("source/**/*.html.md")
  files.include do |f|
    File.symlink?(f)
  end
  FileUtils.rm_f(files)
end

rule ".html.md" => ".md" do |t|
  basefile = File.basename(t.source)
  basepath = File.dirname(t.source)
  targetfile = File.basename(t.name)
  begin
    case
    when basefile == 'LICENSE.md'
      next

    when t.source == 'vendor/peatio/README.md'
      targetfile = 'about.html.md'

    when t.source == 'vendor/peatio/CHANGELOG.md'
      targetfile = 'changelog.html.md'

    when basepath == 'vendor/peatio/docs'
      targetfile = File.join('peatio', targetfile)

    when basepath == 'vendor/peatio-sdk/docs'
      targetfile = File.join('sdk', targetfile)

    else
      puts "Ignoring file #{t.source}"
      next

    end

    source = File.join('..', t.source)
    target = File.join('source', targetfile)
    puts "Symlink: #{source} #{target}"
    File.symlink(source, target)

  rescue Errno::EEXIST
    puts "File #{target} Exists"
  end
end
