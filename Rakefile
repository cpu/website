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
      # Do not check the 'certificateautomation.com' website - it has a broken
      # TLS configuration (missing intermediate)
      /certificateautomation\.com/,
      # TLS 1.2 only sites are currently broken
      # TODO(@cpu): figure out how to upgrade Curl in CI
      /www\.froxlor\.org/,
      /kristaps\.bsd\.lv/,
      # Mojzis.com is failing with "SSL connect error", unclear why
      # TODO(@cpu): diagnose mojzis.com TLS error
      /mojzis\.com/,
    ],
    :typhoeus => {
      # Without specifying the CA Path all TLS verification will fail
      :capath => "/etc/ssl/certs",
      # Libcurl Connection reuse seems flaky on some versions
      # Disabling it outright gives better results
      :forbid_reuse => true,
      # Using a more generous timeout as testing 282 links from a slower
      # internet connection can produce frequent timeouts otherwise
      :timeout => 15,
      # Without specifying a browser-like Accept header `crates.io` will return
      # a JSON API error
      :headers => { :Accept => "text/html, application/xml, */*" },
    }
  }).run
end