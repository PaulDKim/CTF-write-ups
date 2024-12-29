# Natas1 (Level 0 -> 1)

  * username: `natas1`
  * password: `0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq`
  * url: `http://natas1.natas.labs.overthewire.org/`
  * flag: `TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI`
  * vulnerability: `sensitive data exposure` 

## Proof of Concept

1. We see text stating: "You can find the password for the next level on this page but rightclicking has been blocked!"
  * Right clicking is not the only way to view source code of a web page. We can utilize `CTRL+U`
2. Just like the last challenge, we are able to view the password to the next level through the source code: `<!--The password for natas2 is TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI -->`
