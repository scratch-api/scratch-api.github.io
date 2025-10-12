If you want your Python code to run 24/7, you'll either need to use a hosting service or host locally. This page lists different hosting services. If you know a hosting option that isn't listed here, open an issue to ask for it to be added.

We have [a YouTube video](https://www.youtube.com/watch?v=lPYJu3crgGg) that also walks through how to use some of these services.

# Free services

## 1. sillydev

**[Link: panel.sillydev.co.uk](https://panel.sillydev.co.uk/)**

**How to use:** [Video tutorial](https://youtu.be/lPYJu3crgGg?t=426)

Best if you need lots of scripts running 24/7.

## 2. Back4app

**[Link: back4app.com](https://back4app.com)**

A platform offering Container as a service (CaaS). This allows you to use any coding language.
- On free plan, you get 600 free deploy hours per month. This means your app will run for 25 days per month.
- Ease of setup: 6/10

**How to use:**

To get started, fork this template on GitHub: https://github.com/templates-back4app/containers-python-flask-sample/blob/main/app.py

Edit your Python code so that the lines executing your backend server (like `client.run()` or `events.start()`) are inside a `def run():` function.

Then, add a `backend_code.py` file and paste your Python code to it. Go to the `app.py` file and add this code to line 2:

```py
from threading import Thread
import backend_code
Thread(target=backend_code.run).start()
```

After you did that, go to back4app, create a new app and select "Container". Back4app will ask you to log in with GitHub. Select the repository fork you created before.
If everything was done correctly, your code should be executed as soon as the container has started.

**Warning:**

Your app will only stay awake if you ping it using a pinger like uptimerobot.com.

# Paid services

## 1. Replit

**[Link: replit.com](https://replit.com)**

Offers different tiers for hosting Python code. Very easy to set up. However, replit's pricing has skyrocketed over the last two years and is now pretty expensive. 

## 2. IONOS VPS

**[Link: ionos.com/servers/vps](https://www.ionos.com/servers/vps)**

Offers cheap VPS (Virtual private servers) for hosting any kind of code. The lowest tier offered is enough for hosting scratchattach backends. However, a VPS is way harder to set up than other hosting options.

*Not sponsered. The `Easy of use` rating takes factors like the time required to set up into account, but isn't purely objective.*

# More

<https://dash.huguitisnodes.host/register>
Video timestamp: [0:04](https://youtu.be/lPYJu3crgGg?t=4)
> best for running 1-2 scripts, no renewal required!


<https://bot-hosting.net/>
Video timestamp: [3:47](https://youtu.be/lPYJu3crgGg?t=227)
> Second best for running one or two scripts for a long time - requires you to earn credits. You can earn enough credits in one day to run your server for a week.

<https://panel.sillydev.co.uk/>
Video timestamp: [7:06](https://youtu.be/lPYJu3crgGg?t=426)
> Best if you need lots of scripts running 24/7

<https://www.hidencloud.com/>
Video timestamp: [9:37](https://youtu.be/lPYJu3crgGg?t=577)
> Lets you run one script and requires renewal once a week.

<https://botcore.org/>
Video timestamp: [14:29](https://youtu.be/lPYJu3crgGg?t=869)
> Similar to sillydev, but required frequent renewals and server resources are expensive

<https://skycastle.us/>
