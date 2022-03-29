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

## The JDBCAppender
As the name suggests, this appender uses JDBC to write logs to a relational database.

For this configuration, you need to define a ConnectionSource, which can be either a JNDI Data Source or a custom ConnectionFactory. The logger uses the ConnectionSource to get JDBC connections, which is why it’s important to use a connection pool for better performance.

To configure the appender in the XML config file, you can use the JDBC tag:
<pre class="prettyprint prettyprinted" style=""><span class="tag">&lt;Appenders&gt;</span><span class="pln">
    </span><span class="tag">&lt;JDBC</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"JDBCAppender"</span><span class="pln"> </span><span class="atn">tableName</span><span class="pun">=</span><span class="atv">"logs"</span><span class="tag">&gt;</span><span class="pln">
        </span><span class="tag">&lt;DataSource</span><span class="pln"> </span><span class="atn">jndiName</span><span class="pun">=</span><span class="atv">"java:/comp/env/jdbc/LoggingDataSource"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
        </span><span class="tag">&lt;Column</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"date"</span><span class="pln"> </span><span class="atn">isEventTimestamp</span><span class="pun">=</span><span class="atv">"true"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
        </span><span class="tag">&lt;Column</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"logger"</span><span class="pln"> </span><span class="atn">pattern</span><span class="pun">=</span><span class="atv">"%logger"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
        </span><span class="tag">&lt;Column</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"level"</span><span class="pln"> </span><span class="atn">pattern</span><span class="pun">=</span><span class="atv">"%level"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
        </span><span class="tag">&lt;Column</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"message"</span><span class="pln"> </span><span class="atn">pattern</span><span class="pun">=</span><span class="atv">"%message"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
        </span><span class="tag">&lt;Column</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"exception"</span><span class="pln"> </span><span class="atn">pattern</span><span class="pun">=</span><span class="atv">"%ex{full}"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
    </span><span class="tag">&lt;/JDBC&gt;</span><span class="pln">
</span><span class="tag">&lt;/Appenders&gt;</span></pre>


## The FailoverAppender
Finally, let’s have a look at the FailoverAppender; this defines a primary appender and a list of backups that will step in to handle the logging in case the primary one fails.
In a production environment, having a failover strategy for your logging mechanism is always a good idea.
For example, you can configure a primary JDBCAppender, with a secondary the RollingFile and Console appenders in case a database connection cannot be established:
<pre class="prettyprint prettyprinted" style=""><span class="tag">&lt;Appenders&gt;</span><span class="pln">
    </span><span class="tag">&lt;JDBC</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"JDBCAppender"</span><span class="pln"> </span><span class="atn">tableName</span><span class="pun">=</span><span class="atv">"logs"</span><span class="tag">&gt;</span><span class="pln">
        </span><span class="tag">&lt;DataSource</span><span class="pln"> </span><span class="atn">jndiName</span><span class="pun">=</span><span class="atv">"java:/comp/env/jdbc/LoggingDataSource"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
        </span><span class="tag">&lt;Column</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"date"</span><span class="pln"> </span><span class="atn">isEventTimestamp</span><span class="pun">=</span><span class="atv">"true"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
        </span><span class="tag">&lt;Column</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"logger"</span><span class="pln"> </span><span class="atn">pattern</span><span class="pun">=</span><span class="atv">"%logger"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
        </span><span class="tag">&lt;Column</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"level"</span><span class="pln"> </span><span class="atn">pattern</span><span class="pun">=</span><span class="atv">"%level"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
        </span><span class="tag">&lt;Column</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"message"</span><span class="pln"> </span><span class="atn">pattern</span><span class="pun">=</span><span class="atv">"%message"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
        </span><span class="tag">&lt;Column</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"exception"</span><span class="pln"> </span><span class="atn">pattern</span><span class="pun">=</span><span class="atv">"%ex{full}"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
    </span><span class="tag">&lt;/JDBC&gt;</span><span class="pln">
</span><span class="tag">&lt;/Appenders&gt;</span></pre>


<h3><strong>Configuring Layouts</strong></h3>
<p>While the appenders are responsible for sending log messages to a destination, the layouts are used by appenders to define how a log message will be formatted.</p>
<p>Here’s a brief description of some of the more commonly used layouts that log4j2 offers:</p>
<ul>
<li><em>PatternLayout</em> – configures messages according to a String pattern</li>
<li><em>JsonLayout</em> – defines a JSON format for log messages</li>
<li><em>CsvLayout</em> –&nbsp;can be used to create messages in a CSV format</li>
</ul>
<h4><strong>The <em>PatternLayout</em></strong></h4>
<p>The first type of layout we’re going to look at is the&nbsp;<em>PatternLayout</em>. This is quite a flexible solution that gives you a lot of control over the final output of the log message.</p>
<p>The mechanism is primarily driven by a conversion pattern which contains conversion specifiers.&nbsp;Each specifier begins with the <em>%</em> sign, followed by modifiers that control things like width and color of the message, and a conversion character that represents the content, such as date or thread name.</p>
<p>Let’s look at an example of configuring a <em>PatternLayout</em> that configures log lines to show the date, thread, log level and log message with different colors for different log levels:</p>

<pre class="prettyprint prettyprinted" style=""><span class="tag">&lt;Console</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"Console"</span><span class="pln"> </span><span class="atn">target</span><span class="pun">=</span><span class="atv">"SYSTEM_OUT"</span><span class="tag">&gt;</span><span class="pln">
    </span><span class="tag">&lt;PatternLayout</span><span class="pln"> </span><span class="atn">pattern</span><span class="pun">=</span><span class="atv">"%d{HH:mm:ss.SSS} [%t] 
      %highlight{%level}{FATAL=bg_red, ERROR=red, WARN=yellow, INFO=green, DEBUG=blue} - %msg%n"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
</span><span class="tag">&lt;/Console&gt;</span></pre>

<p>These specifiers are well worth understanding in detail, so let’s have a closer look:</p>
<ul>
<li><em>%d{HH:mm:ss.SSS}</em> – outputs the date of the log event in the specified format</li>
<li><em>%t</em> – outputs the thread name</li>
<li><em>%level</em> – displays the log level of the message</li>
<li><em>%highlight{%level}</em> – is used to define the colors for the pattern between curly brackets</li>
<li><em>%msg%n</em> – outputs the log message</li>
</ul>


<p>You can read more about the full set of options for defining patterns in the log4j2 documentation on <em><a href="https://logging.apache.org/log4j/2.x/manual/layouts.html#PatternLayout" target="_blank" rel="noopener noreferrer" style="outline: none;">PatternLayout</a></em>.</p>

<h3><span style="color: #000000;"><strong>Configuring Filters</strong></span></h3>
<p>Filters&nbsp;in log4j2 are used <strong>to determine if a log message should be processed or skipped</strong>.</p>
<p>A filter can be configured for the entire configuration or at logger or appender level.</p>
<p>The library provides several types of filters that can be used:</p>
<ul>
<li><em>BurstFilter</em> – controls the number of log events allowed</li>
<li><em>DynamicThresholdFilter</em> – filters log lines based on certain attributes</li>
<li><em>RegexFilter</em> – filters messages based on whether they match a regular expression</li>
</ul>
<p>You can, for example, <strong>control the rate with which the application is allowed to log data</strong>.</p>
<p>In order to do, you can set up a <em>BurstFilter</em>&nbsp;and apply that to INFO messages:</p>
<pre class="prettyprint prettyprinted" style=""><span class="tag">&lt;Filters&gt;</span><span class="pln">
    </span><span class="tag">&lt;BurstFilter</span><span class="pln"> </span><span class="atn">level</span><span class="pun">=</span><span class="atv">"INFO"</span><span class="pln"> </span><span class="atn">rate</span><span class="pun">=</span><span class="atv">"10"</span><span class="pln"> </span><span class="atn">maxBurst</span><span class="pun">=</span><span class="atv">"100"</span><span class="tag">/&gt;</span><span class="pln">
</span><span class="tag">&lt;/Filters&gt;</span></pre>
<p>This will selectively ignore control the traffic of INFO level messages and below while making sure you’re not losing any of the more important messages above INFO.</p>
<p>In this case, <em>rate</em>&nbsp;defines the average logs messages that should be processed per second, and <em>maxBurst</em>&nbsp;controls the overall size of the traffic burst before the filter starts eliminating log entries.</p>
<p>Similarly, we can configure the appender only to accept log messages that match a specific regular expression:</p>
<pre class="prettyprint prettyprinted" style=""><span class="tag">&lt;Appenders&gt;</span><span class="pln">
    </span><span class="tag">&lt;JDBC</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"JDBCAppender"</span><span class="tag">&gt;</span><span class="pln">
      </span><span class="tag">&lt;RegexFilter</span><span class="pln"> </span><span class="atn">regex</span><span class="pun">=</span><span class="atv">"*jdbc*"</span><span class="pln"> </span><span class="atn">onMatch</span><span class="pun">=</span><span class="atv">"</span><span class="pln">ACCEPT</span><span class="atv">"</span><span class="pln"> </span><span class="atn">onMismatch</span><span class="pun">=</span><span class="atv">"</span><span class="pln">DENY</span><span class="atv">"</span><span class="tag">/&gt;</span><span class="pln">
    </span><span class="tag">&lt;/JDBC&gt;</span><span class="pln">
</span><span class="tag">&lt;/Appenders&gt;</span></pre>

<p>Overall, this filtering mechanism can be used with great precision to make sure each appender in your overall logging configuration is tracking the right information. The ability to only log very specific and relevant information generally leads to <strong>very quick root cause analysis, especially in complex systems&nbsp;</strong>– especially when coupled with <a href="https://stackify.com/best-log-viewer-prefix/">a powerful log viewing tool</a>.</p>
<h3><strong>Configuring Loggers</strong></h3>
<p>Besides the <em>Root</em> logger, we can also define additional <em>Logger</em> elements with different log levels, appenders or filters. Each <em>Logger</em> requires a name that can be used later to reference it:</p>
<pre class="prettyprint prettyprinted" style=""><span class="tag">&lt;Loggers&gt;</span><span class="pln">
    </span><span class="tag">&lt;Logger</span><span class="pln"> </span><span class="atn">name</span><span class="pun">=</span><span class="atv">"RollingFileLogger"</span><span class="tag">&gt;</span><span class="pln">
        </span><span class="tag">&lt;AppenderRef</span><span class="pln"> </span><span class="atn">ref</span><span class="pun">=</span><span class="atv">"RollingFileAppender"</span><span class="pln"> </span><span class="tag">/&gt;</span><span class="pln">
    </span><span class="tag">&lt;/Logger&gt;</span><span class="pln">
</span><span class="tag">&lt;/Loggers&gt;</span></pre>
<p>To write log messages using this particular Logger, you can obtain a reference to it using the <em>LogManager</em> class:</p>
<pre class="prettyprint prettyprinted" style=""><span class="typ">Logger</span><span class="pln"> rfLogger </span><span class="pun">=</span><span class="pln"> </span><span class="typ">LogManager</span><span class="pun">.</span><span class="pln">getLogger</span><span class="pun">(</span><span class="str">"RollingFileLogger"</span><span class="pun">);</span><span class="pln">
rfLogger</span><span class="pun">.</span><span class="pln">info</span><span class="pun">(</span><span class="str">"User info updated"</span><span class="pun">);</span></pre>
<p>Another very common way to structure the hierarchy of these loggers is based on the Java class:</p>
<pre class="prettyprint prettyprinted" style=""><span class="typ">Logger</span><span class="pln"> logger </span><span class="pun">=</span><span class="pln"> </span><span class="typ">LogManager</span><span class="pun">.</span><span class="pln">getLogger</span><span class="pun">(</span><span class="typ">MyServiceTest</span><span class="pun">.</span><span class="kwd">class</span><span class="pun">);</span></pre>


































