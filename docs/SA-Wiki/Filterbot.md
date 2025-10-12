Filterbot framework that can be used to automatically delete spam comments. You can either use pre-made filter profiles (like _f4f filter_ or _advertising filter_) or set up your own, custom filter rules. 

**Note:** Since you don't receive messages for your own comments, commenty posted by you are not deleted by the filterbot.

# Basic usage

```py
import scratchattach as sa
from scratchattach import HardFilter, SoftFilter, SpamFilter

session = sa.login("username", "password")
filterbot = session.connect_filterbot(log_deletions=True)

@filterbot.event
def on_ready():
    print("Filterbot ready")

filterbot.add_f4f_filter() # Adds pre-made f4f filter profile
filterbot.add_ads_filter() # Adds pre-made advertising filter profile
filterbot.add_spam_filter() # Adds pre-made spam filter profile

filterbot.start()
```

# Advanced

It's possible to define your own filters. When you receive a comment that violates one of your filters, it will automatically be deleted.

There are three types of filters:

## HardFilter

If a comment violates one of the hard filters, it will always be deleted (no matter the results of the other filters).

**How to create and add a hard filter:**

```py
hard_filter = HardFilter("filter name", equals=None, contains=None, author_name=None, project_id=None, profile=None, case_sensitive=False)
filterbot.add_filter(hard_filter)
```

**Keyword arguments of the HardFilter constructor:**

Generally: Arguments are None by default. Setting an argument to None basically means that this filter criterium is disabled. The filter criteriums are AND-connected, meaning the filter will only be violated if _all_ of the conditions (defined by the arguments) are met. For OR-connections, just create multiple filters.

- equals: The filter is violated if the comment content equals the string this field is set to
- contains: The filter is violated if the comment content contains the string this field is set to. Tip: Set `contains` to "" to filter all comments (because `string contains ""` is always True)
- author_name: The filter is violated if the comment is from a specific author
- project_id: The filter is violated if the comment was posted on a specific project
- profile: The filter is violated if the comment was posted on a specific profile

Through the `case_sensitive` argument, you can specify whether checking the `equals` and `contains` criteria should be case-sensitive or not.

## SoftFilter

If a comment violates a spam filter, the score value of the soft filter will be added to the comment's total violation score (initially 0.0). If this total score is at least 1.0 after all filters were applied, the comment will be deleted.

**How to create and add a soft filter:**

```py
soft_filter = SoftFilter("filter name", score_value, equals=None, contains=None, author_name=None, project_id=None, profile=None, case_sensitive=False)
filterbot.add_filter(soft_filter)
```

## SpamFilter

Spam filters are hard filters that only delete a comment if it is the 2nd matching comment that was posted in less than 5 minutes.

**How to create and add a spam filter:**

```py
spam_filter = SpamFilter("filter name", equals=None, contains=None, author_name=None, project_id=None, profile=None, case_sensitive=False)
filterbot.add_filter(spam_filter)
```