# Logic

This page attempts to explain the logical flow of how to control the water heater.

1. By default, the user does not have a control privileges via a session. During an unprivileged session, the user may access: `heaterName.cgi`, `version.cgi`, `getParams.cgi` and `getInfo.cgi`
2. A user may escalate their session to control privileges via `ctrl.cgi` by requesting Heating Control or Bath Fill Control. While having control privileges, this exposes further information under `getInfo.cgi` and `set.cgi`.
3. The system will wait for the session timer and then timeout the control privileges. Using a `set.cgi` command will reset your session timer as will turning on the hot water.
4. Your control privileges expire after the session timer runs out.