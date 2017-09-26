# Monitoring macOS Content Caching with Zabbix

With the release of macOS 10.13 High Sierra, Apple have made a big change to
their App and iCloud content caching services.  This function used to be part
of their add-on macOS Server package, but with 10.13 they've moved the feature
into the regular macOS operating system.  This means that any Macintosh
computer running 10.13 High Sierra can serve as a content cache for a local
network.

## Why do I want to run this?

If you have more than one Apple device on your local netork you can see
benefits from running the caching service.  All iOS and macOS application
downloads pass through the cache, meaning you only have to download them once.
This saves your bandwidth and means that cached installs can run much faster.

If you enable the optional iCloud caching, you'll get similar (and even more
noticeable) performance improvements for iCloud Photo Library syncing, iCloud
Drive, and other data sharing across your Apple devices.

Take a photo with your iPhone and it's uploaded via the caching service to
Apple's iCloud servers.  When your MacBook syncs the photo via iCloud, it
doesn't have to download from "the cloud" but instead pulls the local, cached
image.

## More Information

- [This code and
  documentation](https://github.com/nugget/zbx-macos-content-caching)
- [Prepare for changes to Content Caching in macOS High
  Sierra](https://support.apple.com/en-vn/HT208025)

# What was lost in the migration

The old macOS Server version had fancy logs and graphs that allowed you to
monitor cahing performance and activity to keep an eye on the efficacy and
health of the service.  When they moved it all to regular macOS all that stuff
disappeared.  It's basically just an on/off checkbox.

Instead, there's just a command-line tool that outputs the raw data from the
service.  You can see the stats  by running `AssetCacheManagerUtil status` from
a Terminal window.

# Monitoring via Zabbix

If you are running [Zabbix](https://www.zabbix.com) 3.4 I've included
a template which will make all kinds of pretty graphs and related items for the
caching service.  I can guarantee that it's not worth installing Zabbix just
for this, but if you're already running it then the template is quite easy to
get going.  Just import the xml template file also found in this GitHub repo.

## Example Output
```
2017-09-25 19:57:12.288 AssetCacheManagerUtil[20807:3449689] Built-in caching server status: {
    Activated = 1;
    Active = 1;
    CacheDetails =     {
        Books = 174082575;
        "Mac Software" = 24423474829;
        Other = 1738767542;
        iCloud = 12954227645;
        "iOS Software" = 37416034017;
    };
    CacheFree = 298293413392;
    CacheLimit = 375000000000;
    CacheStatus = OK;
    CacheUsed = 76706586608;
    Parents =     (
    );
    Peers =     (
    );
    PersonalCacheFree = 362045772355;
    PersonalCacheLimit = 375000000000;
    PersonalCacheUsed = 12954227645;
    Port = 49242;
    PrivateAddresses =     (
        "10.10.3.21"
    );
    PublicAddress = "aaa.bbb.ccc.ddd";
    RegistrationStatus = 1;
    RestrictedMedia = 0;
    ServerGUID = "DEADBEEF-FOOD-A1AB-1040-EAB10CFEA18";
    StartupStatus = OK;
    TotalBytesDropped = 679075;
    TotalBytesImported = 128672588;
    TotalBytesReturnedToChildren = 0;
    TotalBytesReturnedToClients = 35798809867;
    TotalBytesReturnedToPeers = 0;
    TotalBytesStoredFromOrigin = 21063186167;
    TotalBytesStoredFromParents = 0;
    TotalBytesStoredFromPeers = 0;
}
```
