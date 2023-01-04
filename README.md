# LNBits Free Lightning Address Extension (Lightning Address - Simple)

The Name of this extension is Lightning Address Simple, and is Powered by LNURL.

The purpose of this extension is to make Free Lightning Addresses available for small groups without requiring the use of cloudflare or paying for the addresses. This extension is an option for anyone who wants to host lightning addresses for a non-production small LNBits install. Some examples may include: friends and family, game, teaching demo, or an experimental app. 


## How to install in 3 easy steps:

Prerequisite: We assume you have a working instance of LNBits already running.

1. Disable the paid Lightning Address extension by copying the settings from .env.example in this repo into your .env
2. Copy the extensions/lnaddy folder from this repo into your LNBits extensions folder installation (lnbits/lnbits/extensions/)
3. Replace the following code in your LNBits installation at lnbits/core/views/public_api.py  with the code from this repo, listed below or found in the core/views/public_api.py file in this repo.


```
@core_app.get("/.well-known/lnurlp/{username}")
async def lnaddress(username: str, request: Request):
    # check if extension is disabled
    if "lnaddress" not in settings.lnbits_disabled_extensions:
        from lnbits.extensions.lnaddress.lnurl import lnurl_response
        domain = urlparse(str(request.url)).netloc
        return await lnurl_response(username, domain, request)
        
    elif "lnaddy" not in settings.lnbits_disabled_extensions:
        from lnbits.extensions.lnaddy.lnurl import lnurl_response
        domain = urlparse(str(request.url)).netloc
        return await lnurl_response(username, domain, request)
```

Restart your LNBits Instance. 

The extension should now be available. 

<img src="https://i.imgur.com/4Gnvwxh.png" width=300/>


That's it. 




## Limitations:
- This is experimental code.
- It does not provide any reverse proxy or nginx setup, the end user is responsible for this.
- This extension will enable addresses at your LNBits domain. For example, if your LNBits is hosted at mybits.com, then Lightning addresses created will be named username@mybits.com
- This extension only works if the other LN Address extension is disabled.


## Notes

- This code basically extends the lnurlp functionality. 
- Pull requests and issues welcome to help improve it. 
- it is a work in progress
