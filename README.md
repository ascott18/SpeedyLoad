
# This AddOn is Obsolete

Speedy Load is obsolete as of WoW 8.0 - the Battle for Azeroth pre-patch. Blizzard has removed/fixed all of the offending events that fired excessively during loading screens. An update to fix the errors you will encounter in BfA (these errors are due to events that have been removed) will not be released. You should uninstall this addon.
 
## Info

Speedy Load disables certain events during loading screens to drastically improve loading times.

On my own computer, running a state-of-the-art Intel Core i7 2600k processor with 50 addons loaded, Speedy Load has decreased the time taken for my hunter to zone into the Molten Front daily quest area from 5 seconds to 3.6 seconds. That is a 28% increase in speed and a savings of 1.4 seconds! Your results will vary based on class and number of addons, but if you have an older computer, you will almost certainly experience a significant difference.

Speedy Load will decrease the time spent in loading screens for:

* Instances and Raids
* Boats and Zeppelins
* Mage portals
* Cataclysm zone portals
* Summons
* ...And nearly every other loading screen you will ever encounter in-game!

## How it Works

To understand how Speedy Load works and how it is affecting your loading screens, you must first understand the basic workings of the AddOn API. Most AddOns, including the default Blizzard UI, are coded so that certain pieces of code are only executed when certain events occur. In order to see these events, a AddOn must have a frame register an event. Once an event has been registered, an AddOn will execute certain code when an event is fired. There are currently 679 different events that can be registered for, and they can generally be divided into two types: those that include data, and those that do not.

Events that include data with them when they are fired cannot be safely be unregistered because this would cause data to be missed. An example of this is CHAT_MSG_GUILD - the event that fires when somebody says something in your guild's chat. If this event were to be unregistered from every frame during loading screens, you would not see what was said in your guild while you were in the loading screen. It is for this reason that Speedy Load will not unregister any events that may be carrying important data

Events that do not include data can be further broken down into two sub-types: events that trigger something, and events that notify a change. An example of the first sub-type is PVPQUEUE_ANYWHERE_SHOW. This event tells that battleground/arena/war games interface to show, and should not (and will not, by Speedy Load) be unregistered. SPELLS_CHANGED is an example of the second sub-type - it simply notifies the UI that your spells changed, and that it should re-check to see what abilities you have learned and what is available to you.

These types of events (i.e. events without data that only notify the UI of an update) can generally be unregistered during a loading screen, because they often times will fire excessively (more than once) during the loading process without any real purpose beyond the last firing. What Speedy Load does is unregister a number of these non-critical events when you enter a loading screen. When the loading screen completes, these events are re-registered to every frame of every AddOn that had registered them before the loading screen, and then Speedy Load will "fake" the firing of these events only once (and only if they actually occurred during the loading screen) in order to ensure that everything that depends on these events will continue functioning properly.
