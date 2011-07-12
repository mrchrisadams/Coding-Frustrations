
require 'erb'
require 'pathname'
require 'term/ansicolor'
# including here gives us easy text colouring
include Term::ANSIColor

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
    sh "rm -rf wordpress/wp-content"
    sh "ln -s #{Pathname.pwd}/wp-content #{Pathname.pwd}/wordpress/wp-content"
  end

  desc "clear up wordpress"
  task  :clear_up do
    sh "rm -rf latest.tar.gz"
  end

  desc "clear apache conf files"
  task  :clear_up_apache_conf_files do
    sitename = "codingfrustrations.dev"
    sh "rm -rf /etc/apache2/sites-available/#{sitename}.conf"
    sh "rm -rf /etc/apache2/sites-enabled/#{sitename}.conf"
  end

  desc "create apache conf file"
  task :make_apache_conf do
    # define a few handy variables
    sitename = "codingfrustrations.dev"
    docroot = Pathname.pwd
    template_path = File.join(docroot, "config", "apache.conf.erb")
    # Now pass in the variables into the template with the binding method
    populated_conf_template = ERB.new(File.read(template_path)).result(binding)
    # Write it locally, so we can move it into the correct path for Apache
    File.open(File.join(docroot, "#{sitename}.conf"), 'w') {|f| 
      f.write populated_conf_template
    }
  end

  desc "Move generated apache file into correct directory for Apache"
  task :move_conf_file do
    sitename = "codingfrustrations.dev"
    FileUtils.mv "#{Pathname.pwd}/#{sitename}.conf", "/etc/apache2/sites-available/#{sitename}.conf"
  end

  desc "Symlink generated conf file for Apache to see it"
  task :symlink_generated_apache_conf do
    sitename = "codingfrustrations.dev"
    FileUtils.symlink "/etc/apache2/sites-available/#{sitename}.conf", "/etc/apache2/sites-enabled/#{sitename}.conf"
  end

  desc "Set up apache site"
  task :set_up_apache_site => [:clear_up_apache_conf_files, :make_apache_conf, :move_conf_file, :symlink_generated_apache_conf] do
    print green "Apache site now set up. Time to make your database, and get coding!"
    
  end

end

namespace db do

  desc "create database"
  task :create do
    sh "mysqladmin create coding_frustrations_dev -u root --password=''"
  end
  
  desc "Destroy database"
  task :destroy do
    sh "yes | mysqladmin drop coding_frustrations_dev -u root --password=''"
  end

  desc "Backup db to sqldump"
  task :backup do
    # create a sane timestamp
    # TODO compress the dump before putting it into the db directory
    timestamp = Time.now.strftime("%Y-%m-%m-%H-%M")
    sh "mysqldump coding_frustrations_dev -u root --password='' > db/timestamp.sql"
  end


end
