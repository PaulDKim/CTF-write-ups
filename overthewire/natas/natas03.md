# Natas3 (Level 2 -> 3)

  * username: `natas3`
  * password: `3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH`
  * url: `http://natas3.natas.labs.overthewire.org`
  * flag: `QryZXc2e0zahULdHrtHxzyYkj59kUxLQ`
  * vulnerability: `sensitive directory exposure`

## Proof of Concept

1. Similar to the last challenge, I see the following text: "There is nothing on this page"
2. This prompts me to check the source code of the challenge and see something interesting: `<!-- No more information leaks!! Not even Google will find it this time... -->`
3. When I navigate to `http://natas3.natas.labs.overthewire.org/robots.txt`, I see the following page:  
User-agent: *  
Disallow: /s3cr3t/
4. While the file disallowed crawlers from accessing the /s3cr3t/ directory `(Disallow: /s3cr3t/)`, it does not stop attackers from directly accessing it if they know the URL.
5. When I navigate to `http://natas3.natas.labs.overthewire.org/s3cr3t/` I see a directive with a `users.txt` file with the following content: `natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ`

## Notes
1. `Google Dorking` can be used to search for publicly exposed files or directories on websites that could be vulnerable to path traversal attacks.  
   * Google dorking is made possible by how search engines, like Google, index and `crawl` the web.
2. `Crawlers` are automated programs used by search engines to visit and index the content of websites.
3. `robots.txt` is a simple text file placed at the root of a website `(e.g., https://example.com/robots.txt)` that provides instructions to `web crawlers` about which parts of the website they are allowed or disallowed to crawl and index.
4. `Example of a basic robots.txt file:`  
User-agent: *  
Disallow: /private/  
Allow: /public/

