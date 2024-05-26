---
banner: TCM Security Course Notes/OSINT Fundamentals/Attachments/asdasd.webp
---
> [!NOTE]
> What is Password OSINT? It is the process by which we or a hacker looks for breached credentials. The information presented in this guide is subject to change.

# Password OSINT - Hunting Breached Passwords Using Dehashed `fas:Key`

## Dehashed `fas:Database` - ![[Pasted image 20240526123852.png]][https://dehashed.com/](https://dehashed.com/)

**Dehashed** is a comprehensive database search engine that allows users to find information from breached databases. This service provides the ability to search for various types of data, including email addresses, usernames, IP addresses, and phone numbers. 

> [!important]
> **Pricing**: Dehashed requires a subscription for access to its full features. Prices vary based on the level of access and number of searches.

![Prices](Pasted%20image%2020240526084043.png)

### Searching with Dehashed

When using Dehashed, you can search by:
- Address
- Email
- Domain
- IP
- Phone Number

For example, if you search for a user such as `bob@tesla.com`, Dehashed will display the associated hashed password. This hashed password can then be used to search for breached credentials on Google by searching the hash directly, like this:

```
"WahtEverTheHasIS"
```

Alternatively, you can enter the hash into the following websites to find the corresponding plaintext password:

- [Hashmob](https://hashmob.net/) `fas:Search`
- [Hashes.org Archive](https://weakpass.com/wordlist/1931) `fas:Archive`

## Useful Links `fas:Link`

- [WEAKPASS Wordlists](https://weakpass.com/) `fas:List`
- [Dehashed](https://dehashed.com/) `fas:Database`
- [Hashmob](https://hashmob.net/) `fas:Search`
- [Hashes.org Archive](https://weakpass.com/wordlist/1931) `fas:Archive`

> [!TIP]
> Regularly update your search methods and tools, as new databases and techniques become available frequently. This ensures you are using the most effective strategies for password OSINT.

By following these steps and utilizing the resources provided, you can effectively hunt for breached passwords and strengthen your security measures.