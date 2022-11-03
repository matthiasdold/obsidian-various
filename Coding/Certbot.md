### General notes
On chronicler, I had to use the `--apache` flag with the `certbot renew` command to have it run through. Otherwise an unexpected error with port `80:` binding appeared

```bash
 ... produced an unexpected error: Problem binding to port 80: Could not bind to IPv4 or IPv6.. Skipping.
```

Working for me was:
```bash
sudo certbot renew --apache
```

Which I tested with the `--dry-run` flag before.


### Adding the renewal as automatic policy

Should be the cron under `etc/cron.d/certbot
Not sure if the config in this file on chronicler is correct however.