1. Use credentials API
  1. Change production to require MASTER_KEY
  2. https://github.com/freeletics/creds
2. Use Yard for documentation
  1. Install Yard by including gem 'yard' in `Gemfile` (development only)
    ```
    group :development do
      gem 'yard'
    end
    ```
  2. Include `/doc` and `/.yardoc` in .gitignore
  3. Start Yard server by typing in `yard server -r` in project root directory (-r reloads on every request)
  4. Use cheatsheet to write documentation for classes and methods https://devhints.io/rdoc
3. Use `annotate` to annotate schema in models
  1. Install gem for development
    ```
    group :development do
      gem 'annotate'
    end
    ```
  2. Run `rails g annotate:install`
  3. Generate in RDOC format rather than plaintext by configuring `auto_annotate_models.rake`
    ```
    'format_bare'               => 'false',
    'format_rdoc'               => 'true',
    ```
