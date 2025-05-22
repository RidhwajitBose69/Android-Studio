package com.example.sharedpref;

import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;

import android.os.Handler;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;


public class MainActivity extends AppCompatActivity {


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            long delayMillis = 4000; // 4 second delay

             new Handler().postDelayed(new Runnable() {// Handle delay to next intent
                @Override
                public void run() {
                    SharedPreferences pref = getSharedPreferences("login", MODE_PRIVATE);
                    Boolean check = pref.getBoolean("flag", false);// store key as false default if check is true move to home activity
                    Intent iNext;
                    if (check) {
                        iNext = new Intent(MainActivity.this, HomeActivity.class);
                    } else {
                        iNext = new Intent(MainActivity.this, LoginActivity.class);
                    }
                    startActivity(iNext);
                    finish();
                }
            }, delayMillis);
            return insets;
        });
    }

    }
