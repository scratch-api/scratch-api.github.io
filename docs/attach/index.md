# scratchattach

Scratchattach is a scratch-API wrapper. This directory documents the WIP asynchronous API.

## Logging in

### Logging in with username & password: 

```py
import os
import asyncio
import scratchattach.async_api as sa
from dotenv import load_dotenv

load_dotenv()

async def main():
    async with await sa.login(os.environ["SA_USERNAME"], os.environ["SA_PASSWORD"]) as sess:
        print(sess.user_id)

if __name__ == "__main__":
    asyncio.run(main())
```

### Logging in with session id

```py
import os
import asyncio
import scratchattach.async_api as sa
from dotenv import load_dotenv

load_dotenv()

async def main():
    async with await sa.login_by_id(os.environ["SA_ID"]) as sess:
        print(sess.user_id)

if __name__ == "__main__":
    asyncio.run(main())
```
