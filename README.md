salt-troubleshoot
=================

Quick guide &amp; my notes on troubling shooting salt.

Here are my quick go-to commands for trouble shooting salt issues.

## Gah, getting an error about tabs.

By far, the worst thing is somehow getting tabs in your sls files.
Find any files with tabs with this command, and then in sublime
text 2 do a ```View``` > ```Indentation``` > ```Convert Indentation
to Spaces```.

Command:

```bash
grep $'\t' -r --include "*.sls" .
```

## Running salt-minion in the foreground

If you're running into problems with the salt-minion failing to do
something I find it extremely helpful to run the salt-minion in the
foreground.

```bash
# Stop the minion
service salt-minion stop

# Run the minion in the foreground in debug mode
salt-minion -l debug
```

Then you can watch what happens when commands come across.

## salt-call on the minion

Sometimes its easier to run the commands on the minion while debugging,
so you can use the salt-call functionality:

```bash
salt-call test.ping
salt-call state.highstate
```

## Why are my states not being applied?

Sometimes you run into a problem with seeing what the salt master
is interpreting your SLS files as. I find these commands helpful:

```bash
# Show me the final top file:
salt '*' state.show_top

# Show me a specific salt sls file:
salt '*' state.show_sls example.state
```

Heck, pretty much everything in the 
[state module docs](http://docs.saltstack.com/ref/modules/all/salt.modules.state.html) 
is a gem.

## Pillar items!

Make sure you've refreshed your pillar items.

```bash
# Refresh Pillar Data on Minion
salt '*' saltutil.refresh_pillar
```

Show me my pillar values:

```bash
# List Pillar Values
salt '*' pillar.items
```

Can my minion get the pillar data correctly?

```bash
# From the minion
salt-call pillar.get string:to:pillar:data
```

## Grains!

Okay, what are the actual grain values:
```bash
# List grains
salt '*' grains.ls
```