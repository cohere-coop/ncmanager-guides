require 'html-proofer'

task :default => [:test]

task test: %i[test_workflows html_proofer]

task :html_proofer do
  sh 'bundle exec jekyll build --future'
  options = {
    allow_missing_href: true, # Matches html-proofer 3.x behavior; USWDS theme's identifier footer renders `<a href="">`
    assume_extension: '.html',
    cache: { timeframe: { external: '30d' }, storage_dir: 'tmp/.htmlproofer' }, # storage_dir must match the path cached in .github/workflows/test.yml
    # check_html: true, # Problem with theme nav
    check_external_hash: true,
    check_sri: true,
    ignore_status_codes: [999], # Fake status used by LinkedIn
    enforce_https: true,
  }
  proofer = HTMLProofer.check_directory('./_site', options)

  proofer.before_request do |request|
    if request.base_url.to_s.start_with?('https://twitter.com')
      # Twitter responds with a 400 if it thinks you have an unsupported browser
      request.options[:headers]['User-Agent'] = 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_6) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.0.1 Safari/605.1.15'
    end
  end

  proofer.run
end

task :test_workflows do
  Dir['.github/workflows/*.yml'].each do |file|
    puts "Checking #{file}"
    YAML.load_file file
  end
end
