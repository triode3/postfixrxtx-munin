# README 
for the postfixrxtx plugin for graphing the number of emails sent and received in the last five minutes to [Munin](https://munin-monitoring.org/)

This readme is broken into a few small sections: What,  Why, How to install, How to change, License. 

# What is this thing?

This readme assumes you know what [Munin](https://munin-monitoring.org/) is all about, and that you want to graph the number of emails received or sent by postfix.

Here is an exmaple output from munin monitoring only received and delivered mail. The plugin will also monitor rejected, but we decided to leave it out on our system, as we get a lot of rejects.

![screenshot of postfixrxtx-munin from a node](https://github.com/triode3/postfixrxtx-munin/blob/main/images/postfixrxtx-munin.png)

# Why did I make this?

We have an HPC cluster and all emails go through a single gateway. While there are plenty of logs, and even long standing programs ([pflogsumm](https://github.com/sarahkadar/pflogsumm/blob/master/pflogsumm.pl), for example), we just wanted to see a trend. Well, since munin goes off every 5 minnutes, why don't we just read the last 5 minutes of the log file and get sent and received mails and plot that? That is what this (really simple, not 100.0% accurate) plugin does. Think of this as a trending tool and not an exact science (for this plugin at least). 

# How to install it.

It is a plugin just like any munin plugin, so you need to do the following:

- place the postfix_mailrxtx file into /usr/share/munin/plugins (or if you moved it, the munin plugin dir)
- create a link from /etc/munin/plugins/postfix_mailrxtx to /usr/share/munin/plugins/postfix_mailrxtx
- restart munin-node
- restart munin (if you want to). 

Again, if you moved munins plugins dir, change the links to relect this. 

# How to change it, etc. 

- If you want to change the time window, edit postfixrxtx and change ST (SampleTime).
- If you want to remove rejects (like the snapshot above), you can change the plugin:

In the case $1 in config section remove or comment out:

```
rejected.label rejected email
rejected.info Email rejected by postfix
```

And after the received and delivered statements at the bottom, remove or comment out:

```
ejected=$(grep "postfix/smtpd" /var/log/maillog |awk -v d1="$ST" '$0 > d1' |grep reject |wc -l)
echo "rejected.value " $rejected
```

# LICENSE

GNU GENERAL PUBLIC LICENSE (see LICENSE.md)

A copy of the GPL3 is included with this software. (see gpl-3.0.txt)




