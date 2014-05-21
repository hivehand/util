bin_dir = File.expand_path "~/bin"
directory bin_dir

files = %w[ dl jj loo path ]
files += Dir.glob("git-*")

task :default => :deploy

task :deploy => bin_dir do
  files.each do |file|
    target = "#{bin_dir}/#{file}"
    unless FileUtils.uptodate?(target, [file])
      # Setting the preserve flag preserves the exec bits, but also
      # preserves the original timestamp, which isn't really what we
      # want. We therefore invoke touch as a follow-up.
      FileUtils.cp(file, target, preserve: true)
      FileUtils.touch(target)
      puts "Updated #{target}."
    end
  end
end
