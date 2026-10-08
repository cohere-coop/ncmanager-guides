require 'html-proofer'

task :default => [:test]

module HTMLProofer
  class Cache
    # HTMLProofer writes to the cache twice, once after checking the external
    # URLS (when HTMLProofer::Runner#validate_external_urls calls
    # UrlValidator#run) and then again after checking internal URLs (in
    # HTMLProofer::Runner#validate_internal_urls).
    #
    # The second write erases the first write, making it useless. This is a hack
    # workaround to keep that from happening by not writing to the cache log
    # when it is empty.
    def write
      File.write(cache_file, @cache_log.to_json) unless @cache_log.empty?
    end
  end
end

task test: %i[test_workflows html_proofer]

task :html_proofer do
  sh 'bundle exec jekyll build --future'
  options = {
    assume_extension: true,
    cache: { timeframe: '30d', storage_dir: 'tmp/.htmlproofer' }, # storage_dir must match the path cached in .github/workflows/test.yml
    # check_html: true, # Problem with theme nav
    check_external_hash: true,
    check_img_http: true,
    check_sri: true,
    http_status_ignore: [999], # Fake status used by LinkedIn
    enforce_https: true,
    parallel: { in_processes: 3 },
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
