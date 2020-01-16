## Android Services
> Android services are system component which allows performing a longer-running operation while not interacting with the user.

- Just like Activities, Service runs on Main Thread and has a life cycle but unlike Activity, Services do not have a UI.

- Anyone can use & create Service or its direct subclasses IntentServcice in their Apps and it works pretty well.

Common usages are: Playing Media, Downloading files from the Internet, etc.

## System Services
> System services are a direct derivative of SystemService class. They reside in com.android.server packagein AOSP tree. 


https://android.googlesource.com/platform/frameworks/base/+/refs/heads/master/services/

enter image description here


- You can fetch list of System service using :
      adb shell services list

- System Services also run in Main Thread so if you want to do some CPU intensive task then you much reuse HandlerThread architecture of AOSP.


## What makes System service different from Android Services?
>System Services are started by SystemServer hence they run as a System process which gives them additional privileges which normal Android Service will never get hence they are called SystemService.

Example: You can't use Instrumentation to inject events in App other than yours because you will need INJECT_EVENT permission which is not granted to normal apps. SystemService can do that because they have elevated access.

A simple example of System Service :

 - The constructor is called and provided with the system Context to initialize the system service.
-   TheonStart() is called to get the service running.

            package com.android.server.dispatcher;
            import android.annotation.TargetApi;
            import android.app.Instrumentation;
            import android.content.BroadcastReceiver;
            import android.content.Context;
            import android.content.Intent;
            import android.content.IntentFilter;
            import android.dispatcher.DispatchManager;
            import android.os.Build;
            import android.os.Handler;
            import android.os.Message;
            import android.util.Slog;

            import com.android.server.ServiceThread;
            import com.android.server.SystemService;

            import static android.os.Process.THREAD_PRIORITY_FOREGROUND;

            /**
             * Simple System Service
             */
            @TargetApi(Build.VERSION_CODES.CUPCAKE)
            public class AwesomeSystemService extends SystemService implements Handler.Callback {

                //-----------CONSTANTS-----------------

             //TAGS
             public static final String TAG = "KeyDispatcherService";
            //Handler Message: What
            private static final int DO_SOMETHING = 1;

             //-----------VARIABLES-----------------
             private Context mContext;
             private final Handler mHandler;
             private final ServiceThread mHandlerThread

             @TargetApi(Build.VERSION_CODES.CUPCAKE)
                public AwesomeSystemService(Context context) {
                 super(context);
                mContext = context;

               //Start Handler Thread for performing task in background
               mHandlerThread = new ServiceThread(TAG, THREAD_PRIORITY_FOREGROUND,false /*allowIo*/);
               mHandlerThread.start();

               //Get Handler from Looper we can now use this to pass message on Handler thread
               mHandler = new Handler(mHandlerThread.getLooper(), this);

               Slog.i(TAG, "KeyDispatcherService Constructor!!!"+dispatchManager)

               }

                @Override
                public void onStart() {
                Slog.e(TAG, "KeyDispatcherService Service Starts!!!"+dispatchManager);
                       //Send Messages to Handler to perform any task in Background

                        int argument = Math.random()

                 mHandler.sendMessage(mHandler.obtainMessage(DO_SOMETHING, argument, 0, null));
              }

              /**
             * Handles Message passed to Handler Thread in background
             * @param message
             * @return
             */
             @Override
                public boolean handleMessage(Message message) {
                Slog.e(TAG, "Message Received!!!: "+message.arg1 + " WHAT "+message.what);

                // Add Your Logic Here 
               return true;
             }
            }


Register and start Service from SystemServer

Import your service class

            import com.android.server.dispatcher.AwesomeService;

Add Class Name among the constant fields

            // Customization
            private static final String KEY_DISPATCHER_SERVICE_CLASS =
                    " com.android.server.dispatcher.AwesomeService";


In  startOtherServices() method start your service like this:



            //Start My Awesome Service
            traceBeginAndSlog("StartAwesomeService");
            mSystemServiceManager.startService(AwesomeService.class);
            traceEnd();
