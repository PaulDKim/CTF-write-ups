# Natas0

  * username: `natas0`
  * password: `natas0`
  * url: `http://natas0.natas.labs.overthewire.org/`
  * flag: `0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq`

## Procedure

1. Presented with text stating: "You can find the password for the next level on this page."
2. Utilize `CTRL+U` or `Right Click + Inspect` in order to view the page source
3. This lab introduces the concept of `Sensitive Data Exposure` which is when developers accidently leave sensitive information in the source code.
4. The password for natas1 is commented in the source code: `<!--The password for natas1 is 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq -->`
