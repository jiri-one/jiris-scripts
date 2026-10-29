# jiris-scripts

Small command-line tools I use on my Linux desktop. Each one is a single file that needs nothing but its interpreter.

## proton-send

`proton-send` sends a plain-text email through [Proton Mail SMTP submission](https://proton.me/support/smtp-submission). It needs only Python 3.

- A dry run shows the message before anything is sent.
- The body goes out as quoted-printable, so long lines arrive exactly as written. That matters for commands in a bug report.

### Setup

1. In Proton Mail, open Settings → All settings → Proton Mail → IMAP/SMTP → SMTP tokens and generate a token for one of your addresses.
   - It must be a custom domain address: Proton has no SMTP for @proton.me, @protonmail.com or @pm.me.
   - SMTP is only available on some paid plans.
2. Store the sender and the token:

   ```sh
   install -d -m 700 ~/.config/proton-smtp
   echo 'Jane Doe <jane@example.org>' > ~/.config/proton-smtp/from
   (umask 077; cat > ~/.config/proton-smtp/token)   # paste the token, then Ctrl-D
   ```

3. Put `proton-send` on your `PATH` and test the login with `proton-send --check`.

### Usage

A draft is a few header lines, an empty line and the body:

```
To: John Smith <john@example.com>
Cc: someone@example.net
Subject: Hello

Hi John,
...
```

```sh
proton-send draft.txt          # show the headers and body that would be sent
proton-send --send draft.txt   # send it
```

- **Headers:** a draft may use To, Cc, Bcc, Reply-To, Subject, In-Reply-To and References.
- **From:** always comes from the configuration, because a token only works for its own address.
- **Addresses:** separate them with commas. The script refuses a header it can't parse rather than drop a recipient.
- **Encryption:** messages sent over SMTP aren't end-to-end encrypted.

## License

GPL-3.0-or-later, see [LICENSE](LICENSE).
