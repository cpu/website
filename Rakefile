require 'html-proofer'

task :test do
  sh "bundle exec jekyll build"
  HTMLProofer.check_directory("./_site", {
    :url_ignore => [
      # Do not check the ISRG Root test pages - the CI system won't have the X1
      # root available in /etc/ssl/certs to verify "valid-isrgrootx1" and the
      # others are deliberately expired & revoked.
      /expired-isrgrootx1\.letsencrypt\.org/,
      /revoked-isrgrootx1\.letsencrypt\.org/,
      /valid-isrgrootx1\.letsencrypt\.org/,
      # Crates.io returns a JSON error when curl'd unless an "Accept:
      # text/html" header is sent as well. Unfortunately setting that Accept
      # globally causes two other sites (likely using WAFs?) to forbid the
      # requests. For now, ignore.
      /crates\.io/,
    ],
    :typhoeus => {
      :capath => "/etc/ssl/certs",
      # Libcurl Connection reuse seems flaky on some versions
      # Disabling it outright gives better results
      :forbid_reuse => true,
      # Using a more generous timeout as testing 282 links from a slower
      # internet connection can produce frequent timeouts otherwise
      :timeout => 15,
    }
  }).run
end
