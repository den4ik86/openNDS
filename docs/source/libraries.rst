Library Utilities
=================

Overview
********

A number of library utilities are included. These may be used by NDS itself, FAS and Preauth. These may in the future, be enhanced, have additional functionality added.

By default, library utilities will be installed in the folder

``/usr/lib/opennds/``

List of Library Utilities
*************************

get_client_interface.sh
#######################
This utility allows the interface a client is using to be determined from the client mac address.

It can be used in ThemeSpec and local FAS scripts.

Its output is also sent to FAS in the encrypted query string as the variable "clientif"

  Usage: get_client_interface.sh [clientmac]

  Returns: [local_interface] [meshnode_mac] [local_mesh_interface]

  Where:

    [local_interface] is the local interface the client is using.

    [meshnode_mac] is the mac address of the 802.11s meshnode the client is using (null if mesh not present).

    [local_mesh_interface] is the local 802.11s interface the client is using (null if mesh not present).

libopennds.sh
#############
This utility controls many of the functions required for PreAuth/ThemeSpec scripts.

  Usage: libopennds arg1 arg2 ... argN

clean:
------
    **arg1**: "*clean*", removes custom files, images and client data

    *returns*: tmpfsmountpoint (the mountpoint of the tmpfs volatile storage of the router.

tmpfs:
------
    **arg1**: "*tmpfs*", finds the tmpfs mountpoint

    *returns*: tmpfsmountpoint (the mountpoint of the tmpfs volatile storage of the router.

mhdcheck:
---------
    **arg1**: "*mhdcheck*", checks if MHD is running (used by MHD watchdog)

    *returns*: "1" if MHD is running, "2" if MHD is not running

gatewaymac:
-----------
    **arg1**: "*gatewaymac*", finds the mac address of an interface.

        **arg2**: ifname

    *returns*: mac address of the interface

gatewayroute:
-------------
    **arg1**: "*gatewayroute*", checks for a valid gateway route for an interface.

    *returns*: the ip gateway address and the ip gateway interface, or "offline" if the router is offline

clientaddress:
--------------

    arg1: "clientaddress", find and return both ip and mac of a client

        arg2: contains either client mac or client ip

    *returns*: client ip and mac

rmcid:
------

    arg1: "rmcid", Remove an existing cidfile (client identifcation file)

        arg2: contains the cid (client id)

        arg3: contains mountpoint of the ndscids database

    *returns*: "done"

write:
------

    arg1: "write", Write client info element to cidfile (client identifcation file)

        arg2: contains the cid (client id)

        arg3: contains mountpoint of the ndscids database

        arg4: contains info element

    *returns*: "done"

parse:
------

    arg1: "parse", Parse for sub elements and write to cidfile (client identification file)

        arg2: contains the cid (client id)

        arg3: contains mountpoint of the ndscids database

        arg4: contains info elements to parse

    *returns*: "done"

download:
---------

    arg1: "download", Download files required for themespec

        arg2: contains the themespec path

        arg3: contains the image list

        arg4: contains the file list

        arg5: contains the refresh flag, set to 0 to download if file missing, 1 to refresh downloads, 3 to skip downloads

        arg6: contains the webroot

    *returns*: "done"

get_option_from_config:
-----------------------

    arg1: "get_option_from_config", Get the config option value

        arg2: contains the option to get

    *returns*: the requested config option, or an empty string if not configured

debuglevel:
-----------

    arg1: "debuglevel", Sets the debuglevel for externals

        arg2: contains the debuglevel

    *returns*: the debuglevel

get_debuglevel:
---------------

	arg1: "get_debuglevel", Gets the debuglevel set for externals

    *returns*: the debuglevel

syslog:
-------

	arg1: "syslog", Write a debug message to syslog

	    arg2: contains the string to to write to syslog if enabled by debuglevel

	    arg3: "debugtype" contains debug type: debug, info, warn, notice, err, emerg.

    *returns*: "done"

startdaemon:
------------

    arg1: "startdaemon", Start a daemon process

        arg2: contains the b64 encoded daemon startup command

    *returns*: pid of the daemon and exit code 0 if successful. If unsuccessful or terminated immediately returns "0" or a status information string with code 1

stopdaemon:
-----------

    arg1: "stopdaemon", Stop a daemon process

        arg2: contains the pid of the daemon to stop

    *returns*: "done" if sucessful or "nack" with exit code 1 if unsuccessful

get_interface_by_ip:
--------------------

    arg1: "get_interface_by_ip", get the interface name that is the gateway for an IP address

        arg2: contains the ip to check

    *returns*: Interface name with exit code 0, or exit code 1 if failed to get interface name

write_log:
----------

    arg1: "write_log", write a string to the openNDS log

        arg2: contains the string to log

    *returns*: "done"

dhcpcheck:
----------

    arg1: "dhcpcheck", Checks if an ip address was allocated by dhcp

        arg2: contains the ip to check

    *returns*: The mac address that was allocated to the ip address or null and exit code 1 if not allocated

deauth:
-------

	arg1: "deauth", deauthenticates a client by ip or mac address

        arg2: contains the ip or mac address

    *Can NOT be called from a binauth script*


    *returns*: the status of the deauth request

daemon_deauth:
--------------

    arg1: "daemon_deauth", initiates a daemon process to deauth a client by ip or mac address

        arg2: contains the ip or mac address

    *Can be called from a binauth script*

    *returns* the pid of the daemon_deauth process

    The actual client deauth will be reported in the syslog if successful

urlencode:
----------

    arg1: "urlencode", urlencode a string

        arg2: contains the string to be encoded

    *returns* the encoded string

urldecode:
----------

    arg1: "urldecode", urldecode a string

        arg2: contains the string to be decoded

    *returns* the decoded string

send_to_fas_deauthed:
---------------------

**Note:** This library function is used by the default binauth_log.sh script. The default remote FAS script fas-aes-https.php writes the received deauthentication data to a deauth log.

    arg1: send_to_fas_deauthed, Sends deauthed notification to an https fas
        arg2: contains the deauthentication log.

    The deauthentication log is of the format:

``method=[method], clientmac=[clientmac], bytes_incoming=[bytes_incoming],
bytes_outgoing=[bytes_outgoing], session_start=[session_start],
session_end=$6, token=[token], custom=[custom data as sent to binauth]``

Returns exit code 0 if sent, 1 if failed

send_to_fas_custom:
-------------------

    arg1: send_to_fas_custom, Sends a custom string to an https fas
        arg2: contains the string to send

    The format of the custom string is not defined, so is fully customisable.

Returns exit code 0 if sent, 1 if failed

users_to_router:
----------------

    arg1: users_to_router, sets allow or passthrough mode for users_to_router rules.

        arg2: the mode to set (ie allow, passthrough or cleanup)


    Passthrough facilitates chaining to lower priority nftables tables/chains (eg FW4 in OpenWrt)

Returns exit code 0 if set, 1 if failed

pre_setup:
----------

    arg1: pre_setup, creates/configures openNDS nftables base chains

Returns exit code 0 if successful, 1 if failed

delete_chains:
--------------

    arg1: delete_chains, deletes the openNDS nftables base chains

Returns exit code 0 always

delete_client_rule:
-------------------

    arg1: delete_client_rule, deletes a client rule

        arg2: is the table

        arg3: is the chain

        arg4: is the verdict - accept, drop, queue, continue, return, jump, goto or all

        arg5: is the client ip address

Returns exit code 0 if successful

replace_client_rule
-------------------

    arg1: replace_client_rule, deletes a client rule

        arg2: is the table

        arg3: is the chain

        arg4: is the verdict - accept, drop, queue, continue, return, jump, goto or all

        arg5: is the client ip address

Returns exit code 0 if successful

nftset
------

    arg1: nftset, Creates walledgarden nftset

        arg2: is add, insert or delete the rule

        arg3: is the nftset name

Returns exit code 0 if successful

pad_string
----------

    arg1: pad_string, pads a string to a length given by the length of the pad string, with extra characters from the pad string.

        arg2: is the hand, ie "left" or "right"

        arg3: is the pad string eg "1234567890"

        arg4: is the string to pad

Returns exit code 0 if successful

write_to_syslog
---------------

    arg1: write_to_syslog, write debug message to syslog

        arg2: contains the string to log

        arg3: contains the debug level string: debug, info, warn, notice, err, emerg.

Returns exit code 0 if successful

check_heartbeat
---------------

    arg1: check_heartbeat, check the openNDS heartbeat

Returns the heartbeat status string and exit code 0 if alive, 1 if dead

auth_restore
------------

    arg1: auth_restore, restore the authentication of clients after a restart.

        Reads the authenticated client database if created by Binauth

Returns exit code 0 always

configure_log_location
----------------------

    arg1: configure_log_location, configure the log location

Returns the directory into which log files should be stored and exit code 0 if successful

is_nodog
--------

    arg1: is_nodog, check if nodogsplash is installed

Returns string nodog_yes and exit code 0 if it is, nodog_no and exit code >0 if it is not

generate_key
------------

    arg1: generate_key, generate a pseudo-random hexadecimal key value

Returns the pseudo-random hexadecimal key value

set_key
-------

    arg1: set_key, adds option faskey to config file

    arg2: the key to set

Returns exit code 0 always

hash_str
--------

    arg1: generates a hash from the string supplied

    arg2: the string to hash

Returns the hashed string and exit code 0 if successful

wget_request
------------

    arg1: wget_request, send a request to a remote fas url

    arg2: The url we want to send the request to

    arg3: The requested action

    arg4: The gatewayhash of this router

    arg5: The user agent to send to the remote fas

    arg6: The payload to send to the fas

Returns the reply to the request from the remote fas and exit code 0 if successful

preemptivemac
-------------

    arg1: preemptivemac, parses the preemptivemac list and authenticates clients in that list

    arg2: optional client mac address to immediately pre-emptively authenticate instead of parsing the list

Returns exit code 0 always

resolve_fqdn
------------

    arg1: resolve_fqdn, get the first ip address to resolve from a DNS query to the fqdn

Returns the resolved ip address or an empty string and returns exit code 0 always

config_input_fields
-------------------

    arg1: config_input_fields, configure custom input and hidden (passthrough) fields from fas_custom_variables_list

Returns html code for custom inputs and custom passthrough. Returns exit code 0 always.

get_meshnode_list
-----------------

    arg1: get_meshnode_list, get a list of known mesh11sd meshnodes

Returns a list of meshnode mac addresses. Returns exit code 0 always.

get_next_preemptive_auth
------------------------

    arg1: get_next_preemptive_auth, gets the auth string for the next client to reauthenticate after a restart. Deletes that clients record from the preemptive_auth database.

Returns the auth string for the client. Returns exit code 0 always.

?fas:
-----
    arg1: "*?fas=<b64string>*", generates ThemeSpec html using b64encoded data sent from openNDS

        arg2: urlencoded_useragent_string

        arg3: mode (1, 2 or 3) (this is the mode specified in option login_option in the config file.

        arg4: themespecpath (if mode = 3)


Returns html for the specified ThemeSpec.
