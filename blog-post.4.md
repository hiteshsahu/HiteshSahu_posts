


> Minimal Recycler view ready to use Kotlin template for:

 - Vertical layout
 - A single TextView on each row
 - Responds to click events (Single and LongPress)


 Add a recycle view in your layout



       <android.support.v7.widget.RecyclerView
                android:id="@+id/wifiList"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
               /> 



Create a layout to display list items (list_item.xml)


     
     <?xml version="1.0" encoding="utf-8"?>
    <android.support.v7.widget.CardView
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
            <LinearLayout
                android:padding="5dp"
                android:layout_width="match_parent"
                android:orientation="vertical"
                android:layout_height="wrap_content">
        
                <android.support.v7.widget.AppCompatTextView
                    android:id="@+id/ssid"
                    android:text="@string/app_name"
                    android:layout_width="match_parent"
                    android:textSize="17sp"
                    android:layout_height="wrap_content" />
                
          </LinearLayout>
         </android.support.v7.widget.CardView>




Now create a minimal Adapter to hold data, code here is self explanatory

        class WifiAdapter(private val wifiList: ArrayList<ScanResult>) : RecyclerView.Adapter<WifiAdapter.ViewHolder>() {
        
            // holder class to hold reference
            inner class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
                //get view reference
                var ssid: TextView = view.findViewById(R.id.ssid) as TextView
            }
        
            override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
                // create view holder to hold reference
                return ViewHolder( LayoutInflater.from(parent.context).inflate(R.layout.list_item, parent, false))
            }
        
            override fun onBindViewHolder(holder: ViewHolder, position: Int) {
                //set values
                holder.ssid.text =  wifiList[position].SSID
            }
        
            override fun getItemCount(): Int {
                return wifiList.size
            }
            // update your data
            fun updateData(scanResult: ArrayList<ScanResult>) {
                wifiList.clear()
                notifyDataSetChanged()
                wifiList.addAll(scanResult)
                notifyDataSetChanged()
            }
        }





Add this class to handle Single click and long click events on List Items


            import android.content.Context;
            import android.support.v7.widget.RecyclerView;
            import android.view.GestureDetector;
            import android.view.MotionEvent;
            import android.view.View;
            public class RecyclerTouchListener implements RecyclerView.OnItemTouchListener {
            
                public interface ClickListener {
                    void onClick(View view, int position);
            
                    void onLongClick(View view, RecyclerView recyclerView, int position);
            
                }
                private GestureDetector gestureDetector;
                private ClickListener clickListener;
            
                public RecyclerTouchListener(Context context, final RecyclerView recyclerView, final ClickListener clickListener) {
                    this.clickListener = clickListener;
                    gestureDetector = new GestureDetector(context, new GestureDetector.SimpleOnGestureListener() {
                        @Override
                        public boolean onSingleTapUp(MotionEvent e) {
                            return true;
                        }
            
                        @Override
                        public void onLongPress(MotionEvent e) {
                            View child = recyclerView.findChildViewUnder(e.getX(), e.getY());
                            if (child != null && clickListener != null) {
                                clickListener.onLongClick(child,recyclerView,  recyclerView.getChildPosition(child));
                            }
                        }
                    });
                }
            
                @Override
                public boolean onInterceptTouchEvent(RecyclerView rv, MotionEvent e) {
                    View child = rv.findChildViewUnder(e.getX(), e.getY());
                    if (child != null && clickListener != null && gestureDetector.onTouchEvent(e)) {
                        clickListener.onClick(child, rv.getChildPosition(child));
                    }
                    return false;
                }
            
                @Override
                public void onTouchEvent(RecyclerView rv, MotionEvent e) {
            
                }
            
                @Override
                public void onRequestDisallowInterceptTouchEvent(boolean disallowIntercept) {
            
                }


   Lastly Set your adapter to Recycler View and add Touch Listener to start intercepting touch event for single or double tap on list items


        wifiAdapter = WifiAdapter(ArrayList())

        wifiList.apply {
            // vertical layout
            layoutManager = LinearLayoutManager(applicationContext)
            // set adapter
            adapter = wifiAdapter

            // Touch handling
            wifiList.addOnItemTouchListener(RecyclerTouchListener(applicationContext, wifiList, object : RecyclerTouchListener.ClickListener {
                override fun onClick(view: View?, position: Int) {
                    Toast.makeText(applicationContext, "RV OnCLickj " + position, Toast.LENGTH_SHORT).show()
                }

                override fun onLongClick(view: View, recyclerView: RecyclerView, position: Int) {
                    Toast.makeText(applicationContext, "RV OnLongCLickj " + position, Toast.LENGTH_SHORT).show()
                }
            }
            ))
        }
        

> Bonus : Update Data
    

    wifiAdapter.updateData(mScanResults as ArrayList<ScanResult>)
     

>Result:


<img src="https://i.stack.imgur.com/qOYLd.pngw=500" alt="RecyclerView" style="width:70%;"/>


