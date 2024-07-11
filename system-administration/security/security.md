# Security

- This article contains recommendations and best practices for hardening an Arch Linux system.

## 1 Concepts

- Security and convenience must be balanced.

- The biggest threat is, and will always be, the user.

- The principle of least privilege: Each part of a system should only be able to access what is strictly required, and nothing more.

- Defense in depth: Security works better in independent layers.

- Be a little paranoid. And be suspicious.

- You can never make a system 100% secure unless you unplug the machine from all networks, turn it off, lock it in a safe, smother it in concrete and never use it.

- Prepare for failure.

    - Create a plan ahead of time to follow when your security is broken.

## 2 Passwords

### 2.1 Choosing secure passwords

- The tenets of strong passwords are based on *length* and *randomness*.

- Insecure passwords include thos containing or thos using as a base before substitution/variation:

    - Personally identifiable information

    - Simple character substitutions on words, as modern dictionary attacks can easily work with these

    - Root "words" or common strings followed or preceded by added numbers, symbols, or characters

    - Common phrases or short strings of common dictionary words including with character substitution

        - See Diceware below for when a  combination of dictionary of dictionary words can be secure)

    - Any of the most common passwords

- The best choice for a password is something long and generated from a random source.

- It is also very effective to combine the mnemonic and random technique by saving long randomly generated passwords with a password manager, which will be in turn accessed with a memorable "master password"/primary password that must be used only for that purpose.

- The master password must be memorized and never saved.

- This requires the password manager to be installed on a system to easily access the password.

- It can be effective to use a memorable lone series of unrelated words as a password.

- The theory is that if a sufficiently long phrase is used, the gained entropy from the password's length can counter the lost entropy from the use of dictionary words.

- If the set of words you choose from is large (multiple thousand words) and you choose 5-7 or even more random words from it, this method provides great entropy, even assuming the attacker knows the set of possible words chosen from and the number of words chosen.

- Watch out for keyloggers (software and hardware), screen loggers, social engineering, shoulder surfing, and avoid reusing passwords so insecure servers cannot leak more information than necessary.

- Password managers can help manage large numbers of complex passwords: if you are copy-pasting the stored passwords from the manager to the applications that need them, make sure to clear the copy buffer every time, and ensure they are not saved in any kind of log.

- Note that password managers that are implemented as browser extensions may be vulnerable to side channel attacks.

- These can be mitigated by using password managers that run as separate applications.

- It is better to have an encrypted database of secure passwords, guarded behind a key and one strong master password, than it is to have many similar weak passwords.
