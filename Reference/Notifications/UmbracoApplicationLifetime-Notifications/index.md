---
versionFrom: 9.3.0
versionTo: 10.0.0
meta-title: Umbraco Application Lifetime Notifications
description: Represents an Umbraco application lifetime (starting, started, stopping, stopped) notification
---

# Umbraco Application Lifetime Notifications

Umbraco application lifetime notifications are published for the starting, started, stopping, and stopped events of the Umbraco runtime. These events implement the `IUmbracoApplicationLifetimeNotification` interface that contains a single `IsRestarting` property.

A Umbraco application is restarted after an install or upgrade has been completed, so you can use this property to prevent running code twice (on initial boot and restart). To prevent running code when the application is in the install or upgrade state, inject an `IRuntimeState` instance in your notification and inspect the `Level` property instead.

## Usage

Example usage of the UmbracoApplicationLifetime notifications:

```C#
using Microsoft.Extensions.Logging;
using Umbraco.Cms.Core.Composing;
using Umbraco.Cms.Core.Events;
using Umbraco.Cms.Core.DependencyInjection;
using Umbraco.Cms.Core.Notifications;

public class UmbracoApplicationNotificationComposer : IComposer
{
    public void Compose(IUmbracoBuilder builder)
    {
        builder.AddNotificationHandler<UmbracoApplicationStartingNotification, UmbracoApplicationNotificationHandler>();
        builder.AddNotificationHandler<UmbracoApplicationStartedNotification, UmbracoApplicationNotificationHandler>();
        builder.AddNotificationHandler<UmbracoApplicationStoppingNotification, UmbracoApplicationNotificationHandler>();
        builder.AddNotificationHandler<UmbracoApplicationStoppedNotification, UmbracoApplicationNotificationHandler>();
    }
}

public class UmbracoApplicationNotificationHandler : INotificationHandler<UmbracoApplicationStartingNotification>, INotificationHandler<UmbracoApplicationStartedNotification>, INotificationHandler<UmbracoApplicationStoppingNotification>, INotificationHandler<UmbracoApplicationStoppedNotification>
{
    private readonly ILogger _logger;

    public UmbracoApplicationNotificationHandler(ILogger<UmbracoApplicationNotificationHandler> logger) => _logger = logger;

    public void Handle(UmbracoApplicationStartingNotification notification) => Log(notification, notification.IsRestarting);

    public void Handle(UmbracoApplicationStartedNotification notification) => Log(notification, notification.IsRestarting);

    public void Handle(UmbracoApplicationStoppingNotification notification) => Log(notification, notification.IsRestarting);

    public void Handle(UmbracoApplicationStoppedNotification notification) => Log(notification, notification.IsRestarting);

    private void Log(INotification notification, bool isRestarting) => _logger.LogInformation("{Type} - {IsRestarting}", notification.GetType().Name, isRestarting);
}
```

## Notifications

<table>
  <tr>
    <th>Notification</th>
    <th>Members</th>
    <th>Description</th>
  </tr>

  <tr>
    <td>UmbracoApplicationStartingNotification</td>
    <td>
      <ul>
        <li>RuntimeLevel RuntimeLevel</li>
        <li>bool IsRestarting</li>
      </ul>
    </td>
    <td>
        Triggered when the application is starting after all <code>IComponents</code> are initialized but before any incoming requests are accepted.<br />
      <ol>
        <li>RuntimeLevel: Gets the runtime level.</li>
        <li>IsRestarting: Gets a value indicating whether Umbraco is restarting (e.g. after an install or upgrade).</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td>UmbracoApplicationStartedNotification</td>
    <td>
      <ul>
        <li>bool IsRestarting</li>
      </ul>
    </td>
    <td>
      Triggered when the application has fully started and is accepting incoming requests.<br />
      <ol>
        <li>IsRestarting: Gets a value indicating whether Umbraco is restarting (e.g. after an install or upgrade).</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td>UmbracoApplicationStoppingNotification</td>
    <td>
      <ul>
        <li>bool IsRestarting</li>
      </ul>
    </td>
    <td>
      Triggered when the application is performing a graceful shutdown after all <code>IComponents</code> are terminated.<br />
      <ol>
        <li>IsRestarting: Gets a value indicating whether Umbraco is restarting (e.g. after an install or upgrade).</li>
      </ol>
    </td>
  </tr>

  <tr>
    <td>UmbracoApplicationStoppedNotification</td>
    <td>
      <ul>
        <li>bool IsRestarting</li>
      </ul>
    </td>
    <td>
      Triggered when the application has performed a graceful shutdown.<br />
      <ol>
        <li>IsRestarting: Gets a value indicating whether Umbraco is restarting (e.g. after an install or upgrade).</li>
      </ol>
    </td>
  </tr>

</table>
