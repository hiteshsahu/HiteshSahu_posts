**SystemSevice:** This is the base class for services running in the system process. We can Override and implement the lifecycle event callback methods as needed.

### The lifecycle of a SystemService:
/frameworks/base/services/core/java/com/android/server/SystemService.java 
     
 The constructor is called and provided with the system Context to initialize the system service.
 onStart() is called to get the service running.  

 The service should publish its binder interface at this point using publishBinderService().

   It may also publish additional local interfaces that other services within the system server may use to access 
   privileged internal functions.onBootPhase(int phase) is called as many times as there are boot phases until 
   PHASE_BOOT_COMPLETED} is sent, which is the last boot phase. Each phase is an opportunity to do special work
   The different phases are:
     
- PHASE_WAIT_FOR_DEFAULT_DISPLAY 
- PHASE_LOCK_SETTINGS_READY
- PHASE_LOCK_SETTINGS_READY
- PHASE_ACTIVITY_MANAGER_READY
- PHASE_THIRD_PARTY_APPS_CAN_START
- PHASE_BOOT_COMPLETED

  
For systems supporting multiuser scenarios the following callbacks are also provided as the system services would be common 
for all processes
 
 - onStartUser()
 - onStopUser()
 - onSwitchUser()
 - onCleanpUser()

 
    All lifecycle methods are called from the system server's main looper thread.
 
**SystemServiceManager:** Manages creating, starting, and other lifecycle events of SystemServices
/frameworks/base/services/core/java/com/android/server/SystemServiceManager.java 

This class is instantiated by systemserver class to start different system services.

As of now not all services in SysterServer are extended from SystemService. However that seems to be the targeted approach

SysterServer:
/frameworks/base/services/java/com/android/server/SystemServer.java 

The main method of system server class calls the run method.
Within run set time to 1970.(earliset supported time)
Set "persist.sys.dalvik.vm.lib.2" property to current runtime.(In case this is the first boot after update where runtime has changed)
Set binder threads and main threads on foreground priority.
- Looper.prepareMainLooper();
- Prepare main looper
- System.loadLibrary("android_servers");
- createSystemContext(); 
Initialize System Context. - Runs the activity threads main method. Get System Context by running contextImpl;
Create SystemServiceManager and add services as local services.

   startBootstrapServices();
      Start Boot Critical Services
     - Installer
     - ActivityManager
     - PowerManager
     - LightService
     - DisplayManagerService
 mSystemServiceManager.startBootPhase(SystemService.PHASE_WAIT_FOR_DEFAULT_DISPLAY)
     - PackageManager
     - UserManager
     - Sensor

  startCoreServices();
     Start Other Essential Services
     - Battery
     - UsageStats
     - WebviewUpdate
  startOtherServices();
      Start Other Misc Services
      - SchedulingPolicyService
      - TelecomLoaderService
      - TelephonyRegistry
      - EntropyMixer
     - CameraService
     - AccountManagerService
     installSystemProviders()
     - VibratorService
     - ConsumerIrService
     Init WatchDog
     - AlarmManagerService
     - WindowManagerService
     - InputManagerService
     - BluetoothService
    -  InputMethodManagerService
     - AccessibilityManagerService
      wm.displayReady(); //make display ready
      - MountService
      - UiModeManagerService
      mPackageManagerService.performBootDexOpt();   
     showBootMessage(com.android.internal.R.string.android_upgrading_starting_apps)
      - LockSettingsService
      - PersistentDataBlockService
      - DeviceIdleController
     - DevicePolicyManagerService
     - StatusBarManagerService
     - ClipboardService
     - NetworkManagementService
     - TextServicesManagerService
     - NetworkScoreService
     - NetworkStatsService
     - NetworkPolicyManagerService
     - WifiP2pService
     - WifiService
     - WifiScanningService
     - WifiRttService
     - EthernetService
     - ConnectivityService
     - NetworkServiceDiscoveryService
     - UpdateLockService
     - MountService
     - NotificationManagerService
     - LocationManagerService
     - CountryDetectorService
     - SearchManagerService
     - DropBoxManagerService
     - WallpaperManagerService
     - AudioService
    -  DockObserver
     - MidiService
     - UsbService
     - SerialService
     - TwilightService
    -  JobSchedulerService
     - BackUpManagerService
     - AppWidgetService
     - VRServices
    -  GestureLauncherService
     - DiskStatsService
    - SamplingProfilerService
    - NetworkTimeUpdateService
    - CommonTimeManagementService
    - CertBlacklister
    - DreamManagerService
    - AssetAtlasService
    - GraphicsStatsService
    - PrintMnagerService
    - RestrictionsManagerService
   -  MediaSessionService
    - HdmiControlService
    - TvInputManagerService
    - MediaRouterService
    - TrustManagerService
    - FingerprintService
    - BackgroundDexOptService
    - LauncherAppsService
    - MediaProjectionManagerService
    - MmsServiceBroker
    
mSystemServiceManager.startBootPhase(SystemService.PHASE_LOCK_SETTINGS_READY)

mSystemServiceManager.startBootPhase(SystemService.PHASE_SYSTEM_SERVICES_READY)

Make SystemReady calls for all system services

    -  wm.systemReady();
    -  mPowerManagerService.systemReady(mActivityManagerService.getAppOpsService())
    -  mPackageManagerService.systemReady();
    -  mDisplayManagerService.systemReady(safeMode, mOnlyCore);
    -  mActivityManagerService.systemReady();

SystemServer registers a callback. It will call back into us once it has gotten to the state
   where third party code can really run (but before it has actually started launching the initial applications), for us to complete our initialization.

          ******ActivityManager systemReady runs here ******************
  
 -  mSystemServiceManager.startBootPhase(SystemService.PHASE_ACTIVITY_MANAGER_READY);
 -  startSystemUi(context);
      
  SystemReady for other services are called
 -   Watchdog.getInstance().start();
 - mSystemServiceManager.startBootPhase(SystemService.PHASE_THIRD_PARTY_APPS_CAN_START);
 -  Call systemRunning() for services
  
   // Loop forever.
   Looper.loop();
