# Error Logging in Prysm for Desktop

Prysm for desktop includes error logging. You can open the logs to
troubleshoot issues you might experience with the app.

!!! tip "Tip"

    You can also contact Prysm support and share your logs with them to get help with issues.

Both the main Prysm for desktop app and its companion Prysm Sharing
Agent app include this error logging. The following sections include the
details for both apps.

## Opening Log Files

To open log files for troubleshooting, you locate and open the logs in
the Logs folder:

1.  On the device where Prysm for desktop is installed, **navigate**
    to:

    `C:\Users\<Username>\AppData\Local\Packages\PrysmInc.Prysm_gcnfe4jvxweze\LocalState\logs`

    !!! note "Note"

        If you want to open log files for Prysm Sharing Agent, on the device where it's installed, navigate to:

    `C:\Users\<Username>\AppData\Local\Packages\PrysmInc.PrysmSharingExtension_gcnfe4jvxweze\LocalState\logs`

2.  In the folder, you see the log files, which are .txt files named for
    the day of activities that the log contains, such as:
    - `log-20180327.txt` (for Prysm for desktop)
    - `prysm-desktop-ext-log-20180327.txt` (for Prysm Sharing Agent)
3.  **Open** the log file corresponding to the time your issue occurred
    (in Notepad or your preferred text editor).

## Configuring Logging

Prysm recommends using the preconfigured logging settings. If you find
that those aren't working for you, you can locate and edit the
configuration .xml file to change the configuration:

1.  On the device where Prysm for desktop is installed, **navigate** to:

    `C:\Users\<Username>\AppData\Local\Packages\PrysmInc.Prysm_gcnfe4jvxweze\LocalState\UserConfig.xml`

    If you're configuring logging for Prysm Sharing Agent, on the device where it's installed, navigate to:

    `C:\Users\<Username>\AppData\Local\Packages\PrysmInc.PrysmSharingExtension_gcnfe4jvxweze\LocalState\UserConfig.xml`

2.  Open **UserConfig.xml** (in Notepad or your preferred text editor).

    The contents of the file look similar to the following:

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
        <appSettings>
        <add key="AppInsightsLogLevel" value="Warning">
        <add key="LogFileLogLevel" value="Information">
        <add key="AllowRememberMe" value="false"/>
        </appSettings>
        </configuration>

    !!! note "Note"

        The following line of the file is relevant only if you use Microsoft Azure Application Insights:

        `<add key="AppInsightsLogLevel" value="Warning">`

        You can set this Application Insights log level value to a different value than you set for LogFileLogLevel to configure different levels of logging for Application Insights and for the log file on the device.

3.  If you need to change the preconfigured level of logging, change the
    values to other standard debugging level values, such as:
    - Information
    - Warning
    - Error
    - Trace
    - Fatal
    - All
    - Off
4.  **Save** and **close** the UserConfig.xml file.
