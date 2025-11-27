# Cookies

![](https://github.com/user-attachments/assets/0f0f4d56-9809-48ae-a19a-40653810611c)

The scratch website stores a number of cookies in your browser.
These can be found in the 'Application' or 'Storage' section of your [browser's devtools](https://developer.mozilla.org/en-US/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools).
They are officially documented [here](https://mitscratch.freshdesk.com/en/support/solutions/articles/4000219342-cookies-policy)

## scratchsessionsid

The main cookie of significance here is the `scratchsessionsid` cookie, which is used to authenticate requests.
This means that it is required for many account actions - like posting comments, following users, or viewing your unshared projects.

According to the official cookies documentation, the `scratchsessionsid` cookie takes 2 years to expire, which actually means that many scripts use it as their only authentication method (instead of a username and password)
The [scratchattach library](https://github.com/TimMcCool/scratchattach/) provides functionality for logging in directly with a session id, or by generating one using your username and password.

### Content

It actually turns out that the `scratchsessionsid` cookie contains information encoded in a urlsafe-base64 zlib-compressed/base62/hash format.

This is [documented in django](https://github.com/django/django/blob/7b32485ee98edf7e8b94ad9c8acdccee562bf216/django/core/signing.py).

[Python implementation of a sessionid decoder](https://github.com/TimMcCool/scratchattach/blob/d52f5ecbcb86a072ea9325cf255811dc2711c08f/scratchattach/site/session.py#L1130-L1156)

There are 3 sections of the cookie, separated by colons. We can name these p1, p2, p3.

#### p1

This contains most of the data in the session id. It is actually a JSON object, which has been stringified, then zlib-compressed, then urlsafe base64 encoded (with padding removed).
Do note that this part is actually only zlib compressed if it begins with a `.` symbol. In all cases in scratch so far, it has not been observed, but you may want to keep this in mind.

In python, it can be decoded with `zlib`, `base64`, and `json` like so: `json.loads(zlib.decompress(base64.urlsafe_b64decode(p1+"==")))`

Output (partially redacted):
`{'username': 'faretek0', 'token': '<some hash>:<some other hash>', 'login-ip': '123.45.678.910', '_auth_user_id': '160388196', 'django_timezone': 'America/New_York', '_auth_user_hash': '<a hash value>', 'testcookie': 'worked', '_auth_user_backend': 'authentication.backends.ReplicationBackend'}`

You would notice that this contains certain information (your IP) which you likely do not want to be public.
It could be considered more dangerous to have a leaked session-id compared to a leaked password, from a privacy standpoint - although there are other concerns here, like the fact that that one can view an email address of an account given the username and password.

#### p2

This is a [base62-encoded](https://github.com/TimMcCool/scratchattach/blob/d52f5ecbcb86a072ea9325cf255811dc2711c08f/scratchattach/utils/commons.py#L256-L263) unix timestamp of the login time. 
To convert this to a python datetime object, use `datetime.fromtimestamp`.

### p3

This is probably some authentication hash of some kind.
