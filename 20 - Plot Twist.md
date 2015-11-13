# 20 - Plot Twist

#### Overview

Utilize your browser's javascript console

#### Problem

We need to get the flag at this site. That shouldn't be too hard.

#### Hint

There must be a backup on that site SOMEWHERE . . . you just have to look harder

#### Solution

For a 20 point problem, it would reasonable to assume that the flag would be hidden in an obvious place.  We first try to view the source of the website, but we see the following comment:

    <!-- you thought the flag would be in the comments didn't you? nice try we're better than that -->

We then proceed to check the javascript console (F12 for chrome).  We find the following message:

    no one would EVER think to look in the console! flag backup: easyctf{remember_to_check_everywhere}

The flag is indeed logged in the console.

#### Flag
    easyctf{remember_to_check_everywhere}
