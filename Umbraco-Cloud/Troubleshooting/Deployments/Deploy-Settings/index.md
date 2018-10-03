# Timeout issues
Umbraco Deploy have a few built in timeouts, which on larger sites might needs to be modified. You will usually see these timeouts in the backoffice with an exception mentioning a timeout. It will be as part of a full restore or a full deploy of an entire site. In the normal workflow you should never hit these timeouts.

The defaults will cover most though. Changing the defaults is just updating the `/Config/UmbracoDeploy.settings.config`. There are four settings available, which all uses the default timeout which currently is set for 8 minutes.
- sessionTimeout
- sourceDeployTimeout
- httpClientTimeout
- databaseCommandTimeout

The settings are simply set on the deploy element in the file. All settings are in seconds:

    <?xml version="1.0" encoding="utf-8"?>
    <settings xmlns="urn:umbracodeploy-settings">
      <deploy sessionTimeout="1200" sourceDeployTimeout="1200" httpClientTimeout="1200"/>
    </settings>

### Large media libraries
If you are often hitting the timeouts on content transfer or restores it is likely because your media library is too large, it is recommended that you switch to Blob storage of media files if you have a media library larger than 1gb. See how [here!](../../../Set-Up/Media)