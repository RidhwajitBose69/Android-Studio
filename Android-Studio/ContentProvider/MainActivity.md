package com.example.contentprovider;

import android.Manifest;
import android.content.ContentResolver;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.net.Uri;
import android.os.Bundle;
import android.provider.ContactsContract;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

public class MainActivity extends AppCompatActivity {

    private static final int REQUEST_CONTACTS_PERMISSION = 1;
    private TextView contactsTextView;
    private Button contactsButton;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main); // Change to your main layout
        contactsTextView = findViewById(R.id.contactsTextView);
        contactsButton = findViewById(R.id.contactsButton);

        contactsButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                requestContacts();
            }
        });
    }

    private void requestContacts() {
        if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_CONTACTS) != PackageManager.PERMISSION_GRANTED) {// This check if app already has permission or not
            ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.READ_CONTACTS}, REQUEST_CONTACTS_PERMISSION);
        } else {
            readContacts();
        }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        if (requestCode == REQUEST_CONTACTS_PERMISSION) {
            if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {// grantresults is array check wheather it has elements user responded to it
                readContacts();// then this function is called
            } else {
                Toast.makeText(this, "Permission denied", Toast.LENGTH_SHORT).show();
            }
        }
    }

    private void readContacts() {
        ContentResolver contentResolver = getContentResolver();// allow permission retreive data
        Uri uri = ContactsContract.CommonDataKinds.Phone.CONTENT_URI;//
        Cursor cursor = contentResolver.query(uri, null, null, null, null);// performs an querey

        if (cursor != null && cursor.getCount() > 0) {
            StringBuilder contacts = new StringBuilder();// class to build strings by appending
            while (cursor.moveToNext()) {
                String name = cursor.getString(cursor.getColumnIndexOrThrow(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME));//  retrieves values as string
                String phoneNumber = cursor.getString(cursor.getColumnIndexOrThrow(ContactsContract.CommonDataKinds.Phone.NUMBER));
                contacts.append("Name: ").append(name).append(", Phone: ").append(phoneNumber).append("\n");// appending the values
            }
            cursor.close();
            contactsTextView.setText(contacts.toString());// set textview to tet we retreived

        } else {
            contactsTextView.setText("No contacts found.");
        }

    }
}





