# Natas2 (Level 1 -> 2)

  * username: `natas2`
  * password: `TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI`
  * url: `http://natas2.natas.labs.overthewire.org`
  * flag: `3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH`

## Procedure and Thought Process

1. I see a black box with the following text: "There is nothing on this page"
2. I tried `CTRL+U` and saw that there's an image source path of: `files/pixel.png`
  * So I tried http://natas2.natas.labs.overthewire.org/files/ and saw a directory containing pixel.png and users.txt
  * Let's take a look at users.txt:

# username:password
alice:BYNdCesZqW
bob:jw2ueICLvT
charlie:G5vCxkVV3m
natas3:`3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH`
eve:zo4mJWyNj2
mallory:9urtcpzBmH

3. This is a form of `Directory Traversal` or `Insecure Direct Object References (IDOR)`
