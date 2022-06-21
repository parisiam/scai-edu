<h1>MAINTENANCE</h1>

[TOC]

# Maintenance mode

Maintenance mode is for preventing any users other than administrators from using the site while maintenance is taking place, though *it's not designed to prevent user access during version upgrades*.

Go to `Administration > Server > Maintenance mode`  and set **Maintenance mode** (maintenance_enabled) to Enable.

![manintenance_mode](.img/maintenance/manintenance_mode.jpg)

For more see: https://docs.moodle.org/311/en/Maintenance_mode

# Pause the cron

Cron should normally be enabled, however this setting allows it to be disabled temporarily, for example before a server restart. If disabled, the system is prevented from starting new background tasks. Note that the cron should not be disabled for a long time, as this will prevent important functionality from working.

Go to  `Administration > Server > Tasks > Task processing`  and uncheck **Enable cron** (cron_enabled).

![cron](.img/maintenance/cron.jpg)

# Pause registration

This mode is not a maintenance mode strictly speaking and in this mode, already registered users (students, teachers, admins...) can still access the Moodle and keep working. But new users can't register anymore.

Go to Site `Administration > Plugins > Authentication > Manage authentication`, and set **Self registration** (registerauth) to disable.

![registration](.img/maintenance/registration.jpg)

To resume registration, choose *Email-based self-registration*.

