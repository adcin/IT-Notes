# My most important passwords:
`JustJoking^-^`
`PasswordInCapitalLettersAndFivesInsteadOf55-WithoutSpacesButWithADash:-`
`onetwothree456:123AsWords&ALLin-capital-letters.Period`

A collection of links and content on the subject of passwords.

- [YouTube.com - How to Choose a Password - Computerphile](https://www.youtube.com/watch?v=3NjQ9b3pgIg)
- [xkcd.com - Password Strength](https://xkcd.com/936/)
- [YouTube.com - Diceware & Passwords - Computerphile](https://www.youtube.com/watch?v=Pe_3cFuSw1E)
- [theworld.com - The Diceware Passphrase Home Page](https://theworld.com/~reinhold/diceware.html)
- [YouTube.com - Have You Been Pwned? - Computerphile](https://www.youtube.com/watch?v=hhUb5iknVJs&t=314s)
- [haveibeenpwned.com - Have I Been Pwned](https://haveibeenpwned.com/)
- [nist.gov - NIST Digital Identity Guidelines](https://pages.nist.gov/800-63-3/sp800-63-3.html)
- [bsi.bund.de - Sichere Passwörter erstellen](https://www.bsi.bund.de/DE/Themen/Verbraucherinnen-und-Verbraucher/Informationen-und-Empfehlungen/Cyber-Sicherheitsempfehlungen/Accountschutz/Sichere-Passwoerter-erstellen/sichere-passwoerter-erstellen_node.html)

# Generate a password hash

In some occasions you may need to generate the hash of a password. For example as an environment variable for a docker container.

### Method 1: `mkpasswd`

You may need to install `mkpasswd` first. On Ubuntu for example it's part of the "whois" package: `sudo apt install whois`

Generate a hash by using `mkpasswd`:
```shell
mkpasswd --method=SHA-512
# or
mkpasswd -m SHA-512
```
_You'll be asked to enter the password and then the hash will be displayed. Use the option `-s, --stdin` to see your password in clear-text while typing: `mkpasswd -m SHA-512 -s`_

> [!quote]- `mkpasswd --help`
> 
> ```
> Usage: mkpasswd [OPTIONS]... [PASSWORD [SALT]]
> Crypts the PASSWORD using crypt(3).
> 
>       -m, --method=TYPE     select method TYPE
>       -5                    like --method=md5crypt
>       -S, --salt=SALT       use the specified SALT
>       -R, --rounds=NUMBER   use the specified NUMBER of rounds
>       -P, --password-fd=NUM read the password from file descriptor NUM
>                             instead of /dev/tty
>       -s, --stdin           like --password-fd=0
>       -h, --help            display this help and exit
>       -V, --version         output version information and exit
> 
> If PASSWORD is missing then it is asked interactively.
> If no SALT is specified, a random one is generated.
> If TYPE is 'help', available methods are printed.
> 
> Report bugs to <md+whois@linux.it>.
> ```

### Method 2: `openssl`

Generate a hash by using `openssl`:
```shell
openssl passwd -6
```
_You'll be asked to enter the password and then the hash will be displayed. The option `-6` lets openssl use SHA512 as the hashing algorithm._

> [!quote]- `openssl passwd -help`
> 
> ```
> Usage: passwd [options] [password]
> 
> General options:
>  -help               Display this summary
> 
> Input options:
>  -in infile          Read passwords from file
>  -noverify           Never verify when reading password from terminal
>  -stdin              Read passwords from stdin
> 
> Output options:
>  -quiet              No warnings
>  -table              Format output as table
>  -reverse            Switch table columns
> 
> Cryptographic options:
>  -salt val           Use provided salt
>  -6                  SHA512-based password algorithm
>  -5                  SHA256-based password algorithm
>  -apr1               MD5-based password algorithm, Apache variant
>  -1                  MD5-based password algorithm
>  -aixmd5             AIX MD5-based password algorithm
> 
> Random state options:
>  -rand val           Load the given file(s) into the random number generator
>  -writerand outfile  Write random data to the specified file
> 
> Provider options:
>  -provider-path val  Provider load path (must be before 'provider' argument if required)
>  -provider val       Provider to load (can be specified multiple times)
>  -propquery val      Property query used when fetching algorithms
> 
> Parameters:
>  password            Password text to digest (optional)
> ```

