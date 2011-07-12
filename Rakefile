

namespace :wp do 

  desc "Download latest wordpress from wordpress.org"
  task :download do
    sh "wget http://wordpress.org/latest.tar.gz"
  end

  desc "untar wordpress"
  task  :untar do
    sh "tar -xvf latest.tar.gz"
  end

  desc "clear wp_content and symlink source controlled wp-content" 
  task :clear_n_symlink_content do
    require 'pathname'
    sh "rm -rf wordpress/wp-content"
    sh "ln -s #{Pathname.pwd}/wp-content #{Pathname.pwd}/wordpress/wp-content"
  end

  desc "clear up wordpress"
  task  :clear_up do
    sh "rm -rf latest.tar.gz"
  end

end


