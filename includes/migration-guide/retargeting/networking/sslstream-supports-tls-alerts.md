### <a name="sslstream-supports-tls-alerts"></a>SslStream 支持 TLS 警报

|   |   |
|---|---|
|详细信息|TLS 握手失败后，第一个 I/O 读取/写入操作将引发带有内部 <xref:System.ComponentModel.Win32Exception?displayProperty=name> 异常的 <xref:System.IO.IOException?displayProperty=name>。 可使用此 [Schannel 文档](https://msdn.microsoft.com/library/windows/desktop/dd721886%28v=vs.85%29.aspx)将 <xref:System.ComponentModel.Win32Exception?displayProperty=name> 的 <xref:System.ComponentModel.Win32Exception.NativeErrorCode?displayProperty=name> 代码映射到远程参与方的 TLS 警报。有关详细信息，请参阅 [RFC 2246：第 7.2.2 节：错误警报](https://tools.ietf.org/html/rfc2246#section-7.2.2)。 .NET 4.6.2 及以下版本中的行为是，如果另一方握手失败并随后立即拒绝连接，则传输通道（通常是 TCP 连接）将在写入或读取期间超时。|
|建议|调用网络 I/O API 的应用程序（例如 <xref:System.IO.Stream.Read(System.Byte[],System.Int32,System.Int32)>/<xref:System.IO.Stream.Write(System.Byte[],System.Int32,System.Int32)>）应处理 <xref:System.IO.IOException> 或 <xref:System.TimeoutException?displayProperty=name>。默认情况下，从 .NET 4.7 开始启用 TLS 警报功能。 在 .NET 4.7 或更高版本系统上运行的面向 .NET 4.0 - .NET 4.6.2 的应用程序将禁用该功能以保持兼容性。下列配置 API 可用于为在 .NET 4.7 或更高版本的框架上运行的 .NET 4.6 及更高版本应用程序启用或禁用该功能。<ul><li>以编程方式：</li></ul>必须是应用程序执行的第一件事，因为 ServicePointManager 将只初始化一次：<pre><code class="language-C#">AppContext.SetSwitch(&quot;TestSwitch.LocalAppContext.DisableCaching&quot;, true);&#13;&#10;AppContext.SetSwitch(&quot;Switch.System.Net.DontEnableTlsAlerts&quot;, true); // Set to &#39;false&#39; to enable the feature in .NET 4.6 - 4.6.2.&#13;&#10;</code></pre><ul><li>AppConfig：</li></ul><pre><code class="language-XML">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Net.DontEnableTlsAlerts=true&quot;/&gt;&#13;&#10;&lt;!-- Set to &#39;false&#39; to enable the feature in .NET 4.6 - 4.6.2. --&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre><ul><li>注册表项（计算机全局）：</li></ul>将值设置为“false”以在 .NET 4.6 - 4.6.2 中启用该功能。<pre><code>Key = HKLM\SOFTWARE\[Wow6432Node\]Microsoft\.NETFramework\AppContext\Switch.System.Net.DontEnableTlsAlerts&#13;&#10;Type = String&#13;&#10;Value = &quot;true&quot;&#13;&#10;</code></pre>|
|范围|边缘|
|版本|4.7|
|类型|重定目标|
|受影响的 API|<ul><li><xref:System.Net.Security.SslStream?displayProperty=nameWithType></li><li><xref:System.Net.WebRequest?displayProperty=nameWithType></li><li><xref:System.Net.HttpWebRequest?displayProperty=nameWithType></li><li><xref:System.Net.FtpWebRequest?displayProperty=nameWithType></li><li><xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType></li><li><xref:System.Net.Http?displayProperty=nameWithType></li></ul>|
