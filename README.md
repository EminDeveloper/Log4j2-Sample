# Log4j2-Sample

### Basic Log4j2 Configuration
To start using log4j2 in your project, you simply need to add the log4j-core dependency.

Out of the box, log4j2 will automatically provide a simple configuration, if you don’t explicitly define one yourself. The default configuration logs to the console at a level of ERROR level or above.

To start logging messages using this basic configuration, all you need to do is obtain a Logger instance using the LogManager class:
<pre class="prettyprint prettyprinted" style=""><span class="kwd">private</span><span class="pln"> </span><span class="kwd">static</span><span class="pln"> </span><span class="typ">Logger</span><span class="pln"> logger </span><span class="pun">=</span><span class="pln"> </span><span class="typ">LogManager</span><span class="pun">.</span><span class="pln">getLogger</span><span class="pun">(</span><span class="typ">MyService</span><span class="pun">.</span><span class="kwd">class</span><span class="pun">);</span></pre>

Then you can use the logger object with methods corresponding to the log level you want:

<pre class="prettyprint prettyprinted" style=""><span class="pln">logger</span><span class="pun">.</span><span class="pln">info</span><span class="pun">(</span><span class="str">"This is information message"</span><span class="pun">);</span></pre>

## Customizing the Log4j2 Configuration
A custom log4j2 configuration can be created either programmatically or through a configuration file.

The library supports config files written in XML, JSON, YAML, as well as the .properties format. Here, we’re going to use XML to discuss all examples primarily.

First, you can override the default configuration by simply creating a log4j2.xml file on the classpath:

<pre class="prettyprint prettyprinted" style=""><span class="tag">&lt;Appenders&gt;</span><span class="pln">
    </span><span class="tag">&lt;RollingFile</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"RollingFileAppender"</span><span class="pln"> </span><span class="atn">fileName</span><span class="pun">=</span><span class="atv">"logs/app.log"</span><span class="pln">
      </span><span class="atn">filePattern</span><span class="pun">=</span><span class="atv">"logs/${date:yyyy-MM}/app-%d{MM-dd-yyyy}-%i.log.gz"</span><span class="tag">&gt;</span><span class="pln">
        </span><span class="tag">&lt;PatternLayout&gt;</span><span class="pln">
            </span><span class="tag">&lt;Pattern&gt;</span><span class="pln">%d [%t] %p %c - %m%n</span><span class="tag">&lt;/Pattern&gt;</span><span class="pln">
        </span><span class="tag">&lt;/PatternLayout&gt;</span><span class="pln">
        </span><span class="tag">&lt;Policies&gt;</span><span class="pln">
            </span><span class="tag">&lt;OnStartupTriggeringPolicy</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
            </span><span class="tag">&lt;TimeBasedTriggeringPolicy</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
            </span><span class="tag">&lt;SizeBasedTriggeringPolicy</span><span class="pln"> </span><span class="atn">size</span><span class="pun">=</span><span class="atv">"50 MB"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
        </span><span class="tag">&lt;/Policies&gt;</span><span class="pln">
        </span><span class="tag">&lt;DefaultRolloverStrategy</span><span class="pln"> </span><span class="atn">max</span><span class="pun">=</span><span class="atv">"20"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
    </span><span class="tag">&lt;/RollingFile&gt;</span><span class="pln">
</span><span class="tag">&lt;/Appenders&gt;</span></pre>

