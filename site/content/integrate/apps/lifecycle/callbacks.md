---
title: Callbacks
description: Methods that the Apps plugin will invoke on the App
weight: 10
---
The App [manifest]({{<ref "/integrate/apps/structure/manifest">}}) defines a number of callback methods ([Calls]({{<ref "/integrate/apps/structure/call">}})) that are invoked by the Mattermost server at various times. If a callback isn't defined in the manifest, it won't be executed by the Mattermost server.
The following table lists supported callbacks:

| Name                  | Manifest Parameter       | Details                                                                                                                                                                                                                                                                                                                                                                             |
|-----------------------|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `OnInstall`           | `on_install`             | OnInstall gets invoked when a System Admin installs the App with a `/apps install` command. It may return another call to the app, or a form to display.                                                                                                                                                                                                                            |
| `OnUninstall`         | `on_uninstall`           | OnUninstall gets invoked when a System Admin uses the `/apps uninstall` command, before the app is actually removed.                                                                                                                                                                                                                                                                |
| `GetOAuth2ConnectURL` | `get_oauth2_connect_url` | GetOAuth2ConnectURL is called when the App's "connect to third party" link is selected to start an OAuth flow. It must return Data set to the remote OAuth2 redirect URL. A "state" string is created by the proxy, and is passed to the app as a value. The state is a one-time secret that is included in the connect URL, and will be used to validate OAuth2 complete callback. |
| `OnOAuth2Complete`    | `on_oauth2_complete`     | OnOAuth2Complete gets called upon successful completion of the remote (third party) OAuth2 flow, and after the "state" has already been validated. It gets passed the URL query as Values. The App should obtain the OAuth2 user token, and store it persistently for future use using `appclient.StoreOAuth2User`.                                                                 |
| `OnRemoteWebhook`     | `on_remote_webhook`      | OnRemoteWebhook gets invoked when an HTTP webhook is received from a remote system, and is optionally authenticated by Mattermost. The request is passed to the call serialized as HTTPCallRequest (JSON).                                                                                                                                                                          |