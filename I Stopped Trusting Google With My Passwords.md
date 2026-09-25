# I Stopped Trusting Google With My Passwords

## Objective

A couple of days ago, I received an email from Google stating that some of my passwords might have been leaked from certain sites. I got worried and realized that if I had a password vault and kept my passwords secure, I wouldn't be as worried. 

Google's "Password Checkup" checks your saved passwords against known third-party data breach lists. It doesn't mean Google leaked my passwords. The checkup cross-references your saved logins against public breach databases from other companies' security incidents. If a website had your account and got breached and your password showed up in that leaked data, Google flags it. 
*reference https://support.google.com/accounts/answer/9457609?hl=en

That being said, let's stay safe and use best practices. 


## Steps Taken 

1. I'm going to go to this site: "https://passwords.google.com" > Chrome settings > Autofill and passwords> Google Password Manager > Settings Icon > Export Passwords, and save the file and leave it named the default file name.

<img width="1440" height="862" alt="Import Data to Vaultwarden steps 2026-09-25 150054" src="https://github.com/user-attachments/assets/17c2fb24-c871-472f-8640-c6140a67b2b3" />

Vaultwarden gave me more information about Bitwarden and how importing data works. 
<img width="1572" height="862" alt="Bitwarden Export Vault Data Information   2026-09-25 150023" src="https://github.com/user-attachments/assets/3a1b3dc5-9c99-4927-967a-4dc0c3cbebc6" />

2. After logging in to VaultWarden, I selected Tools > Import Data > selected the appropriate file format; in this case, it was CSV > selected the file > Import the Data.

<img width="1440" height="862" alt="Import Data to Vaultwarden steps 2026-09-25 150054" src="https://github.com/user-attachments/assets/0cc5e057-2acb-418f-aee1-01f912827a20" />


3. I made sure it worked, and it did! 


<img width="1598" height="450" alt="Import Successful  2026-09-25 150230" src="https://github.com/user-attachments/assets/4f1ec417-b43b-47cc-b367-422ed729be56" />

4. Something exclusive about VaultWarden is the reports they have and what they do.
  - Exposed Passwords - Passwords exposed in a data breach are easy targets for attackers. Change these passwords to prevent potential break-ins. 
  - Reused Passwords - Reusing passwords makes it easier for attackers to break into multiple accounts. Change these passwords so that each is unique. 
  - Weak passwords - Weak passwords can be easily guessed by attackers. Change these passwords to strong ones using the password generator. 
  - Unsecure websites - URLs that start with http:// don't use the best available encryption. Change the login URLs for these accounts to https:// for safer browsing.
  - Inactive two-step login - Two-step login adds a layer of protection to your accounts. Set up two-step login using Bitwarden Authenticator for these accounts or use an alternative method. 
  - Data Breach - Breached accounts can expose your personal information. Secure breached accounts by enabling 2FA or creating a stronger password. 

This is not a good sign for me. 

<img width="332" height="650" alt="exposed  2026-09-25 150819" src="https://github.com/user-attachments/assets/20cfb0e6-16e6-4632-8329-dadcef3b54cf" />


5. If Exposed Passwords flags something, change that password on the actual site first, then update the entry in Vaultwarden to match. Don't just delete the flagged entry; the exposure is on that website's end. Changing the password is what actually fixes it.
I will do that with every site I know I need or have been using. But I encourage this level of safety in case anything does happen to your personal information. 










































































































































