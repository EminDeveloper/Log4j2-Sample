# Log4j2-Sample

### Basic Log4j2 Configuration
To start using log4j2 in your project, you simply need to add the log4j-core dependency.

Out of the box, log4j2 will automatically provide a simple configuration, if you don’t explicitly define one yourself. The default configuration logs to the console at a level of ERROR level or above.

To start logging messages using this basic configuration, all you need to do is obtain a Logger instance using the LogManager class:
<pre class="prettyprint prettyprinted" style=""><span class="kwd">private</span><span class="pln"> </span><span class="kwd">static</span><span class="pln"> </span><span class="typ">Logger</span><span class="pln"> logger </span><span class="pun">=</span><span class="pln"> </span><span class="typ">LogManager</span><span class="pun">.</span><span class="pln">getLogger</span><span class="pun">(</span><span class="typ">MyService</span><span class="pun">.</span><span class="kwd">class</span><span class="pun">);</span></pre>

Then you can use the logger object with methods corresponding to the log level you want:

<pre class="prettyprint prettyprinted" style=""><span class="pln">logger</span><span class="pun">.</span><span class="pln">info</span><span class="pun">(</span><span class="str">"This is information message"</span><span class="pun">);</span></pre>
