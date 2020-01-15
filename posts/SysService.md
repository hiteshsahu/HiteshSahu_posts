### Android Services

Android services are system component which allows performing a longer-running operation while not interacting with the user.

Just like Activities, Service runs on Main Thread and has a life cycle but unlike Activity, Services do not have a UI.

Anyone can use & create Service or its direct subclasses IntentServcice in their Apps and it works pretty well.

Common usages are: Playing Media, Downloading files from the Internet, etc.

### System Services

System services are a direct derivative of SystemService class. They reside in com.android.server packagein AOSP tree

https://android.googlesource.com/platform/frameworks/base/+/refs/heads/master/services/

System Services also run in Main Thread so if you want to do some CPU intensive task then you much reuse HandlerThread architecture of AOSP.

### What makes System service different from Android Services?

- System Services are started by SystemServer hence they run as a System process which gives them additional privileges which normal Android Service will never get hence they are named name SystemService.

Example: You can't use Instrumentation to inject events in App other than yours because you will need INJECT_EVENT permission which is not granted to normal apps. SystemService can do that because they have elevated access.

The simple example of System Service :

      package com.android.server.appwidget;
      import android.content.Context;
      import com.android.server.AppWidgetBackupBridge;
      import com.android.server.FgThread;
      import com.android.server.SystemService;
      /**
       * SystemService that publishes an IAppWidgetService.
       */
      public class AppWidgetService extends SystemService {
          private final AppWidgetServiceImpl mImpl;
          public AppWidgetService(Context context) {
              super(context);
              mImpl = new AppWidgetServiceImpl(context);
          }
          @Override
          public void onStart() {
              mImpl.onStart();
              publishBinderService(Context.APPWIDGET_SERVICE, mImpl);
              AppWidgetBackupBridge.register(mImpl);
          }
          @Override
          public void onBootPhase(int phase) {
              if (phase == PHASE_ACTIVITY_MANAGER_READY) {
                  mImpl.setSafeMode(isSafeMode());
              }
          }
          @Override
          public void onStopUser(int userHandle) {
              mImpl.onUserStopped(userHandle);
          }
          @Override
          public void onSwitchUser(int userHandle) {
              mImpl.reloadWidgetsMaskedStateForGroup(userHandle);
          }
      }
      
      
