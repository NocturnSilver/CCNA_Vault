#layer3
#subnet

## Classed networks
- Not in use anymore since Classless Inter-Domain Routing allow better flexibility
- common CIDR netmasks include: 
	- /8 = 255.0.0.0 = 11111111 00000000 00000000 00000000
	- /16 = 255.255.0.0 = 11111111 11111111 00000000 00000000
	- /24 = 255.255.255.0 = 11111111 11111111 11111111 00000000
	- /32 = 255.255.255.255 = 11111111 11111111 11111111 11111111

## 

| Class | First Octet | First Octet <br>Numeric Range | Prefix<br>Length | Purpose      | Number<br>Of networks | Addr per<br>Network |
| ----- | ----------- | ----------------------------- | ---------------- | ------------ | --------------------- | ------------------- |
| A     | 0xxxxxxx    | 0-127                         | /8               | Commercial   | 128 (2^7)             | 16,777,216 (2^24)   |
| B     | 1xxxxxxx    | 128-191                       | /16              | Commercial   | 16,384 (2^14)         | 65,536 (2^16)       |
| C     | 110xxxxx    | 192-223                       | /24              | Commercial   | 2,097,152 (2^21)      | 256 (2^8)           |
| D     | 1110xxxx    | 224-239                       |                  | Multicast    |                       |                     |
| E     | 1111xxxx    | 240-255                       |                  | Experimental |                       |                     |
## Classless inter-Domain Routing (CIDR)
- created to solve wasted address spaces
- class address is removed and larger networks could then be split into smaller networks
- the smaller networks are called subnets

## Table of prefix lengths
| prefix length | Number of subnets | Number of Hosts |
| ------------- | ----------------- | --------------- |
| /25           | 2                 | 126             |
| /26           | 4                 | 62              |
| /27           | 8                 | 30              |
| /28           | 16                | 14              |
| /29           | 32                | 6               |
| /30           | 64                | 2               |
| /31           | 128               | 0 (2)           |
| /32           | 256               | 0 (1)           |

## Subnetting Trick
1. segregate between the network bits and the host bits
2. to get the next subnet would be to find the rightmost network bit and use that to keep adding to 255

## Mathematics for Subnets
1. Determine if you have enough addresses for the given address and netmasks $$(hosts\hspace{0.1cm}per\hspace{0.1cm}subnet+2)\hspace{0.1cm}*number\hspace{0.1cm}of\hspace{0.1cm}subnets=total\hspace{0.1cm}number\hspace{0.1cm}of\hspace{0.1cm}addresses$$
2. Determine what subnet netmask to use by checking the hosts per subnet against the number of host bits and the formula below
$$usable\hspace{0.2cm}addresses\hspace{0.2cm}=2^n - 2$$
3. The number of subnets you can make follows the formula $2^n$ where n is the number of borrowed bits. The n would be added to the class network portion to create the appropriate subnet $$2^n$$
## Practice Problems
1. figure out the subnets for 172.16.0.0/16
2. What subnet does host 192.168.5.57/27 belong to?
3. Divide the 192.167.255.0/24 network into five subnet of equal size