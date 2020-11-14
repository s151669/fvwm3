# NAME

*FvwmMFL* - the Fvwm3 front-loader module

# SYNOPSIS

*FvwmMFL* can only be invoked by Fvwm3. Command line invocation of the
*FvwmMFL* will not work.

This module has no command-line options.

# DESCRIPTION

The *FvwmMFL* module provides access to Fvwm events over a unix-domain
socket. This module is intended to provide externally-written programs
(clients) the ability to receive information from Fvwm and to perform an
action on that event.

The information from Fvwm3 is in the form of JSON packets. Each JSON
packet has different fields, depending on the type requested.

# COMMUNICATION

The default unix-domain socket for *FvwmMFL* is
*$TMPDIR/fvwm\_mfl.sock*, although this can be overriden via an
environment variable *FVWMMFL\_SOCKET*.

# REGISTERING INTEREST

Commands can be sent to *FvwmMFL* to control which information is sent
the client. The *set* command is used for this. The table below shows
which events can be subscribed to.

<table>
<tbody>
<tr class="odd">
<td style="text-align: left;"><em>Event</em></td>
<td style="text-align: left;"><em>Description</em></td>
</tr>
<tr class="even">
<td style="text-align: left;">new_window</td>
<td style="text-align: left;">Fired when a new window is mapped and visible.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">map</td>
<td style="text-align: left;">Fired when a window is mapped.</td>
</tr>
<tr class="even">
<td style="text-align: left;">configure_window</td>
<td style="text-align: left;">Fired when a window is moved or resized.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">destroy_window</td>
<td style="text-align: left;">Fired when a window is closed.</td>
</tr>
<tr class="even">
<td style="text-align: left;">new_page</td>
<td style="text-align: left;">Fired when a new page is switched to.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">new_desk</td>
<td style="text-align: left;">Fired when a new desk is switched to.</td>
</tr>
<tr class="even">
<td style="text-align: left;">raise_window</td>
<td style="text-align: left;">Fired when a window is raised (or changes layer).</td>
</tr>
<tr class="odd">
<td style="text-align: left;">lower_window</td>
<td style="text-align: left;">Fired when a window is lowered (or changed layer).</td>
</tr>
<tr class="even">
<td style="text-align: left;">focus_change</td>
<td style="text-align: left;">Fired when a window loses/gains focus.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">enter_window</td>
<td style="text-align: left;">Fired when a window has the pointer moved into it.</td>
</tr>
<tr class="even">
<td style="text-align: left;">leave_Window</td>
<td style="text-align: left;">Fired when a window has pointer moved out of it.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">window_shade</td>
<td style="text-align: left;">Fired when a window is shaded.</td>
</tr>
<tr class="even">
<td style="text-align: left;">window_unshade</td>
<td style="text-align: left;">Fired when a window is unshaded.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">window_name</td>
<td style="text-align: left;">Fired when the window name changes.</td>
</tr>
<tr class="even">
<td style="text-align: left;">visible_name</td>
<td style="text-align: left;">Fired when the visible window name changes.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">res_class</td>
<td style="text-align: left;">Fired when the class of the window is set.</td>
</tr>
<tr class="even">
<td style="text-align: left;">res_name</td>
<td style="text-align: left;">Fired when the resource of the window is set.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">iconify</td>
<td style="text-align: left;">Fired when a window is iconified.</td>
</tr>
<tr class="even">
<td style="text-align: left;">deiconify</td>
<td style="text-align: left;">Fired when a window is deiconified.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">icon_name</td>
<td style="text-align: left;">Fired when the icon name changes.</td>
</tr>
<tr class="even">
<td style="text-align: left;">visible_icon_name</td>
<td style="text-align: left;">Fired when the icon's visible name changes.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">icon_file</td>
<td style="text-align: left;">Fired when the path to the icon changes.</td>
</tr>
<tr class="even">
<td style="text-align: left;">icon_location</td>
<td style="text-align: left;">Fired when the icon location changes.</td>
</tr>
<tr class="odd">
<td style="text-align: left;">restack</td>
<td style="text-align: left;">Fired when the window stacking order changes.</td>
</tr>
<tr class="even">
<td style="text-align: left;">echo</td>
<td style="text-align: left;">Fired when Fvwm receives an Echo command.</td>
</tr>
</tbody>
</table>

For example, to register an interest in *new\_window* and
*focus\_change*, the following commands would be set via the socket:

*set new\_window* *set focus\_change*

To remove interest in an event, use the *unset* command:

*unset focus\_change*

# JSON FORMAT

Each packet sent to a client is in plain JSON. The information contained
in each packet varies depending on the event.

TODO: document each JSON structure.

# EXAMPLE

The following example shows how to monitor for *focus\_change* events at
the shell, printing the JSON returned:

*echo set focus\_change | nc -U /tmp/fvwm\_mfl.sock 2&gt;&1 | jq
--unbuffered .*

Outputs:

{ "focus\_change": { "window": "0x5400022", "type": 0, "hilight": {
"text\_colour": 16777215, "bg\_colour": 32767 } } }

# AUTHORS

This module first appeared in 2020.
