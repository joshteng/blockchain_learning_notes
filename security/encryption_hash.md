3 Common Strategies

1. Symmetric Encryption
This is useful when you want to be able to encrypt and decrypt a string or text. Common algorithm includes AES. It takes a key to be used to encrypt or decrypt the string / text.

2. Public-key Encryption

3. Hashing
Hashing algorithms such as MD5, SHA are used to convert input data into a seemingly random string called hash. There is no way to reverse these hashes into it's original format. This is commonly used for passwords. However, hashing alone is simply not secure for some applications such as user passwords. Hackers may choose to use one of 3 common ways to discover the users password namely: 1. Rainbow Table attack, 2. Dictionary attack, 3. Brute force attack.
Rainbow Table is essentially a key-value pair of passwords and corresponding hashes for given hashing algorithms.
Dictionary attack is basically a list of possible passwords and it can be used iteratively to test if the password is correct.
Brute force attack is basically going through combinations of characters until the correct one is found.
Hence, to make it just a little more difficult for hackers, we can use a salt. A salt can be thought of as unique strings that are blended with the password and the new blended password can be hashed. The unique salt needs to be stored in the database to be used when checking if the user keyed in the right password. Most developers store this hash in plain-text, so if a database gets hacked, there is still a possibility of the hacker guessing what the plain text password is through brute force or dictionary attack provided they can also guess where the salt is blended with the password. To make it even more secured, pepper can be used. Peppers are list of known character(s) that are similarly blended to plain text passwords before being hashed. Peppers are not stored in the database and so it increases the barrier even further for hackers. The downside of peppers is the additional computation time it takes to verify a users' password(s).

The best practise for passwords would still be to require users to key in longer unique passwords, not made up of words in dictionaries and then using salt or encrypted salt. This significantly reduces the risk of a successful dictionary or brute force attack. Other best practices such as preventing the number of retries a user can if the wrong password was keyed in wrongly.