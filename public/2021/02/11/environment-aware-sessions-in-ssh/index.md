# Environment Aware Sessions in SSH

In my first week on the job as a software developer, I deleted an entire table's worth of data from a client's database. Luckily, we had a backup from the night before and that table was low traffic. What happened was that I _thought_ I was on the staging server and I was doing some testing and needed to clear out the existing data. I was not on staging. I was on production. I learned a valuable lesson and I was determined never to make the same mistake again.

Enter, color coded SSH sessions:

{{< figure src="/wp-content/uploads/sites/2/2021/02/EB9DL-%5FXkAEbAYp.png" alt="" caption="" >}}

I developed color schemes for different environments that would easily stand out and alert me of where I was.

For my local environment, I use a black background with green text. This is part Matrix and part me not having a computer growing up in the 90s and missing out on the nostalgia of pre-GUI terminals. On staging, I use a blue/violet mix that isn't jarring to the eye or mind but still catches my attention and makes me aware of the environment. Lastly, for production-based servers I use a red background which definitely stands for "ALERT, whatever you do will have consequences!"

To color code the environments I use separate profiles within [iTerm](https://iterm2.com/documentation-dynamic-profiles.html). Once I customized the profiles to my liking, I needed a way to easily and dynamically switch between them. I did some googling and came across a bash function on [StackOverflow](https://stackoverflow.com/a/53683101) called it2prof. Essentially this function is a shortcut to an iTerm command to switch profiles but the function is a lot cleaner to use: `it2prof staging`. That command will then switch the current iTerm window's profile to the profile and color scheme of my "staging" profile.

To connect to an environment, I use shortcuts and aliases to run my commands at once, for instance: `ssh staging` aliases to `it2prof staging && ssh steven@stage01` which will change the terminal profile and then ssh into my staging server.

This level of safe guarding will likely do for most people, however, I am paranoid and now I have multiple production environments to deal with. For instance, at work, we use a control box to do our releases, but we also have individual production servers that you can log into. So I wanted an extra notification of where I was to pop up.

{{< figure src="/wp-content/uploads/sites/2/2021/01/staging-1024x383.png" alt="" caption="" >}}

{{< figure src="/wp-content/uploads/sites/2/2021/01/control-box-1024x148.png" alt="" caption="" >}}

{{< figure src="/wp-content/uploads/sites/2/2021/01/production-1024x174.png" alt="" caption="" >}}

Text to ASCII! I wanted a proper warning message to pop up and be as in my face as possible to let me know which boxes I was on. I used [this tool](http://ascii.mastervb.net/) to convert my text to ASCII art which I then saved to text files either on my local machine or the remote server.

If you have your own user on a server you could do like what I did on the staging environment and update the .bashrc file to output the contents of a file when your Bash profile is loaded (upon log in). To do this, update your .bashrc file with a line like `cat env_msg.txt` where "env\_msg.txt" is the text file containing your ASCII art.

My production servers use different user permissions so instead, I stored those warning files locally and just updated my aliases. For work, I created a `fub` (short for [Follow Up Boss](https://www.followupboss.com/)) function that helps me do various things, one of them being ssh-ing into a server. I simply updated each SSH command with a call to `cat` my text file like so: `it2prof control && cat /Users/steven/production-warning.txt && ssh production01`. Now all I have to type into my terminal is `fub ssh prod01` and my terminal profile turns red, I get the boldface production warning, and I'm ssh'd into the proper machine.

Now, I'm always aware of what machine I'm working on and I haven't made the same mistake again as I did all those years ago.

Do what you need to to guard/help yourself. Remember, stay safe out there.


---

> Author: <no value>  
> URL: http://localhost:1313/2021/02/11/environment-aware-sessions-in-ssh/  

