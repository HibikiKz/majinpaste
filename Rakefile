require 'yaml'
require 'logger'
require 'active_record'

namespace :db do
  db_config = YAML.load_file('./config/database.yml')['development']

  desc "Create Database"
  task :create do
    puts "Create Database...."
    ActiveRecord::Base.establish_connection(
      adapter: 'postgresql',
      host: db_config['host'],
      username: db_config['user'],
      password: db_config['password'],
      database: db_config['database']
    )
    ActiveRecord::Base.connection.create_database(db_config['database'])
    puts "Done!"
  end
 
  desc "Drop Database"
  task :drop do
    puts "Dropping Database...."
    ActiveRecord::Base.establish_connection(
      adapter: 'postgresql',
      host: db_config['host'],
      username: db_config['user'],
      password: db_config['password'],
      database: db_config['database']
    )
    ActiveRecord::Base.connection.drop_database(db_config['database'])
    puts "Done!"
  end
 
  desc "Database Migrate"
  task :migrate do
    puts "Migrating Database...."
    ActiveRecord::Base.establish_connection(
      adapter: 'postgresql',
      host: db_config['host'],
      username: db_config['user'],
      password: db_config['password'],
      database: db_config['database']
    )
    migrator = ActiveRecord::MigrationContext.new("db/migrate/")
    migrator.migrate
    Rake::Task["db:schema"].invoke
    puts "Done!"
  end

  desc "Create DB Schema"
  task :schema do
    ActiveRecord::Base.establish_connection(
      adapter: 'postgresql',
      host: db_config['host'],
      username: db_config['user'],
      password: db_config['password'],
      database: db_config['database']
    )
    require 'active_record/schema_dumper'
    File.open("db/schema.rb", "w:utf-8") do |schema_file|
      ActiveRecord::SchemaDumper.dump(ActiveRecord::Base.connection, schema_file)
    end
  end
end


