
# Android Slices Tutorial


>  ### Continuing in last post here I gone explain how to create Slices for your application. If you have not read it please go through my previous post [Understanding Slices](http://hiteshsahu.com/blogs/android_slices) to get your setup ready.

Using predefined templates you can create:

1.  __Basic slices__ Simple list item
2.  __Advance Slices__ : 
  - With List 
  - List With Header 
  - Grid 
  - With Switches 
  - With Header containing Multiple Actions 
  - Slider Bar
  - Progress Bar 
  - Combination of Slices


## First Get your setup ready

You need to install [__Slice Viewer__](https://github.com/googlesamples/android-SliceViewer/releases) apk to view your slices. Download latest relase of slice viewer apk from [https://github.com/googlesamples/android-SliceViewer/releases](https://github.com/googlesamples/android-SliceViewer/releases) & install it using command:

> `adb install -r -t slice-viewer.apk`

where :
 - -r: replaces existing application
 - -t: allow test packages


Now you can view your Slice by running the following command:

> `adb shell am start -a android.intent.action.VIEW -d slice-content://com.hiteshsahu.slicedemo/`

It is pretty much like loading different pages of a website by changing URLS

For example:
 - __content://com.hiteshsahu.slicedemo/__        :  load home slice
 - __content://com.hiteshsahu.slicedemo/about__   :  load about slice
 - __content://com.hiteshsahu.slicedemo/contact__ :  load contact slice

and so on see the result below.

####  Every time you gone launch a new slice with this adb command then that slice will be automatically get added in Slice Viewer demo app. Which you can see any time later. 

 Same can be achieve with the help of Android studio but I prefer command line tool. COmmand line tool is less complicate and less overhead.  
 
 
 ### Uninstall sliceviewer
 
 If you have uninstalled _sliceviewer.apk_ by drag and drop in Launcher app  then chances are you might no longer will be able to install it back via adb command. This is a weird bug I found. In that case you will need to uninstall sliceviewer using this command. 
 
     adb  uninstall com.example.android.sliceviewer

## Building Custom Slices

As already described in my previous post [Understanding SLices](http://hiteshsahu.com/blogs/android_slices) you can use Android Studio to create Slices as shown in pic below:

<img src="http://hiteshsahu.com/assets/img/blogs/slices/create_slice.png" />

Once Your slice provider is created you can create custom slices based on URL in  onBindSlice


        /**
        *  Show appropriate Slice with pending action based on URI path
        *
        *   content://com.hiteshsahu.slicedemo/ : home
        *   content://com.hiteshsahu.slicedemo/about : about
        *   content://com.hiteshsahu.slicedemo/contact : contact
        *   content://com.hiteshsahu.slicedemo/etc : 404
        */
        override fun onBindSlice(sliceUri: Uri): Slice? {
            // small-builders-ktx for a nicer interface in Kotlin.

            val context = context ?: return null

            // get path from Uri
            val sliceUriPath = sliceUri.path

            // Create custom actions based on URI path eg. showing contact or home
            val activityActionForPath = crateActionForPath(sliceUriPath) ?: return null

            // return a small based on path and action
            when (sliceUriPath) {
                "/" -> {
                    // HOME URI
                    return ListBuilder(context, sliceUri, ListBuilder.INFINITY)
                            .addRow(
                                    ListBuilder.RowBuilder()
                                            .setTitle("Go to  Home \uD83C\uDFE0")
                                            .setPrimaryAction(activityActionForPath))
                            .build()
                }

                "/about" -> {

                    // ABOUT URI
                    return ListBuilder(context, sliceUri, ListBuilder.INFINITY)
                            .addRow(
                                    ListBuilder.RowBuilder()
                                            .setTitle("Go to About  \uD83D\uDE00 ")
                                            .setPrimaryAction(activityActionForPath))
                            .build()
                }

                "/contact" -> {
                    // CONTACT URI
                    return ListBuilder(context, sliceUri, ListBuilder.INFINITY)
                            .addRow(
                                    ListBuilder.RowBuilder()
                                            .setTitle("Go to Contact \uD83D\uDCDE")
                                            .setPrimaryAction(activityActionForPath))
                            .build()
                }
                else -> {
                    // ANYTHING ELSE
                    return ListBuilder(context, sliceUri, ListBuilder.INFINITY)
                            .addRow(
                                    ListBuilder.RowBuilder()
                                            .setTitle("Error 404 You are long way from home. \uD83D\uDC31")
                                            .setPrimaryAction(activityActionForPath))
                            .build()
                }
            }
        }


Code here is self explanotary we are trying to create a List with a simple row containing Title and Action.

>  Action is a __SliceAction__ object coontaining __Pending intent__ which may contain data which you want to share with your App.  
 

See how I am creating a Intent to launch my activity and data based on path URI. 

            val pendingIntent = Intent(context, SlicesDemoActivity::class.java)
             pendingIntent.putExtra(PAGE_KEY, ScreenNameEnum.HOME.name)

 Next I created a PendingIntent  using that intent with flag __PendingIntent.FLAG_ONE_SHOT__  and new request id                                                 _System.currentTimeMillis().toInt()_
       

            PendingIntent.getActivity( context,
                                                System.currentTimeMillis().toInt(),
                                                pendingIntent,
                                                PendingIntent.FLAG_ONE_SHOT)

Now I can create a Slice Action 


   return SliceAction.create(
                            PendingIntent.getActivity(
                                    context,
                                    System.currentTimeMillis().toInt(),
                                    pendingIntent,
                                    PendingIntent.FLAG_ONE_SHOT),
                            IconCompat.createWithResource(context,
                                    R.drawable.icon),
                            ListBuilder.ICON_IMAGE,
                            "Open App")

Complete Code for crateting __Action__ based on slice path:
    
        /**
        * Craete actions based on path
        */
        private fun crateActionForPath(slicePath: String?): SliceAction? {

            val pendingIntent = Intent(context, SlicesDemoActivity::class.java)

            // add payload based on path
            when (slicePath) {
                "/" -> { // HOME URI

                    // set data in intent
                    pendingIntent.putExtra(PAGE_KEY, ScreenNameEnum.HOME.name)

                    // Create pending intent with ICON_IMAGE size
                    return SliceAction.create(
                            PendingIntent.getActivity(
                                    context,
                                    System.currentTimeMillis().toInt(),
                                    pendingIntent,
                                    PendingIntent.FLAG_ONE_SHOT),
                            IconCompat.createWithResource(context,
                                    R.drawable.icon),
                            ListBuilder.ICON_IMAGE,
                            "Open App")


                }
                "/about" -> { // ABOUT URI

                    // set data in intent
                    pendingIntent.putExtra(PAGE_KEY, ScreenNameEnum.ABOUT.name)

                    // Create pending intent with SMALL_IMAGE size
                    return SliceAction.create(
                            PendingIntent.getActivity(
                                    context,
                                    System.currentTimeMillis().toInt(),
                                    pendingIntent,
                                    PendingIntent.FLAG_ONE_SHOT),
                            IconCompat.createWithResource(context,
                                    R.drawable.small),
                            ListBuilder.SMALL_IMAGE,
                            "Open App")

                }
                "/contact" -> {

                    // set data in intent
                    pendingIntent.putExtra(PAGE_KEY, ScreenNameEnum.CONTACT.name)

                    // Create pending intent with LARGE image size
                    return SliceAction.create(
                            PendingIntent.getActivity(
                                    context,
                                    System.currentTimeMillis().toInt(),
                                    pendingIntent,
                                    PendingIntent.FLAG_ONE_SHOT),
                            IconCompat.createWithResource(context,
                                    R.drawable.large),
                            ListBuilder.LARGE_IMAGE,
                            "Open App")


                }
                else -> { // ANYTHING ELSE
                    pendingIntent.putExtra(PAGE_KEY, ScreenNameEnum.OTHER.name)


                    // Default with app icon
                    return SliceAction.create(
                            PendingIntent.getActivity(
                                    context,
                                    System.currentTimeMillis().toInt(),
                                    pendingIntent,
                                    PendingIntent.FLAG_ONE_SHOT),
                            IconCompat.createWithResource(context,
                                    R.mipmap.ic_launcher),
                            ListBuilder.LARGE_IMAGE,
                            "Open App")
                }
            }
        }

__RESULT:__ 

<img src="https://github.com/hiteshsahu/Android-Slices-Demo/blob/master/art/slice_home.png" width="80%">
        