# Android4.2
package com.example.pankaj.adapter42;


    public class Display extends Activity implements OnItemClickListener{


    List<String> name1 = new ArrayList<String>();
    List<String> phno1 = new ArrayList<String>();
    MyAdapter ma ;
    Button select;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.display);

        getAllContacts(this.getContentResolver());
        ListView lv= (ListView) findViewById(R.id.lv);
        ma = new MyAdapter();
        lv.setAdapter(ma);
        lv.setOnItemClickListener(this);
        lv.setItemsCanFocus(false);
        lv.setTextFilterEnabled(true);
        // adding
        select = (Button) findViewById(R.id.button1);
        select.setOnClickListener(new OnClickListener()
        {

            @Override
            public void onClick(View v) {
                StringBuilder checkedcontacts= new StringBuilder();
                System.out.println(".............."+ma.mCheckStates.size());
                for(int i = 0; i < name1.size(); i++)

                {
                    if(ma.mCheckStates.get(i)==true)
                    {
                        checkedcontacts.append(name1.get(i).toString());
                        checkedcontacts.append("\n");

                    }
                    else
                    {
                        System.out.println("Not Checked......"+name1.get(i).toString());
                    }


                }

                Toast.makeText(Display.this, checkedcontacts,1000).show();
            }
        });


    }
    @Override
    public void onItemClick(AdapterView<?> arg0, View arg1, int arg2, long arg3) {
        // TODO Auto-generated method stub
        ma.toggle(arg2);
    }

    public  void getAllContacts(ContentResolver cr) {

        Cursor phones = cr.query(ContactsContract.CommonDataKinds.Phone.CONTENT_URI, null,null,null, null);
        while (phones.moveToNext())
        {
            String name=phones.getString(phones.getColumnIndex(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME));
            String phoneNumber = phones.getString(phones.getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER));
            System.out.println(".................."+phoneNumber);
            name1.add(name);
            phno1.add(phoneNumber);
        }

        phones.close();
    }
    class MyAdapter extends BaseAdapter implements CompoundButton.OnCheckedChangeListener
    {  private SparseBooleanArray mCheckStates;
        LayoutInflater mInflater;
        TextView tv1,tv;
        CheckBox cb;
        MyAdapter()
        {
            mCheckStates = new SparseBooleanArray(name1.size());
            mInflater = (LayoutInflater)Display.this.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        }
        @Override
        public int getCount() {
            // TODO Auto-generated method stub
            return name1.size();
        }

        @Override
        public Object getItem(int position) {
            // TODO Auto-generated method stub
            return position;
        }

        @Override
        public long getItemId(int position) {
            // TODO Auto-generated method stub

            return 0;
        }

        @Override
        public View getView(final int position, View convertView, ViewGroup parent) {
            // TODO Auto-generated method stub
            View vi=convertView;
            if(convertView==null)
                vi = mInflater.inflate(R.layout.row, null);
            TextView tv= (TextView) vi.findViewById(R.id.textView1);
            tv1= (TextView) vi.findViewById(R.id.textView2);
            cb = (CheckBox) vi.findViewById(R.id.checkBox1);
            tv.setText("Name :"+ name1.get(position));
            tv1.setText("Phone No :"+ phno1.get(position));
            cb.setTag(position);
            cb.setChecked(mCheckStates.get(position, false));
            cb.setOnCheckedChangeListener(this);

            return vi;
        }
        public boolean isChecked(int position) {
            return mCheckStates.get(position, false);
        }

        public void setChecked(int position, boolean isChecked) {
            mCheckStates.put(position, isChecked);
            System.out.println("hello...........");
            notifyDataSetChanged();
        }

        public void toggle(int position) {
            setChecked(position, !isChecked(position));
        }
        @Override
        public void onCheckedChanged(CompoundButton buttonView,
                                     boolean isChecked) {
            // TODO Auto-generated method stub

            mCheckStates.put((Integer) buttonView.getTag(), isChecked);
        }
    }
}
------------
package com.example.pankaj.adapter42;

import android.app.Activity;
import android.database.Cursor;
import android.net.Uri;
import android.os.Bundle;
import android.provider.ContactsContract;
import android.util.Log;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.EditText;
import android.widget.ListView;
import android.widget.SimpleCursorAdapter;

import com.google.android.gms.appindexing.Action;
import com.google.android.gms.appindexing.AppIndex;
import com.google.android.gms.appindexing.Thing;
import com.google.android.gms.common.api.GoogleApiClient;

public class MainActivity extends Activity implements OnClickListener {

    ListView list;
    Cursor cursor;
    SimpleCursorAdapter adapter;
    String phoneNumber;
    Button AddNumber;
    EditText number1;
    EditText number2;
    EditText number3;
    String contact1;
    String contact2;
    CheckBox checkBox;
    String contact_name;

    static final String TAG = "In AddContacts";
    /**
     * ATTENTION: This was auto-generated to implement the App Indexing API.
     * See https://g.co/AppIndexing/AndroidStudio for more information.
     */
    private GoogleApiClient client;

    @SuppressWarnings("deprecation")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        AddNumber = (Button) findViewById(R.id.AddNumbers);
        number1 = (EditText) findViewById(R.id.NumberField1);
        AddNumber.setOnClickListener(this);
        list = (ListView) findViewById(R.id.listMode);


        Log.d(TAG, "onCreate");


        Cursor cursor = getContentResolver().query(
                ContactsContract.CommonDataKinds.Phone.CONTENT_URI, null, null,
                null, null);

        String[] nameNumberArray = new String[cursor.getCount()];
        Log.d("rows", Integer.toString(cursor.getCount()));

        Log.d("phone", "here");
        while (cursor.moveToNext()) {
            String phoneNumber =

                    cursor.getString(cursor
                            .getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER));
            String name = cursor.getString(cursor
                    .getColumnIndex(ContactsContract.Contacts.DISPLAY_NAME));
            Log.i("Number + Name", phoneNumber + name);
        }
        if (cursor.getCount() > 0) {

            int i = 0;
            if (cursor.moveToFirst()) {
                do {
                    phoneNumber = cursor
                            .getString(cursor
                                    .getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER));

                    contact_name = cursor
                            .getString(cursor
                                    .getColumnIndex(ContactsContract.Contacts.DISPLAY_NAME));
                    nameNumberArray[i] = contact_name + "  , " + phoneNumber;
                    i++;
                } while (cursor.moveToNext());
            }

        }

        final String[] FROM = {nameNumberArray[0], nameNumberArray[1]};
        final int[] TO = {R.id.contact_name, R.id.phone_number,};
        adapter = new SimpleCursorAdapter(this, R.layout.row, cursor, FROM, TO);
        list.setAdapter(adapter);
        cursor.close();
        // ATTENTION: This was auto-generated to implement the App Indexing API.
        // See https://g.co/AppIndexing/AndroidStudio for more information.
        client = new GoogleApiClient.Builder(this).addApi(AppIndex.API).build();
    }

    @Override
    public void onClick(View v) {


        Log.d(TAG, "onClicked method");


    }

    /**
     * ATTENTION: This was auto-generated to implement the App Indexing API.
     * See https://g.co/AppIndexing/AndroidStudio for more information.
     */
    public Action getIndexApiAction() {
        Thing object = new Thing.Builder()
                .setName("Main Page") // TODO: Define a title for the content shown.
                // TODO: Make sure this auto-generated URL is correct.
                .setUrl(Uri.parse("http://[ENTER-YOUR-URL-HERE]"))
                .build();
        return new Action.Builder(Action.TYPE_VIEW)
                .setObject(object)
                .setActionStatus(Action.STATUS_TYPE_COMPLETED)
                .build();
    }

    @Override
    public void onStart() {
        super.onStart();

        // ATTENTION: This was auto-generated to implement the App Indexing API.
        // See https://g.co/AppIndexing/AndroidStudio for more information.
        client.connect();
        AppIndex.AppIndexApi.start(client, getIndexApiAction());
    }

    @Override
    public void onStop() {
        super.onStop();

        // ATTENTION: This was auto-generated to implement the App Indexing API.
        // See https://g.co/AppIndexing/AndroidStudio for more information.
        AppIndex.AppIndexApi.end(client, getIndexApiAction());
        client.disconnect();
    }
}}
------------
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity" >


    <TextView
        android:text="TextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_alignParentStart="true"
        android:layout_marginStart="18dp"
        android:id="@+id/textView" />

    <EditText
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/textView"
        android:layout_alignParentStart="true"
        android:layout_marginTop="21dp"
        android:id="@+id/NumberField1" />

    <Button
        android:text="AddNumbers"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/NumberField1"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="118dp"
        android:id="@+id/AddNumbers" />


</RelativeLayout>
--------------------
