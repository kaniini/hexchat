# HexChat Perl Interface

(This file is currently not converted properly to Markdown format.)

<h2><a name="introduction" />Introduction</h2>
<p>This is the new Perl interface for X-Chat 2.  However, due to changes in
xchat's plugin code you will need xchat 2.0.8 or above to load this.  Scripts
written using the old interface will continue to work. If there are any
problems, questions, comments or suggestions please email them to the address
on the bottom of this page.</p>
<p>
</p>
<h2><a name="constants" />Constants</h2>
<p>
</p>
<h3><a name="priorities" />Priorities</h3>
<ul>
<li><strong><code>Xchat::PRI_HIGHEST</code></strong>
</li>
<li><strong><code>Xchat::PRI_HIGH</code></strong>
</li>
<li><strong><code>Xchat::PRI_NORM</code></strong>
</li>
<li><strong><code>Xchat::PRI_LOW</code></strong>
</li>
<li><strong><code>Xchat::PRI_LOWEST</code></strong>
</li>
</ul>
<p>
</p>
<h3><a name="return_values" />Return values</h3>
<ul>
<li><strong><code>Xchat::EAT_NONE</code>     - pass the event along</strong>
</li>
<li><strong><code>Xchat::EAT_XCHAT</code>    - don't let xchat see this event</strong>
</li>
<li><strong><code>Xchat::EAT_PLUGIN</code>   - don't let other scripts and plugins see this event but xchat will still see it</strong>
</li>
<li><strong><code>Xchat::EAT_ALL</code>      - don't let anything else see this event</strong>
</li>
</ul>
<p>
</p>
<h4><a name="timer_and_fd_hooks" />Timer and fd hooks</h4>
<ul>
<li><strong><code>Xchat::KEEP</code>         - keep the timer going or hook watching the handle</strong>
</li>
<li><strong><code>Xchat::REMOVE</code>       - remove the timer or hook watching the handle</strong>
</li>
</ul>
<p>
</p>
<h3><a name="hook_fd_flags" />hook_fd flags</h3>
<ul>
<li><strong><code>Xchat::FD_READ</code>			- invoke the callback when the handle is ready for reading</strong>
</li>
<li><strong><code>Xchat::FD_WRITE</code>		- invoke the callback when the handle is ready for writing</strong>
</li>
<li><strong><code>Xchat::FD_EXCEPTION</code>	- invoke the callback if an exception occurs</strong>
</li>
<li><strong><code>Xchat::FD_NOTSOCKET</code>	- indicate that the handle being hooked is not a socket</strong>
</li>
</ul>
<p>
</p>
<h2><a name="functions" />Functions</h2>
<p>
</p>
<h3><a name="xchat_register" /><code>Xchat::register( $name, $version, [$description,[$callback]] )</code></h3>
<ul>
<li><strong><code>$name</code>             -  The name of this script</strong>
</li>
<li><strong><code>$version</code>          -  This script's version</strong>
</li>
<li><strong><code>$description</code>   -  A description for this script</strong>
</li>
<li><strong><code>$callback</code>      -  This is a function that will be called when the is script
                     unloaded. This can be either a reference to a
                     function or an anonymous sub reference.</strong>
</li>
</ul>
<p>This is the first thing to call in every script.</p>
<p>
</p>
<h3><a name="xchat_hook_server" /><code>Xchat::hook_server( $message, $callback, [\%options] )</code></h3>
<p>
</p>
<h3><a name="xchat_hook_command" /><code>Xchat::hook_command( $command, $callback, [\%options] )</code></h3>
<p>
</p>
<h3><a name="xchat_hook_print" /><code>Xchat::hook_print( $event,$callback, [\%options] )</code></h3>
<p>
</p>
<h3><a name="xchat_hook_timer" /><code>Xchat::hook_timer( $timeout,$callback, [\%options | $data] )</code></h3>
<p>
</p>
<h3><a name="xchat_hook_fd" /><code>Xchat::hook_fd( $handle, $callback, [ \%options ] )</code></h3>
<p>These functions can be to intercept various events.
hook_server can be used to intercept any incoming message from the IRC server.
hook_command can be used to intercept any command, if the command doesn't currently exist then a new one is created.
hook_print can be used to intercept any of the events listed in Setttings-&gt;Advanced-&gt;Text Events
hook_timer can be used to create a new timer</p>
<ul>
<li><strong><code>$message</code>       -  server message to hook such as PRIVMSG</strong>
</li>
<li><strong><code>$command</code>       -  command to intercept, without the leading /</strong>
</li>
<li><strong><code>$event</code>      -  one of the events listed in Settings-&gt;Advanced-&gt;Text Events</strong>
</li>
<li><strong><code>$timeout</code>       -  timeout in milliseconds</strong>
</li>
<li><strong><code>$handle</code>     - the I/O handle you want to monitor with hook_fd. This must be something that has a fileno. See perldoc -f fileno or <a href="http://perldoc.perl.org/functions/fileno.html">fileno</a></strong>
</li>
<li><strong><code>$callback</code>   -  callback function, this is called whenever
                  the hooked event is trigged, the following are
                  the conditions that will trigger the different hooks.
                  This can be either a reference to a
                  function or an anonymous sub reference.</strong>
</li>
<li><strong>\%options   -  a hash reference containing addional options for the hooks</strong>
</li>
</ul>
<p>Valid keys for \%options:</p>
<table border="1">   <tr>
   <td>data</td>  <td>Additional data that is to be associated with the<br />
                  hook. For timer hooks this value can be provided either as<br />
                  <code>Xchat::hook_timer( $timeout, $cb,{data=&gt;$data})</code><br />
                  or <code>Xchat::hook_timer( $timeout, $cb, $data )</code>.<br />
                  However, this means that hook_timer cannot be provided<br />
                  with a hash reference containing data as a key.<br />                  example:<br />
                  my $options = { data =&gt; [@arrayOfStuff] };<br />
                  Xchat::hook_timer( $timeout, $cb, $options );<br />
                  <br />
                  In this example, the timer's data will be<br />
                  [@arrayOfStuff] and not { data =&gt; [@arrayOfStuff] }<br />
                  <br />
                  This key is valid for all of the hook functions.<br />
                  <br />
                  Default is undef.<br />
                  </td>
   </tr>   <tr>
      <td>priority</td> <td>Sets the priority for the hook.<br />
                        It can be set to one of the
                        <code>Xchat::PRI_*</code> constants.<br />
                        <br />
                        This key only applies to server, command
                        and print hooks.<br />
                        <br />
                        Default is <code>Xchat::PRI_NORM</code>.
                        </td>   </tr>   <tr>
      <td>help_text</td>   <td>Text displayed for /help $command.<br />
                           <br />
                           This key only applies to command hooks.<br />
                           <br />
                           Default is "".
                           </td>
   </tr>   <tr>
      <td>flags</td>   <td>Specify the flags for a fd hook.<br />
                       <br />
                       See <a href="#hook_fd_flags">hook fd flags</a> section for valid values.<br />
                       <br />
                       On Windows if the handle is a pipe you specify<br />
                       Xchat::FD_NOTSOCKET in addition to any other flags you might be using.<br />
                       <br />
                       This key only applies to fd hooks.<br />
                       Default is Xchat::FD_READ
                           </td>
   </tr></table><p>
</p>
<h4><a name="when_callbacks_are_invoked" />When callbacks are invoked</h4>
<p>Each of the hooks will be triggered at different times depending on the type
of hook.</p>
<table border="1">   <tr style="background-color: #dddddd">
      <td>Hook Type</td>   <td>When the callback will be invoked</td>
   </tr>   <tr>
      <td>server hooks</td>   <td>a <code>$message</code> message is 
                              received from the server
                              </td>
   </tr>   <tr>
      <td>command hooks</td>  <td>the <code>$command</code> command is
                              executed, either by the user or from a script
                              </td>
   </tr>   <tr>
      <td>print hooks</td> <td>X-Chat is about to print the message for the
                           <code>$event</code> event
                           </td>
   </tr>   <tr>
      <td>timer hooks</td> <td>called every <code>$timeout</code> milliseconds
                           (1000 millisecond is 1 second)<br />
                           the callback will be executed in the same context where
                           the hook_timer was called, if the context no longer exists
                           then it will execute in a random context
                           </td>
   </tr>   <tr>
      <td>fd hooks</td> <td>depends on the flags that were passed to hook_fd<br />
                        See <a href="#hook_fd_flags">hook_fd flags</a> section.
                        </td>
   </tr>
</table><p>The value return from these hook functions can be passed to <code>Xchat::unhook</code> 
to remove the hook.</p>
<p>
</p>
<h4><a name="callback_arguments" />Callback Arguments</h4>
<p>All callback functions will receive their arguments in <code>@_</code> like every
other Perl subroutine.</p>
<p>
Server and command callbacks<br />
<br />
<code>$_[0]</code>   -  array reference containing the IRC message or command and
arguments broken into words<br />
example:<br />
/command arg1 arg2 arg3<br />
<code>$_[0][0]</code> -  command<br />
<code>$_[0][1]</code> -  arg1<br />
<code>$_[0][2]</code> -  arg2<br />
<code>$_[0][3]</code> -  arg3<br />
<br />
<code>$_[1]</code>   -  array reference containing the Nth word to the last word<br />
example:<br />
/command arg1 arg2 arg3<br />
<code>$_[1][0]</code>   -  command arg1 arg2 arg3<br />
<code>$_[1][1]</code>   -  arg1 arg2 arg3<br />
<code>$_[1][2]</code>   -  arg2 arg3<br />
<code>$_[1][3]</code>   -  arg3<br />
<br />
<code>$_[2]</code>   -  the data that was passed to the hook function<br />
<br />
Print callbacks<br />
<br />
<code>$_[0]</code>   -  array reference containing the values for the
                        text event see Settings-&gt;Advanced-&gt;Text Events<br />
<code>$_[1]</code>   -  the data that was passed to the hook function<br />
<br />
Timer callbacks<br />
<br />
<code>$_[0]</code>   -  the data that was passed to the hook function<br />
<br />fd callbacks<br />
<br />
<code>$_[0]</code>   -  the handle that was passed to hook_fd<br />
<code>$_[1]</code>   -  flags indicating why the callback was called<br />
<code>$_[2]</code>   -  the data that was passed to the hook function<br />
</p><p>
</p>
<h4><a name="callback_return_values" />Callback return values</h4>
<p>All server, command and print  callbacks should return one of
the <code>Xchat::EAT_*</code> constants.
Timer callbacks can return Xchat::REMOVE to remove
the timer or Xchat::KEEP to keep it going</p>
<p>
</p>
<h4><a name="miscellaneous_hook_related_information" />Miscellaneous Hook Related Information</h4>
<p>For server hooks, if <code>$message</code> is &quot;RAW LINE&quot; then <code>$cb</code> will be called for
every IRC message than X-Chat receives.</p>
<p>For command hooks if <code>$command</code> is &quot;&quot; then <code>$cb</code> will be called for
messages entered by the user that is not a command.</p>
<p>For print hooks besides those events listed in 
Settings-&gt;Advanced-&gt;Text Events, these additional events can be used.</p>
<table border="1">   <tr style="background-color: #dddddd">
      <td>Event</td> <td>Description</td>
   </tr>   <tr>
      <td>"Open Context"</td> <td>a new context is created</td>
   </tr>   <tr>
      <td>"Close Context"</td>   <td>a context has been close</td>
   </tr>   <tr>
      <td>"Focus Tab"</td> <td>when a tab is brought to the front</td>
   </tr>   <tr>
      <td>"Focus Window"</td> <td>when a top level window is focused or the
                              main tab window is focused by the window manager
                              </td>
   </tr>   <tr>
      <td>"DCC Chat Text"</td>   <td>when text from a DCC Chat arrives.
                                 <code>$_[0]</code> will have these values<br />
                                 <br />
                                 <code>$_[0][0]</code>   -  Address<br />
                                 <code>$_[0][1]</code>   -  Port<br />
                                 <code>$_[0][2]</code>   -  Nick<br />
                                 <code>$_[0][3]</code>   -  Message<br />
                                 </td>
   </tr>   <tr>
      <td>"Key Press"</td> <td>used for intercepting key presses<br />
			$_[0][0] - key value<br />
			$_[0][1] - state bitfield, 1 - shift, 4 - control, 8 - alt<br />
			$_[0][2] - string version of the key which might be empty for unprintable keys<br />
			$_[0][3] - length of the string in $_[0][2]<br />
		</td>
   </tr>
</table><p>
</p>
<h3><a name="xchat_unhook" /><code>Xchat::unhook( $hook )</code></h3>
<ul>
<li><strong><code>$hook</code>    -  the hook that was previously returned by one of the <code>Xchat::hook_*</code> functions</strong>
</li>
</ul>
<p>This function is used to removed a hook previously added with one of
the <code>Xchat::hook_*</code> functions</p>
<p>It returns the data that was passed to the <code>Xchat::hook_*</code> function when
the hook was added</p>
<p>
</p>
<h3><a name="xchat_print" /><code>Xchat::print( $text | \@lines, [$channel,[$server]] )</code></h3>
<ul>
<li><strong><code>$text</code>    -  the text to print</strong>
</li>
<li><strong><code>\@lines</code>  -  array reference containing lines of text to be printed
               all the elements will be joined together before printing</strong>
</li>
<li><strong><code>$channel</code> -  channel or tab with the given name where <code>$text</code>
               will be printed</strong>
</li>
<li><strong><code>$server</code>  -  specifies that the text will be printed in a channel or tab
               that is associated with <code>$server</code></strong>
</li>
</ul>
<p>The first argument can either be a string or an array reference of strings.
Either or both of <code>$channel</code> and <code>$server</code> can be undef.</p>
<p>If called as <code>Xchat::print( $text )</code>, it will always return true.
If called with either the channel or the channel and the server
specified then it will return true if a context is found and
false otherwise. The text will not be printed if the context
is not found.  The meaning of setting <code>$channel</code> or <code>$server</code> to
undef is the same as
<a href="#xchat_find_context">find_context</a>.</p>
<p>
</p>
<h3><a name="xchat_printf" /><code>Xchat::printf( $format, LIST )</code></h3>
<ul>
<li><strong><code>$format</code>  -  a format string, see &quot;perldoc -f <a href="http://perldoc.perl.org/functions/sprintf.html">sprintf</a>&quot; for further detail</strong>
</li>
<li><strong>LIST     -  list of values for the format fields</strong>
</li>
</ul>
<p>
</p>
<h3><a name="xchat_command" /><code>Xchat::command( $command | \@commands, [$channel,[$server]] )</code></h3>
<ul>
<li><strong><code>$command</code> -  the command to execute, without the leading /</strong>
</li>
<li><strong><code>\@commands</code>  -  array reference containing a list of commands to execute</strong>
</li>
<li><strong><code>$channel</code> -  channel or tab with the given name where <code>$command</code> will be executed</strong>
</li>
<li><strong><code>$server</code>  -  specifies that the command will be executed in a channel or tab that is associated with <code>$server</code></strong>
</li>
</ul>
<p>The first argument can either be a string or an array reference of strings.
Either or both of <code>$channel</code> and <code>$server</code> can be undef.</p>
<p>If called as <code>Xchat::command( $command )</code>, it will always return true.
If called with either the channel or the channel and the server
specified then it will return true if a context is found and false
otherwise. The command will not be executed if the context is not found.
The meaning of setting <code>$channel</code> or <code>$server</code> to undef is the same
as find_context.</p>
<p>
</p>
<h3><a name="xchat_commandf" /><code>Xchat::commandf( $format, LIST )</code></h3>
<ul>
<li><strong><code>$format</code> -  a format string, see &quot;perldoc -f <a href="http://perldoc.perl.org/functions/sprintf.html">sprintf</a>&quot; for further detail</strong>
</li>
<li><strong>LIST     -  list of values for the format fields</strong>
</li>
</ul>
<p>
</p>
<h3><a name="xchat_find_context" /><code>Xchat::find_context( [$channel, [$server]] )</code></h3>
<ul>
<li><strong><code>$channel</code> -  name of a channel</strong>
</li>
<li><strong><code>$server</code>  -  name of a server</strong>
</li>
</ul>
<p>Either or both of <code>$channel</code> and $server can be undef. Calling
<code>Xchat::find_context()</code> is the same as calling
<code>Xchat::find_context( undef, undef)</code> and
<code>Xchat::find_context( $channel )</code> is
the same as <code>Xchat::find_context( $channel, undef )</code>.</p>
<p>If <code>$server</code> is undef, find any channel named $channel.
If <code>$channel</code> is undef, find the front most window
or tab named <code>$server</code>.If both $channel and
<code>$server</code> are undef, find the currently focused tab or window.</p>
<p>Return the context found for one of the above situations or undef if such
a context cannot be found.</p>
<p>
</p>
<h3><a name="xchat_get_context" /><code>Xchat::get_context()</code></h3>
<p>Returns the current context.</p>
<p>
</p>
<h3><a name="xchat_set_context" /><code>Xchat::set_context( $context | $channel,[$server] )</code></h3>
<ul>
<li><strong><code>$context</code> -  context value as returned from <a href="#xchat_get_context">get_context</a>,<a href="#xchat_find_context">find_context</a> or one
               of the fields in the list of hashrefs returned by list_get</strong>
</li>
<li><strong><code>$channel</code> -  name of a channel you want to switch context to</strong>
</li>
<li><strong><code>$server</code>  -  name of a server you want to switch context to</strong>
</li>
</ul>
<p>See <a href="#xchat_find_context">find_context</a> for more details on <code>$channel</code> and <code>$server</code>.</p>
<p>Returns true on success, false on failure</p>
<p>
</p>
<h3><a name="xchat_get_info" /><code>Xchat::get_info( $id )</code></h3>
<ul>
<li><strong><code>$id</code>   -  one of the following case sensitive values</strong>
</li>
</ul>
<table border="1">   <tr style="background-color: #dddddd">
      <td>ID</td>
      <td>Return value</td>
      <td>Associated Command(s)</td>
   </tr>   <tr>
      <td>away</td>
      <td>away reason or undef if you are not away</td>
      <td>AWAY, BACK</td>
   </tr>   <tr>
      <td>channel</td>
      <td>current channel name</td>
      <td>SETTAB</td>
   </tr>   <tr>
      <td>charset</td>
      <td>character-set used in the current context</td>
      <td>CHARSET</td>
   </tr>   <tr>
      <td>event_text &lt;Event Name&gt;</td> <td>text event format string for &lt;Event name&gt;<br />
      Example:
   <div class="example synNormal"><div class='line_number'>
<div>1</div>
</div>
<div class='content'><pre><span class="synStatement">my</span> <span class="synIdentifier">$channel_msg_format</span> = Xchat::get_info( <span class="synStatement">&quot;</span><span class="synConstant">event_text Channel Message</span><span class="synStatement">&quot;</span> );
</pre></div>
</div>
   </td>
   <td></td>
</tr>
<tr>
   <td>host</td>
   <td>real hostname of the current server</td>
   <td></td>
</tr><tr>
   <td>id</td>
   <td>connection id</td>
   <td></td>
</tr><tr>
   <td>inputbox</td>
   <td>contents of the inputbox</td>
   <td>SETTEXT</td>
</tr><tr>
   <td>libdirfs</td>
   <td>the system wide directory where xchat will look for plugins.
   this string is in the same encoding as the local file system</td>
   <td></td>
</tr><tr>
   <td>modes</td>
   <td>the current channels modes or undef if not known</td>
   <td>MODE</td>
</tr><tr>
   <td>network</td>
   <td>current network name or undef, this value is taken from the Network List</td>
   <td></td>
</tr><tr>
   <td>nick</td>
   <td>current nick</td>
   <td>NICK</td>
</tr><tr>
   <td>nickserv</td>
   <td>nickserv password for this network or undef, this value is taken from the Network List</td>
   <td></td>
</tr><tr>
   <td>server</td>   <td>current server name <br />
                     (what the server claims to be) undef if not connected
                     </td>
   <td></td>
</tr><tr>
   <td>state_cursor</td>
   <td>current inputbox cursor position in characters</td>
   <td>SETCURSOR</td>
</tr><tr>
   <td>topic</td>
   <td>current channel topic</td>
   <td>TOPIC</td>
</tr><tr>
   <td>version</td>
   <td>xchat version number</td>
   <td></td>
</tr><tr>
   <td>win_status</td>
   <td>status of the xchat window, possible values are "active", "hidden"
   and "normal"</td>
   <td>GUI</td>
</tr><tr>
  <td>win_ptr</td> <td>native window pointer, GtkWindow * on Unix, HWND on Win32.<br />
  On Unix if you have the Glib module installed you can use my $window = Glib::Object->new_from_pointer( Xchat::get_info( "win_ptr" ) ); to get a Gtk2::Window object.<br />
  Additionally when you have detached tabs, each of the windows will return a different win_ptr for the different Gtk2::Window objects.<br />
  See <a href="http://xchat.cvs.sourceforge.net/viewvc/xchat/xchat2/plugins/perl/char_count.pl?view=markup">char_count.pl</a> for a longer example of a script that uses this to show how many characters you currently have in your input box.
  </td>
  <td></td>
</tr>
<tr>
  <td>gtkwin_ptr</td>
  <td>similar to win_ptr except it will always be a GtkWindow *</td>
  <td></td>
</tr>
<tr>
   <td>xchatdir</td> <td>xchat config directory encoded in UTF-8<br />
                     examples:<br />
                     /home/user/.xchat2<br />
                     C:\Documents and Settings\user\Application Data\X-Chat 2
                     </td>
   <td></td>
</tr><tr>
   <td>xchatdirfs</td>  <td>same as xchatdir except encoded in the locale file system encoding</td>
   <td></td>
</tr>
</table><p>This function is used to retrieve certain information about the current
context. If there is an associated command then that command can be used to change the value for a particular ID.</p><p>
</p>
<h3><a name="hexchat_get_prefs" /><code>Xchat::get_prefs( $name )</code></h3>
<ul>
<li><strong><code>$name</code> -  name of a X-Chat setting (available through the /set command)</strong>
</li>
</ul>
<p>This function provides a way to retrieve X-Chat's setting information.</p>
<p>Returns <code>undef</code> if there is no setting called called <code>$name</code>.</p>
<p>
</p>
<h3><a name="xchat_emit_print" /><code>Xchat::emit_print( $event, LIST )</code></h3>
<ul>
<li><strong><code>$event</code>   -  name from the Event column in Settings-&gt;Advanced-&gt;Text Events</strong>
</li>
<li><strong>LIST     -  this depends on the Description column on the bottom of Settings-&gt;Advanced-&gt;Text Events</strong>
</li>
</ul>
<p>This functions is used to generate one of the events listed under
Settings-&gt;Advanced-&gt;Text Events</p>
<p>Note: when using this function you MUST return Xchat::EAT_ALL otherwise you will end up with duplicate events.
One is the original and the second is the one you emit.</p>
<p>Returns true on success, false on failure</p>
<p>
</p>
<h3><a name="xchat_send_modes" /><code>Xchat::send_modes( $target | \@targets, $sign, $mode, [ $modes_per_line ] )</code></h3>
<ul>
<li><strong><code>$target</code>  -  a single nick to set the mode on</strong>
</li>
<li><strong><code>\@targets</code>   -  an array reference of the nicks to set the mode on</strong>
</li>
<li><strong><code>$sign</code> - the mode sign, either '+' or '-'</strong>
</li>
<li><strong><code>$mode</code> - the mode character such as 'o' and 'v', this can only be one character long</strong>
</li>
<li><strong><code>$modes_per_line</code>   -  an optional argument maximum number of modes to send per at once, pass 0 use the current server's maximum (default)</strong>
</li>
</ul>
<p>Send multiple mode changes for the current channel. It may send multiple MODE lines if the request doesn't fit on one.</p>
<p>Example:</p>
<div class="example synNormal"><div class='line_number'>
<div>1</div>
<div>2</div>
<div>3</div>
<div>4</div>
<div>5</div>
<div>6</div>
<div>7</div>
<div>8</div>
<div>9</div>
<div>10</div>
<div>11</div>
<div>12</div>
<div>13</div>
<div>14</div>
</div>
<div class='content'><pre><span class="synStatement">use strict</span>;
<span class="synStatement">use </span>warning;
<span class="synStatement">use </span>Xchat <span class="synStatement">qw(</span><span class="synConstant">:all</span><span class="synStatement">)</span>;

hook_command( <span class="synStatement">&quot;</span><span class="synConstant">MODES</span><span class="synStatement">&quot;</span>, <span class="synStatement">sub </span>{
   <span class="synStatement">my</span> (<span class="synStatement">undef</span>, <span class="synIdentifier">$who</span>, <span class="synIdentifier">$sign</span>, <span class="synIdentifier">$mode</span>) = <span class="synIdentifier">@{$_[</span><span class="synConstant">0</span><span class="synIdentifier">]}</span>;
   <span class="synStatement">my</span> <span class="synIdentifier">@targets</span> = <span class="synStatement">split</span> <span class="synStatement">/</span><span class="synConstant">,</span><span class="synStatement">/</span>, <span class="synIdentifier">$who</span>;
   <span class="synStatement">if</span>( <span class="synIdentifier">@targets</span> &gt; <span class="synConstant">1</span> ) {
      send_modes( \<span class="synIdentifier">@targets</span>, <span class="synIdentifier">$sign</span>, <span class="synIdentifier">$mode</span>, <span class="synConstant">1</span> );
   } <span class="synStatement">else</span> {
      send_modes( <span class="synIdentifier">$who</span>, <span class="synIdentifier">$sign</span>, <span class="synIdentifier">$mode</span> );
   }
   <span class="synStatement">return</span> EAT_XCHAT;
});
</pre></div>
</div><p>
</p>
<h3><a name="xchat_nickcmp" /><code>Xchat::nickcmp( $nick1, $nick2 )</code></h3>
<ul>
<li><strong><code>$nick1, $nick2</code> -  the two nicks or channel names that are to be compared</strong>
</li>
</ul>
<p>The comparsion is based on the current server. Either a <a href="http://www.ietf.org/rfc/rfc1459.txt" class="rfc">RFC1459</a> compliant
string compare or plain ascii will be using depending on the server. The
comparison is case insensitive.</p>
<p>Returns a number less than, equal to or greater than zero if
<code>$nick1</code> is 
found respectively, to be less than, to match, or be greater than
<code>$nick2</code>.</p>
<p>
</p>
<h3><a name="xchat_get_list" /><code>Xchat::get_list( $name )</code></h3>
<ul>
<li><strong><code>$name</code> -  name of the list, one of the following:
&quot;channels&quot;, &quot;dcc&quot;, &quot;ignore&quot;, &quot;notify&quot;, &quot;users&quot;</strong>
</li>
</ul>
<p>This function will return a list of hash references.  The hash references
will have different keys depend on the list.  An empty list is returned
if there is no such list.</p>
<p>"channels"  -  list of channels, querys and their server</p><table border="1">   <tr style="background-color: #dddddd">
      <td>Key</td>   <td>Description</td>
   </tr>   <tr>
      <td>channel</td>  <td>tab name</td>
   </tr>   <tr>
      <td>chantypes</td>
      <td>channel types supported by the server, typically "#&amp;"</td>
   </tr>   <tr>
      <td>context</td>  <td>can be used with set_context</td>
   </tr>   <tr>
      <td>flags</td> <td>Server Bits:<br />
                     0 - Connected<br />
                     1 - Connecting<br />
                     2 - Away<br />
                     3 - EndOfMotd(Login complete)<br />
                     4 - Has WHOX<br />
                     5 - Has IDMSG (FreeNode)<br />
                    <br />
                    <p>The following correspond to the /chanopt command</p>
                    6  - Hide Join/Part Message (text_hidejoinpart)<br />
                    7  - unused (was for color paste)<br />
                    8  - Beep on message (alert_beep)<br />
                    9  - Blink Tray (alert_tray)<br />
                    10 - Blink Task Bar (alert_taskbar)<br />
<p>Example of checking if the current context has Hide Join/Part messages set:</p>
<div class="example synNormal"><div class='line_number'>
<div>1</div>
<div>2</div>
<div>3</div>
</div>
<div class='content'><pre><span class="synStatement">if</span>( Xchat::context_info-&gt;{flags} &amp; (<span class="synConstant">1</span> &lt;&lt; <span class="synConstant">6</span>) ) {
  Xchat::<span class="synStatement">print</span>( <span class="synStatement">&quot;</span><span class="synConstant">Hide Join/Part messages is enabled</span><span class="synStatement">&quot;</span> );
}
</pre></div>
</div>                     </td>
   </tr>   <tr>
      <td>id</td> <td>Unique server ID </td>
   </tr>
   
   <tr>
      <td>lag</td>
      <td>lag in milliseconds</td>
   </tr>   <tr>
      <td>maxmodes</td> <td>Maximum modes per line</td>
   </tr>   <tr>
      <td>network</td>  <td>network name to which this channel belongs</td>
   </tr>   <tr>
      <td>nickprefixes</td>   <td>Nickname prefixes e.g. "+@"</td>
   </tr>
   
   <tr>
      <td>nickmodes</td>   <td>Nickname mode chars e.g. "vo"</td>
   </tr>   <tr>
      <td>queue</td>
      <td>number of bytes in the send queue</td>
   </tr>
   
   <tr>
      <td>server</td>   <td>server name to which this channel belongs</td>
   </tr>   <tr>
      <td>type</td>  <td>the type of this context<br />
                     1 - server<br />
                     2 - channel<br />
                     3 - dialog<br />
                     4 - notices<br />
                     5 - server notices<br />
                     </td>
   </tr>   <tr>
      <td>users</td> <td>Number of users in this channel</td>
   </tr>
</table><p>"dcc"       -  list of DCC file transfers</p>
<table border="1">   <tr style="background-color: #dddddd">
      <td>Key</td>   <td>Value</td>
   </tr>   <tr>
      <td>address32</td>   <td>address of the remote user(ipv4 address)</td>
   </tr>   <tr>
      <td>cps</td>   <td>bytes per second(speed)</td>
   </tr>   <tr>
      <td>destfile</td> <td>destination full pathname</td>
   </tr>   <tr>
      <td>file</td>  <td>file name</td>
   </tr>   <tr>
      <td>nick</td>
      <td>nick of the person this DCC connection is connected to</td>
   </tr>   <tr>
      <td>port</td>  <td>TCP port number</td>
   </tr>   <tr>
      <td>pos</td>   <td>bytes sent/received</td>
   </tr>   <tr>
      <td>poshigh</td>   <td>bytes sent/received, high order 32 bits</td>
   </tr>   <tr>
      <td>resume</td>   <td>point at which this file was resumed<br />
                        (zero if it was not resumed)
                        </td>
   </tr>   <tr>
      <td>resumehigh</td>   <td>point at which this file was resumed, high order 32 bits<br />
                        </td>
   </tr>   <tr>
      <td>size</td>  <td>file size in bytes low order 32 bits</td>
   </tr>   <tr>
      <td>sizehigh</td>	<td>file size in bytes, high order 32 bits (when the files is > 4GB)</td>
	</tr>
	<tr>
      <td>status</td>   <td>DCC Status:<br />
                        0 - queued<br />
                        1 - active<br />
                        2 - failed<br />
                        3 - done<br />
                        4 - connecting<br />
                        5 - aborted
                        </td>
   </tr>   <tr>
      <td>type</td>  <td>DCC Type:<br />
                     0 - send<br />
                     1 - receive<br />
                     2 - chatrecv<br />
                     3 - chatsend
                     </td>
   </tr></table><p>"ignore"    -  current ignore list</p>
<table border="1">   <tr style="background-color: #dddddd">
      <td>Key</td> <td>Value</td>
   </tr>   <tr>
      <td>mask</td>  <td>ignore mask. e.g: *!*@*.aol.com</td>
   </tr>   <tr>
      <td>flags</td> <td>Bit field of flags.<br />
                     0 - private<br />
                     1 - notice<br />
                     2 - channel<br />
                     3 - ctcp<br />
                     4 - invite<br />
                     5 - unignore<br />
                     6 - nosave<br />
                     7 - dcc<br />
                     </td>
   </tr></table><p>"notify" - list of people on notify</p>
<table border="1">
   <tr style="background-color: #dddddd">
      <td>Key</td>   <td>Value</td>
   </tr>   <tr>
      <td>networks</td>
      <td>comma separated list of networks where you will be notfified about this user's online/offline status or undef if you will be notificed on every network you are connected to</td>
   </tr>   <tr>
      <td>nick</td>  <td>nickname</td>
   </tr>   <tr>
      <td>flags</td> <td>0 = is online</td>
   </tr>   <tr>
      <td>on</td> <td>time when user came online</td>
   </tr>   <tr>
      <td>off</td>   <td>time when user went offline</td>
   </tr>   <tr>
      <td>seen</td>  <td>time when user was last verified still online</td>
   </tr>
</table><p>the values indexed by on, off and seen can be passed to localtime
and gmtime, see perldoc -f <a href="http://perldoc.perl.org/functions/localtime.html">localtime</a> and perldoc -f <a href="http://perldoc.perl.org/functions/gmtime.html">gmtime</a> for more
detail</p><p>"users"     -  list of users in the current channel</p>
<table border="1">   <tr style="background-color: #dddddd">
      <td>Key</td>   <td>Value</td>
   </tr>   <tr>
      <td>away</td>  <td>away status(boolean)</td>
   </tr>   <tr>
      <td>lasttalk</td>
      <td>last time a user was seen talking, this is the an epoch time(number of seconds since a certain date, that date depends on the OS)</td>
   </tr>   <tr>
      <td>nick</td>  <td>nick name</td>
   </tr>   <tr>
      <td>host</td>
      <td>host name in the form: user@host or undef if not known</td>
   </tr>   <tr>
      <td>prefix</td>   <td>prefix character, .e.g: @ or +</td>
   </tr>   <tr>
      <td>realname</td>
       <td>Real name or undef</td>
   </tr>   <tr>
      <td>selected</td>
      <td>selected status in the user list, only works when retrieving the user list of the focused tab. You can use the /USELECT command to select the nicks</td>
   </tr>
</table><p>"networks"	-	list of networks and the associated settings from network list</p>
<table border="1">   <tr style="background-color: #dddddd">
      <td>Key</td>   <td>Value</td>
   </tr>
	
	<tr>
	<td>autojoins</td> <td>An object with the following methods:<br />
		<table>
			<tr>
				<td>Method</td>
				<td>Description</td>
			</tr>			<tr>
				<td>channels()</td>
				<td>returns a list of this networks' autojoin channels in list context, a count of the number autojoin channels in scalar context</td>
			</tr>			<tr>
				<td>keys()</td>
				<td>returns a list of the keys to go with the channels, the order is the same as the channels, if a channel doesn't  have a key, '' will be returned in it's place</td>
			</tr>			<tr>
				<td>pairs()</td>
				<td>a combination of channels() and keys(), returns a list of (channels, keys) pairs. This can be assigned to a hash for a mapping from channel to key.</td>
			</tr>			<tr>
				<td>as_hash()</td>
				<td>return the pairs as a hash reference</td>
			</tr>			<tr>
				<td>as_string()</td>
				<td>the original string that was used to construct this autojoin object, this can be used with the JOIN command to join all the channels in the autojoin list</td>
			</tr>			<tr>
				<td>as_array()</td>
				<td>return an array reference of hash references consisting of the keys "channel" and "key"</td>
			</tr>			<tr>
				<td>as_bool()</td>
				<td>returns true if the network has autojoins and false otherwise</td>
			</tr>
		</table>
	</td>
	</tr>
   
	<tr>
	<td>connect_commands</td> <td>An array reference containing the connect commands for a network. An empty array if there aren't any</td>
	</tr>	<tr>
	<td>encoding</td> <td>the encoding for the network</td>
	</tr>	<tr>
		<td>flags</td>
		<td>
			a hash reference corresponding to the checkboxes in the network edit window
			<table>
				<tr>
					<td>allow_invalid</td>
					<td>true if "Accept invalid SSL certificate" is checked</td>
				</tr>				<tr>
					<td>autoconnect</td>
					<td>true if "Auto connect to this network at startup" is checked</td>
				</tr>				<tr>
					<td>cycle</td>
					<td>true if "Connect to selected server only" is <strong>NOT</strong> checked</td>
				</tr>				<tr>
					<td>use_global</td>
					<td>true if "Use global user information" is checked</td>
				</tr>				<tr>
					<td>use_proxy</td>
					<td>true if "Bypass proxy server" is <strong>NOT</strong> checked</td>
				</tr>				<tr>
					<td>use_ssl</td>
					<td>true if "Use SSL for all the servers on this network" is checked</td>
				</tr>
			</table>
		</td>
	</tr>	<tr>
		<td>irc_nick1</td>
		<td>Corresponds with the "Nick name" field in the network edit window</td>
	</tr>	<tr>
		<td>irc_nick2</td>
		<td>Corresponds with the "Second choice" field in the network edit window</td>
	</tr>	<tr>
		<td>irc_real_name</td>
		<td>Corresponds with the "Real name" field in the network edit window</td>
	</tr>	<tr>
		<td>irc_user_name</td>
		<td>Corresponds with the "User name" field in the network edit window</td>
	</tr>	<tr>
		<td>network</td>
		<td>Name of the network</td>
	</tr>	<tr>
		<td>nickserv_password</td>
		<td>Corresponds with the "Nickserv password" field in the network edit window</td>
	</tr>	<tr>
		<td>selected</td>
		<td>Index into the list of servers in the "servers" key, this is used if the "cycle" flag is false</td>
	</tr>	<tr>
		<td>server_password</td>
		<td>Corresponds with the "Server password" field in the network edit window</td>
	</tr>	<tr>
		<td>servers</td>
		<td>An array reference of hash references with a "host" and "port" key. If a port is not specified then 6667 will be used.</td>
	</tr>
</table><p>
</p>
<h3><a name="xchat_user_info" /><code>Xchat::user_info( [$nick] )</code></h3>
<ul>
<li><strong><code>$nick</code> -  the nick to look for, if this is not given your own nick will be
            used as default</strong>
</li>
</ul>
<p>This function is mainly intended to be used as a shortcut for when you need
to retrieve some information about only one user in a channel. Otherwise it
is better to use <a href="#xchat_get_list">get_list</a>.
If <code>$nick</code> is found a hash reference containing the same keys as those in the
&quot;users&quot; list of <a href="#xchat_get_list">get_list</a> is returned otherwise undef is returned.
Since it relies on <a href="#xchat_get_list">get_list</a> this function can only be used in a
channel context.</p>
<p>
</p>
<h3><a name="xchat_context_info" /><code>Xchat::context_info( [$context] )</code></h3>
<ul>
<li><strong><code>$context</code> -  context returned from <a href="#xchat_get_context">get_context</a>, <a href="#xchat_find_context">find_context</a> and <a href="#xchat_get_list">get_list</a>, this is the context that you want infomation about. If this is omitted, it will default to current context.</strong>
</li>
</ul>
<p>This function will return the information normally retrieved with <a href="#xchat_get_info">get_info</a>, except this is for the context that is passed in. The information will be returned in the form of a hash. The keys of the hash are the <code>$id</code> you would normally supply to <a href="#xchat_get_info">get_info</a> as well as all the keys that are valid for the items in the &quot;channels&quot; list from <a href="#xchat_get_list">get_list</a>. Use of this function is more efficient than calling get_list( &quot;channels&quot; ) and searching through the result.</p>
<p>Example:</p>
<div class="example synNormal"><div class='line_number'>
<div>1</div>
<div>2</div>
<div>3</div>
<div>4</div>
<div>5</div>
<div>6</div>
<div>7</div>
<div>8</div>
<div>9</div>
<div>10</div>
<div>11</div>
</div>
<div class='content'><pre><span class="synStatement">use strict</span>;
<span class="synStatement">use warnings</span>;
<span class="synStatement">use </span>Xchat <span class="synStatement">qw(</span><span class="synConstant">:all</span><span class="synStatement">)</span>; <span class="synComment"># imports all the functions documented on this page</span>

register( <span class="synStatement">&quot;</span><span class="synConstant">User Count</span><span class="synStatement">&quot;</span>, <span class="synStatement">&quot;</span><span class="synConstant">0.1</span><span class="synStatement">&quot;</span>,
   <span class="synStatement">&quot;</span><span class="synConstant">Print out the number of users on the current channel</span><span class="synStatement">&quot;</span> );
hook_command( <span class="synStatement">&quot;</span><span class="synConstant">UCOUNT</span><span class="synStatement">&quot;</span>, \<span class="synIdentifier">&amp;display_count</span> );
<span class="synStatement">sub </span><span class="synIdentifier">display_count </span>{
   prnt <span class="synStatement">&quot;</span><span class="synConstant">There are </span><span class="synStatement">&quot;</span> . context_info()-&gt;{users} . <span class="synStatement">&quot;</span><span class="synConstant"> users in this channel.</span><span class="synStatement">&quot;</span>;
   <span class="synStatement">return</span> EAT_XCHAT;
}
</pre></div>
</div><p>
</p>
<h3><a name="xchat_strip_code" /><code>Xchat::strip_code( $string )</code></h3>
<ul>
<li><strong><code>$string</code>  -  string to remove codes from</strong>
</li>
</ul>
<p>This function will remove bold, color, beep, reset, reverse and underline codes from <code>$string</code>. It will also remove ANSI escape codes which might get used by certain terminal based clients. If it is called in void context <code>$string</code> will be modified otherwise a modified copy of <code>$string</code> is returned.</p>
<p>
</p>
<h2><a name="examples" />Examples</h2>
<p>
</p>
<h3><a name="asynchronous_dns_resolution_with_hook_fd" />Asynchronous DNS resolution with hook_fd</h3>
<div class="example synNormal"><div class='line_number'>
<div>1</div>
<div>2</div>
<div>3</div>
<div>4</div>
<div>5</div>
<div>6</div>
<div>7</div>
<div>8</div>
<div>9</div>
<div>10</div>
<div>11</div>
<div>12</div>
<div>13</div>
<div>14</div>
<div>15</div>
<div>16</div>
<div>17</div>
<div>18</div>
<div>19</div>
<div>20</div>
<div>21</div>
<div>22</div>
<div>23</div>
<div>24</div>
<div>25</div>
<div>26</div>
<div>27</div>
<div>28</div>
<div>29</div>
<div>30</div>
<div>31</div>
<div>32</div>
<div>33</div>
<div>34</div>
<div>35</div>
</div>
<div class='content'><pre><span class="synStatement">use strict</span>;
<span class="synStatement">use warnings</span>;
<span class="synStatement">use </span>Xchat <span class="synStatement">qw(</span><span class="synConstant">:all</span><span class="synStatement">)</span>;
<span class="synStatement">use </span>Net::DNS;
   
hook_command( <span class="synStatement">&quot;</span><span class="synConstant">BGDNS</span><span class="synStatement">&quot;</span>, <span class="synStatement">sub </span>{
   <span class="synStatement">my</span> <span class="synIdentifier">$host</span> = <span class="synIdentifier">$_[</span><span class="synConstant">0</span><span class="synIdentifier">][</span><span class="synConstant">1</span><span class="synIdentifier">]</span>;
   <span class="synStatement">my</span> <span class="synIdentifier">$resolver</span> = Net::DNS::Resolver-&gt;new;
   <span class="synStatement">my</span> <span class="synIdentifier">$sock</span> = <span class="synIdentifier">$resolver-&gt;bgsend</span>( <span class="synIdentifier">$host</span> );
   
   hook_fd( <span class="synIdentifier">$sock</span>, <span class="synStatement">sub </span>{
      <span class="synStatement">my</span> <span class="synIdentifier">$ready_sock</span> = <span class="synIdentifier">$_[</span><span class="synConstant">0</span><span class="synIdentifier">]</span>;
      <span class="synStatement">my</span> <span class="synIdentifier">$packet</span> = <span class="synIdentifier">$resolver-&gt;bgread</span>( <span class="synIdentifier">$ready_sock</span> );
      
      <span class="synStatement">if</span>( <span class="synIdentifier">$packet-&gt;authority</span> &amp;&amp; (<span class="synStatement">my</span> <span class="synIdentifier">@answers</span> = <span class="synIdentifier">$packet-&gt;answer</span> ) ) {
         
         <span class="synStatement">if</span>( <span class="synIdentifier">@answers</span> ) {
            prnt <span class="synStatement">&quot;</span><span class="synIdentifier">$host</span><span class="synConstant">:</span><span class="synStatement">&quot;</span>;
            <span class="synStatement">my</span> <span class="synIdentifier">$padding</span> = <span class="synStatement">&quot;</span><span class="synConstant"> </span><span class="synStatement">&quot;</span> x (<span class="synStatement">length</span>( <span class="synIdentifier">$host</span> ) + <span class="synConstant">2</span>);
            <span class="synStatement">for</span> <span class="synStatement">my</span> <span class="synIdentifier">$answer</span> ( <span class="synIdentifier">@answers</span> ) {
               prnt <span class="synIdentifier">$padding</span> . <span class="synIdentifier">$answer-&gt;rdatastr</span> . <span class="synStatement">'</span><span class="synConstant"> </span><span class="synStatement">'</span> . <span class="synIdentifier">$answer-&gt;type</span>;
            }
         }
      } <span class="synStatement">else</span> {
         prnt <span class="synStatement">&quot;</span><span class="synConstant">Unable to resolve </span><span class="synIdentifier">$host</span><span class="synStatement">&quot;</span>;
      }
      
      <span class="synStatement">return</span> REMOVE;
   },
   {
      <span class="synConstant">flags</span> =&gt; FD_READ,
   });
   
   <span class="synStatement">return</span> EAT_XCHAT;
});
</pre></div>
</div>

<p>
</p>
<h2><a name="contact_information" />Contact Information</h2>
<p>Contact Lian Wan Situ at &lt;atmcmnky [at] yahoo.com&gt; for questions, comments and
corrections about this page or the Perl plugin itself.  You can also find me
in #xchat on FreeNode under the nick Khisanth.</p>
<table border="0" width="100%" cellspacing="0" cellpadding="3">
<tr><td class="block" style="background-color: #cccccc" valign="middle">
<big><strong><span class="block">&nbsp;X-Chat 2 Perl Interface</span></strong></big>
</td></tr>
</table>