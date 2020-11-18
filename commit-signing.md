# Commit Signing

From the Git User Manual:

> Git is cryptographically secure, but it’s not foolproof. If you’re taking work from 
> others on the internet and want to verify that commits are actually from a trusted 
> source, Git has a few ways to sign and verify work using GPG.

As Ed-Fi source repositories have embraced the Apache License, it is more important
than ever that we ensure pull requests and commits are well identified. Although
anyone can submit a pull request, we only want to accept the pull request if the
contributor has accepted the [Contributor License Agreement (CLA)](https://gist.github.com/EdFiBuildAgent/d68fa602d07505c3682e8258b7dc6fbc).
Signing Git commits allows us to both verify the identity of the developer and to
verify that the developer has signed the CLA.

## One-Time Setup on Windows

1. Install Gnu Privacy Guard (GPG)

   The simplest way to install GPG is with chocolatey:

   ```
   choco install -y gpg4win
   ```

   Alternately, you can download and install from https://www.gpg4win.org/.

2. Generate a Key

   The default key length is 2048 bit. 4096 is even better. You'll be prompted for
   name and e-mail. You should use the same information that is associated with your
   GitHub account.

   ```
   gpg --default-new-key-algo rsa4096 --gen-key
   ```
   
3. Configure Git to Always Sign

   You will need the key ID for this. In the following example from the Git manual,
   the id is "E1E474F2023B5ABFF8752630BB4".

   ```
   PS C:\source\> gpg --list-keys
   C:/Users/jon.doe/AppData/Roaming/gnupg/pubring.kbx
   ------------------------------------------------
   pub   rsa4096 2020-05-24 [SC] [expires: 2022-04-22]
         E1E474F2023B5ABFF8752630BB4
   uid           [ultimate] Jon Doe <jon.doe@examppppppplllleeeee.com>
   ```
   
   Configure this globally, or set it up one repository at a time by omitting the
   `--global` argument. Additionally, configure the GPG.exe to be used by Git.

   ```
   git config --global user.signingkey E1E474F2023B5ABFF8752630BB4
   git config --global gpg.program "C:\Program Files (x86)\GnuPG\bin\gpg.exe"
   git config --global commit.gpgsign true
   git config --global tag.gpgsign true
   ```

   :eight_spoked_asterisk: TIP: If you would prefer to take manual control of when
   to sign a commit or tag, you can skip the the `commit.gpgsign` and `tag.gpgsign`
   configurations above. To sign a tag, add flag `-s`. To sign a commit, add flag 
   `-S`. Yes, the difference in capitalization is critical.

   With the configuration settings above, you have no need to add the `s/S` flag.

4. Upload the Key to GitHub

   Export the key using that same key id from above.

   ```
   gpg --armor --export 0A46826A
   ```

   This will display your PGP Public Key Block. Copy the text, beginning with 
   `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`.

   Open https://github.com/settings/keys, click the "New GPGP Key" button, and 
   then paste and save the copied public key.
