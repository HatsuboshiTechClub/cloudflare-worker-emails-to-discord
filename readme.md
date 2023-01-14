# Cloudflare Worker Emails to Discord
Forward emails you receive to Discord

# AGPL
Note this uses the postal-mime library which is AGPL. It's the only decent browser/working in Cloudflare Workers (not dependent on Node) email parsing library that I could find. If you find another one with less restrictive licensing, please let me know!

# How to setup:
Create a webhook to the channel you want your emails in:
https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks

Add it as a secret:

i.e from CLI, you can do: `wrangler secret put DISCORD_WEBHOOK_URL`

Then enter your webhook url, fully qualified with https:// and the domain, for example:

`https://discord.com/api/webhooks/1062548905558016052/uz0cLqaJYtcFp78I5WYsBcc1MOQ4AAX1xMDafVAiuJRqp7sQdWhR1dXkvKx0oADFakSA`

then deploy, i.e wrangler publish. Create the email routes to the worker within the dashboard as you like. Note that a worker running will override any normal forwarding you have on the domain, so you have to use the option below if you want that.

You can also forward to an address if you want, just set the FORWARD_TO_ADDRESS secret the same way to a verified destination address you have. If you want to send to more then one, you can just modify the script.

# Limit

Any emails over the discord embed description size limit will be uploaded as files.

The file will be trimmed (and the name will reflect that, email-trimmed.txt), if it is over 8MB, the default upload limit. 

If you have a boosted server, Level 2 (50MB) or Level 3 (100MB), you can change that constant to allow larger email bodies. 

Note that the current limit of Cloudflare's Postmaster (inbound email service) is 25MB.

# Warning:

There is currently no sanity checks with this. If the email parsing goes above the worker's max memory limits of 128MB or takes too much CPU time to parse, it will just fail and throw an error, which Cloudflare currently handles by rejecting the message entirely, i.e  521 5.3.0 Upstream error, please check

It will still forward your email without issue if you have that enabled, as it tries that first.

# Example of how emails look like in Discord

![Picture of Discord Email Embed](Discord_q6K8tc8Ke1.png "Email Discord Embed")

